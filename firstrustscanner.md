Lecture Code : [тук.](https://github.com/cipherpodliq1/Rust-Port-and-Subdomain-Scanner)

# Introduction

In this lecture we are going to build a rust port and subdomain scanner. Software that is used to map the attack surface of a particular targert , is called a [scanner](https://www.coresecurity.com/blog/top-14-vulnerability-scanners-cybersecurity-professionals). Port scanner, Vulnerability scanner , SQL Injection scanner ... and many others - they are used for one simple goal. To discover an [attack vector](https://www.fortinet.com/resources/cyberglossary/attackvector#:~:text=of%20Attack%20Vectors-,Attack%20Vector%20Definition,breach%2C%20or%20steal%20login%20credentials.) that you can begin your attack from. They automate the long and fastidious task that
the reconnaissance phase can be and prevent human errors (like forgetting a subdomain). 

But you also have to think about the fact that scanners (if not configured properly) are really noisy , and ofter get picked up by the target's defense systems. Another issue is that they often return incomplete or wrong data. So you have to confirm your findings.

We will start building a simple scanner, whose purpose is to find subdoomains of a target , and then scan for the most common ports on each of them. After that we will start making it more sophisticated.

# Error-Handling

Just because, I want to be sure that you understand a little bit about Rust's Error Handling.

Whether it be for libraries or for applications, errors in Rust are strongly-typed and most of the time represented as [enums](https://doc.rust-lang.org/book/ch06-00-enums.html) with one variant for each kind of error our library or program might encounter.

For libraries -> its better to use [thiserror](https://docs.rs/thiserror/latest/thiserror/index.html) crate.

For programs -> its better to use the [anyhow](https://docs.rs/anyhow/latest/anyhow/) crate.

```
use thiserror::Error;

 #[derive(Error, Debug, Clone)]
 pub enum Error {
 #[error("Usage: tricoder <kerkour.com>")]
 CliUsage,
 }
```
------
```
fn main()-> Result<(), anyhow::Error> {
 //...
 }
```

# Enumerating Subdomains

 We are going to use the API provided by "crt.sh",which can be queried by calling the following endpoint: https://crt.sh/?q=%25.[domain.com]&output=json" .

```
 pubfn enumerate(http_client: &Client, target:&str)->
 Result<Vec<Subdomain>, Error> { ↪
 let entries: Vec<CrtShEntry>= http_client
 .get(&format!("https://crt.sh/?q=%25.{}&output=json",target))
 .send()?
 .json()?;
 //cleananddedupresults
 let mutsubdomains: HashSet<String>= entries
 .into_iter()
 .map(|entry|{
 entry
 .name_value
 .split("\n")
 .map(|subdomain|subdomain.trim().to_string())
 .collect::<Vec<String>>()
 })
 .flatten()
 .filter(|subdomain: &String|subdomain != target)
 .filter(|subdomain: &String|!subdomain.contains("*"))
 .collect();
 subdomains.insert(target.to_string());
 let subdomains: Vec<Subdomain> =subdomains
 .into_iter()
 .map(|domain|Subdomain {
 domain,
 open_ports: Vec::new(),
 })
.filter(resolves)
 .collect();
 Ok(subdomains)
 }
```

The "?" in the code means -> If the called function returns an error , abort the current function and return the error.

# Port Scanning

Subdomains and IP range scanning , are an integral part of the "asset discovery phase". After that comes port scanning -> once you identify which servers are reachable via internet, you need to see what services are running on those servers. Scanning is a really large topic, which i will not be able to cover in this article. It is different for everyone , and depends on the results that you are looking for - to be more stealthy , to be faster , have more reliable results and so on...

There are loads of different techniques, but today we will stick with the "trying to open a TCP socket".

A socket is a type of internet pipe. The only difference that instead of water, this pipe is filled with data. For example , when you want to connect to a website, your local browser opens a socket to the website's server, and then all the data passes through (just like water). When a socket is open , it means that the server is ready to process the information. On the other hand , if its closed -> that means no service is listening on the given port. 

In this situation , its important that we use a timeout. Because we do not want to wait forever.

```
use crate::{
 common_ports::MOST_COMMON_PORTS_100,
 model::{Port, Subdomain},
 };
 use std::net::{SocketAddr, ToSocketAddrs};
 use std::{net::TcpStream, time::Duration};
 use rayon::prelude::*;

 pub fn scan_ports(mut subdomain: Subdomain)-> Subdomain {
 let socket_addresses: Vec<SocketAddr> = format!("{}:1024",
 subdomain.domain)
 .to_socket_addrs()
 .expect("port scanner: Creating socket address")
 .collect();
 if socket_addresses.len() == 0 {
 return subdomain;
 }
 subdomain.open_ports = MOST_COMMON_PORTS_100
 .into_iter()
 .map(|port| scan_port(socket_addresses[0], *port))
 .filter(|port| port.is_open) // filter closed ports
 .collect();
 subdomain
 }
 fn scan_port(mut socket_address: SocketAddr, port: u16)-> Port {
 let timeout = Duration::from_secs(3);
 socket_address.set_port(port);
 ↪
 let is_open = if let Ok(_) = TcpStream::connect_timeout(&socket_address,
 timeout) {
 true
 } else {
 false
 };
 Port {
 port: port,
 is_open,
 }
 }
```

But the problem is , that if we leave the scanner this way, it will be really slow. if all ports are closed, we are going to wait  **Number_of_scanned_ports * timeout seconds** .

# Multi-Threading

Fortunately for us , there is this thing called *threads*.

[Threads](https://www.geeksforgeeks.org/thread-in-operating-system/) are primitives provided by the Operating System (OS) that enable developers to use the hardware cores and threads of the cpu. In rust we can spawn threads with -> std::thread::spawn function.

![{CE2F32C2-AC1F-41B5-A931-9950A383D27B}](https://github.com/user-attachments/assets/15e08ff2-6d13-488d-87ac-51d90be5e351)

Each CPU thread can be seen as an independent worker: the workload can be split among the workers. This is especially important as today, due to the law of physics, processors have a hard time scaling up in terms of operations per second (GHz). For those of you that are interested -> this is mostly due to , thermal limites, power consumption , quantum tunneling and Physical Limits of Light Speed and Signal Propagation.
 
Instead, vendors increase the number of cores and threads. Developers have to adapt and design their programs to split the workload between the available threads instead of trying to do all the operations on a single thread, as they sooner or later reach the limit of the processor. With threads, we can split a big task into smaller sub-tasks that can be executed in parallel. In our situation, we will dispatch a task per port to scan. Thus, if we have 100 ports to scan, we will create 100 tasks. Instead of running all those tasks in sequence like we previously did, we are going to run them on multiple threads. If we have 10 threads, with a 3 seconds timeout, it may take up to 30 seconds ( 10 * 3 )to scan all the ports for a single host. If we increase this number to 100 threads, then we will be able to scan 100 ports in only 3 seconds

## Concurrency issues 

Using threads is not an easy win. Many developers fear the concurrency issues which come with multi-threading. Due to the unexpected behaviour, they are extremely hard to spot and debug.  Theycangoundetectedfora longtime, and then, oneday, simply because your system is handling more requests per second or because you upgraded your CPU, your application starts to behave strangely. The cause is almost always that a concurrency bug is hidden in your code.

 One of themost fabulous things aboutRust is that thanks to its ownershipsystem, the compiler guarantees our programs to be datarace free.

## Three causes of data races 
- Two or more pointers access the same data at the same time
- At least one of the pointers is being used to write data
- There is no mechanism to synchronize access to the data amongs threads (thread lock)

## Three rules of ownership
- Each value in rust has a variable thats called "owner"
- There can be only one owner at a time
- When the owner goes out of scope, the value will be dropped

## Two rules of references
- At any given time, you can have either one mutable reference or any number of immutable reference
- References must always be valid

Other concurrency errors you might face include -> deadlocks and race conditions.

# Adding multi-threading to our scanner.

  
