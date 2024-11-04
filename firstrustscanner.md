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
