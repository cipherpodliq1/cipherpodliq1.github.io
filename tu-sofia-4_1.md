# УПРАЖНЕНИЕ

## Task 1 

Given a list of movies, each represented by a tuple containing the movie name and rating, the goal is to organize this information into a dictionary where:
Keys are the ratings.
Values are lists of movie names with that rating.

1. Input a list of tuples in the form [(movie_name, rating), ...].
2. Create a dictionary where keys are ratings, and values are lists of movie names.
3. Ensure movies are sorted by rating and then alphabetically within each rating.

### Напреднали

Make the program work with the Open Movie Database -> [link here](https://www.omdbapi.com/)

## Task 2

Create a script to organize files in a specified directory by their file extensions. Each file should be moved to a folder named after its extension (e.g., .txt files go into a txt folder). If the folder doesn’t exist, the script should create it. 

Requirements:

Traverse a specified directory and list all files.
Sort files based on their extensions and move them to corresponding folders.
Handle potential errors gracefully (e.g., if the file is already in the correct folder).

Hints:

Use os for directory and file manipulation.
You can use os.path.splitext to extract file extensions.

### Напреднали

Create a script that first generates a set of random files with various extensions in a specified directory. Then, it organizes these files by moving them into folders named after their extensions. Additionally, for each file, save details like the file name, extension, size, and creation date to a CSV file.

Requirements:

Generate Files:

Generate a specified number of random files in a target directory.
Assign each file a random name, random extension, and random content.
Organize Files by Extension:

Traverse the target directory and list all files.
Move files into folders based on their extensions (e.g., .txt files go into a txt folder).
Create folders if they don’t already exist.
Log File Properties to CSV:

For each file, record its name, extension, size (in bytes), and creation date, last modification date.
Write this information to a file_log.csv file.
Handle Errors Gracefully:

Ensure the script can handle common errors, such as trying to move a file that’s already in the correct folder.


## Task 3

Description:
Write a script that generates a visual representation of a directory structure, similar to the tree command. It should display directories and files in a hierarchical format with proper indentation.

Requirements:

Recursively traverse directories and files from a given root path.
Print each directory and file with indentation based on depth.
Optionally, add parameters to show file sizes or to limit the depth of traversal.
Hints:

Use os.walk() to recursively traverse directories.
Consider using a recursive function to handle indentation.

Example Output:

```
root_folder
├── file1.txt
├── sub_folder1
│   ├── file2.txt
│   └── file3.txt
└── sub_folder2
    └── file4.txt
```
### Step 1: Start with Directory Traversal

Goal: List all files and directories in the specified root directory.

Tip: Use os.walk() to traverse directories and files. This function makes it easier because it yields a tuple for each 

### Step 2: Build a Recursive Function

Goal: Create a function that can display each directory and file with the correct indentation.

Tip: Use a parameter like depth to keep track of how “deep” you are in the directory structure. This way, you can print each line with the right amount of indentation.

### Step 3: Add Tree-Like Symbols for Visual Structure

Goal: Add symbols like ├── and └── to make it look more like a real directory tree.

Tip: For each item, check if it’s the last one in the list. If it is, use └──; otherwise, use ├──. Use │ to show a continuing line for subdirectories.

### Step 4: Add Options (Optional)

Show file sizes: Use os.path.getsize() to display file sizes next to each file name.

Limit the depth: Add a max_depth parameter and increase depth with each recursion, stopping if depth reaches max_depth.

## Task 4: File Backup System (Windows)

Description: Create a simple file backup system that copies files from a specified source directory to a destination directory on a Windows system, appending a timestamp to each copied file's name. The program should log the names of the copied files and the timestamp of each backup operation.

Requirements:

Select Source and Destination Directories: Use user input to specify where to copy files from and where to copy them to.

Copy Files: Use shutil.copy2() to copy files, maintaining their metadata.
Append Timestamp: Append the current timestamp to each copied file's name to prevent overwrites.
Log Backup Operations: Create a simple log file in the destination directory that records the names of copied files and their corresponding timestamps.

Hints:

Use the os library for directory manipulation.
Use shutil for copying files.
Use datetime to generate timestamps.
You can use os.listdir() to list files in the source directory.

Pseudocode:

```
FUNCTION backup_files(source_directory, destination_directory):
    SET timestamp = CURRENT_DATE_TIME in "YYYYMMDD_HHMMSS" format
    SET log_file_path = destination_directory + "/backup_log.txt"

    OPEN log_file_path in append mode AS log_file

    FOR each file_name IN LIST_FILES(source_directory):
        SET full_file_name = source_directory + "/" + file_name
        
        IF full_file_name is a file THEN:
            SET new_file_name = file_name WITHOUT_EXTENSION + "_" + timestamp + file_extension_of(file_name)
            SET destination_file_path = destination_directory + "/" + new_file_name
            
            COPY full_file_name TO destination_file_path
            WRITE "file_name backed up as new_file_name on timestamp" TO log_file
            PRINT "Backed up: file_name as new_file_name"

    CLOSE log_file

FUNCTION main():
    PRINT "Enter the source directory:"
    READ source_directory

    PRINT "Enter the destination directory:"
    READ destination_directory

    CALL backup_files(source_directory, destination_directory)

CALL main()
```

### Напреднали:

Develop a more sophisticated file backup system for Windows that not only backs up files but also allows for selective backups based on file types. The system should support a command-line interface for user interaction and maintain a detailed log of backup operations, including error handling for various scenarios (e.g., if a file cannot be copied).

Requirements:

Command-Line Interface: Use command-line arguments to specify source and destination directories and file types (extensions) to backup.

Selective Backup: Allow the user to specify which file types to backup (e.g., .txt, .jpg, etc.).

Error Handling: Gracefully handle potential errors (e.g., missing directories, permission issues) and log these errors.

Detailed Backup Log: Maintain a comprehensive log file that includes not just successful backups but also any errors encountered during the process.
