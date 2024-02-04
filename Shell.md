# Shell

`shell` refers to a **command-line interpreter** that provides a user interface for accessing an operating system's services.

In Unix and Linux, shells are the primary way for users to interact with the system via commands and scripts.

There are several types of shells, such as the **Bourne shell (sh)**, C shell (csh), Korn shell (ksh), and others.

## Operating Systems

An operating system (OS) is a software program that acts as an intermediary between computer hardware and the user, as well as between other software applications.

**`Hardware`** is the physical component of a computer system, including the CPU, GPU, RAM, storage devices, etc.

**`Kernel`** is the core component of the OS, acting as an intermediary between the harware and the software applications that run on it. It provides low-level services to the parts of the system and applications, and it controls all hardware interactions.

**`Operating System`** built around the kernel, includes additional components like the user interface (UI) and utility programs, providing a user-friendly environment and higher-level functionalities.

**`Software`** includes application software and system software. Application software consists of programs that perform specific tasks for users (e.g. word processors, web browsers). The operating system itself is considered system software, which provides a platform for application software to run.

* Ubuntu is an example of a Linux distro (distribution).
* Debian is the upstream distro for Ubuntu from which it inherits thousands of *packages*. You can can check the packages with `dpkg -l`.
* Linux is an OS **kernel**.

**Software levels of abstraction**

```
+------------------------+
|        software        |
+------------+-----------+
|            |   libs    |
|   kernel   +-----------+
|            | C stdlib  |
+------------+-----------+
|        hardware        |
+------------------------+
```

sh and Emacs are applications in themselves, one of many **applications** that sit on top of the operating system.


## Process ID (PID)
`PID` is a unique numerical identifier assigned by the OS to each process when it is created. This identifier allows the OS and users to manage and interact with specific processes, such as terminating a process, monitoring its resource usage, or tracing its activity.

### Access and Trace a PID

**View Processes**: `ps` reports a snapshot(简况) of current processes. To see all running processes, use `ps -e`.

**Find a Process by Name**: `pgrep` finds the PID of a process by name. For example, `pgrep nginx` finds the PID(s) of nginx processes.

**Trace Process Execution**: `strace` trace system calls made by a process. For example, `strace -p PID` traces the system calls of the process with the given PID.

**Kill a Process**: `kill PID` terminates a process. If the process does not terminate, `kill -9 PID` forces termination.

### Superuser and Concept of Privilege

* **Superuser** (root) are the only ones with permission to `kill` PID 1, `system`.
* The `sudo` command lets you run a command as "root":

```shell
sudo sh
```

<br>

> **Why have multiple users instead of just root?**
>
> The concept of **minimization of privileges** aka "principle of least privilege". If a program breaks or a user makes a mistake, the damage is limited.

> **Use only the `ps` command to find your own login shell’s process, all that process’s ancestors, and all its descendants.**
>
>1. Find Shell's Process ID**: First, we need to find the PID of current shell session, using the command of `echo $$` or `ps $$`.
>
> 2. Get Ancestors**: To get all ancestors of the shell process, we need to trace the parent process IDs (PPIDs) recursively until we reach the initial process (PID 1). We can do this by repeatedly using ps to find the PPID of each process, starting with the shell's PID. (Or scroll the screen to find)
>
> 3. Get Descendants**: We need to list all processes, let the PID of this process be the PPID of its descendant, and recursively until we reach the numerically largest PID.


## The `(ba)sh` Program

A shell is itself a program, and programs themselves are just files that can be executed by the operating system.

**sh** is the predecessor to **bash** (Bourne Again SH). sh was designed to work on 16-bit machines so it's a very little language. Bash adds some features in addition to the original sh.

There is also a lot of other shell languages ending in sh. Having so many distinct shell languages becomes a problem, so the **POSIX standard** was created as a spec for shells.

Creating another instance of the shell from within the shell itself to execute a one-off command:

```shell
sh -c <command>
```

This is what you call a **subprocess**, or a **child process**. Within your running instance of `sh` (the CLI you're typing into), another *instance* of `sh` is spawned as a child of that `sh`. In fact, every time you run a command, an instance of their little program is attached to your shell as a child process. You can see this for yourself when using commands like [`ps`](#the-ps-program) to show processes and their parentage.


### ASIDE: Some Mischievous Things


**Things you can do to annoy your system administrator*:*

Telling the shell to go into an infinite loop:

```shell
sh -c 'while true; do true; done'
```

A no-op instruction(无操作指令)(休眠) for a number of seconds, doesn't use CPU

```shell
sleep 10
```

The `truncate` command sets a certain file to a certain size, ending at the size you specify (filling with null data if the size expands I presume):

```shell
truncate --size=10TB bigfile
```

If you inspect the filesystem, you'll find that you didn't actually use up that much space, just convince the directory listings that that much space had been allocated for *something*. This is still annoying though because it will trip up the sysadmins when they perform **backups**.


## Baisc Commands in Shell

### 0. Find help and Manual
* `$ command_name --help` - Displays the help documentation (for a quick overview)

* `$ man command_name` - Display the manual pages (for a comprehensive understanding) (including all options, nuances, and examples)

* `$ info command_name` - 

### 1. Navigating and Manipulating the File System
* `~` - Home directory

* `$ cd` - Changes the current directory
    * `$ cd ..` - moves one directory up the hierarchy

* `^D` - Exit the current shell session

* `$ pwd` - Print the current working directory path

* `$ ls` - List **files** and **directories** in the current directory
    * `-a` (show all files, including hidden)
    * `-l` (show files in long listing format) (more details)
    * `-i` (display the inode number of each file)
    * `-d` (show directories themselves) (only list their names without listing their contents)
    * `-r` (reverse order)
    * `-t` (sort by modification time)
    * `-h` (show human-readable sizes)

* `$ mv` - Move files or directories from current location to another **OR** Rename a file (cheap)
    * `$ mv file1 file2... PATH` (Moving)
    * `$ mv oldname.txt newname.txt` (Renaming)

* `$ cp` - Copy files and directories (expensive)
    * `-r` (recursive copying when dealing with directories) (copy every item within the directory, including its sub-directories and all their contents)

* `$ rm` - Remove(delete) files
    * `-r` (Remove **recursively**) (Delete a directory and **all of its contents**, including all files and sub-directories)
    * `$ rmdir` - Remove directories (Safer than `$ rm -r`, will not accidentally delete files)

* `$ mkdir directory_name` - Create a new directory

* `$ touch file_name` - Create a new file if it does not exist **OR** Updates the timestamp of an existing file

### 2. Working with File Contents
* `$ cat file_name` - Display the content of a file

* `$ cat file1 >> file2` - Append the content of file1 to file2

* `$ cat file1 file2 > file3` - Concatenate file1 and file2, storing the result into file3

### 3. Searching Files

* `$ grep` - Search for patterns within files
    * `-E` (enable the use of **extended Regular Expressions**)
    * `-r` (recursively read all files under all directories and their sub-directories)
    * `-i` (ignore case) (make the search case-insensitive)

* `$ find PATH` - Search for files in a directory hierarchy
    * `-type f` (search only for files)
    * `-type d` (search only for directories)
    * `-name file_name` (search for files by name)
        * `-iname` (case-insensitively search for files by name)
    * `-size` (search for files by size)
        * `-size +50M` (search for files over 50 MB)
        * `-size -10K` (search for files under 10 KB)
    * `-exec command_name` (execute a command on the files found)
    * `-maxdepth / -mindepth number` (limit the search to a specific depth of directories)
    * `-delete` (delete the files found (use with caution))


    ```shell
    grep -E '^[^e]+$' /usr/share/dict/linux.words | wc -l
    ```
    > This command searches through the file /usr/share/dict/linux.words, which typically contains a list of English words, to find and count all lines (words) that do not contain the letter 'e'.

    > `grep` is the command used to search for patterns within files
    >
    > `-E` enables **extended regular expression** syntax, allowing more complex patterns
    >
    > `'^[^e]+$'` is an **extended regular expression**:
    > * `^` asserts the start of a line
    > * `[^e]+` matches one or more characters (+) that are not (^ when used inside []) the letter 'e'
    > * `$` asserts the end of a line
    >
    > `|` pass the output of the previous command as input to the next command
    >
    > `wc -l` stands for **word count**, and the `-l` option tells it to count lines


    ```shell
    find . -name "*.txt" -mtime -7 -exec cat {} \;
    ```
    > This command searches for and displays the contents of all .txt files in the current directory and its subdirectories that were modified in the last 7 days.
    
    > `find` is the command used to search for files in a directory hierarchy
    >
    > `-name "*.txt"` tells `find` to match files whose names end with .txt.
    >
    > `-mtime -7`
    > * `mtime` is the **modification time** of the files
    > * `-7` specifies files modified **less than 7 days ago**
    >
    > `-exec cat {} \;`
    >
    > * `-exec command_name` tells `find` to execute a command on each file found
    > * `cat` displays the contents of files
    > * `{}` a placeholder for the file name found by `find`
    > * `\;` marks the end of the command to be executed

### 4. File Compression and Archiving
* `$ tar`
    * `-c` (create a new archive)
    * `-z` (**compress or decompress** the archive using 'gzip')
    * `-v` (list the files processed) (v stands for Verbose mode)
    * `-f` (specify the filename of the archive)
    * `-x` (extract files from an archive)

* `$ tar -czf tgz_file_name file1 file2...` - Create a compressed archive named `tgz_file_name` using gzip and put `file1 file2...` into it

* `$ tar -xzf tgz_file_name` - Extract files from a compressed archive using gzip

    ```shell
    tar -czv -f filename a b c
    tar -czv -f=filename a b c
    tar -czv --file=filename a b c
    # these three commands do the same thing
    ```

### 5. Miscellaneous Commands (some others)
* `$ tree` - Display the directory structure in a tree-like format

* `$ seq` - Generate a sequence of numbers
    * `$ seq 5` (generate numbers from 1 to 5)
    * `$ seq 2 10` (generate numbers from 2 to 10)
    * `$ seq 0 2 10` (generate numbers from 0 to 10, increment by 2)

* `$ shuf` - Generate random permutations of input lines
    * `-e arg1 arg2...` `--echo` (treat each command-line argument as an input line, and output with a random permutation)
    * `-n number` `--head-count` (display any n random lines from the file)
    * `-i LO-HI` `--input-range` (output a random permutation of the numbers from LO to HI)
    * `-r` `--repeat`

* `$ which command_name` - Show the full path of the command's executable (find where it is located)

* `$ chmod + file_name` - Give executable permission to a file (allowing it to be run as a script)

* `$ ln source_file link_name` - Create hard links between files
    * `-s` (use -s if you want to create a symlink)

* `$ ps` - displays only the processes running in the current shell (associated with the terminal session currently using)
    * `-e` (display every process running around the whole system)
    * `-f` (a full-format listing) (includes UID PID PPID)
    * `-H` (displays the process hierarchy like a tree-like format)

* `$ kill PID#` - terminate a process

* `$ shred file_name` - overwrite a file to erase its contents (wear out the actual data on the drive)

## File Expansion

### 1. Home Directory (‘~’)
When used alone, ~ expands to the current user's home directory

```shell
cd ~  
# Changes the directory to the user's home directory.
```

### 2. Specific User’s Home Directory ('~ username')
```shell
cd ~ john 
# Changes the directory to John's home directory.
```

### 3. Relative Paths (‘~/’)
When followed by a slash, ~ expands to the current user's home,  directory and then appends the specified path

```shell
ls ~/Desktop  
# Lists the contents of the Desktop directory in the user's home directory.
```


## Field Splitting
Field splitting is the process where the shell splits the results of expansions (like parameter expansion, arithmetic expansion, and command substitution) into words. 

* This process is guided by the value of the IFS (Internal Field Separator) variable (内部字段分隔符)
* IFS determines the characters used as word delimiters (分隔符) during field splitting. (IFS决定字段分割时用作单词分隔符的字符)
* Its default value includes space, tab, and newline, which means that by default, words are separated by these characters
* We can change the value of IFS to control how the shell splits the results of expansions. For instance, setting IFS to a colon (:) will make the shell use colons as delimiters (分隔符).

> Example 1: Changing IFS <br>
Here, if you echo $PATH after setting IFS to :, the shell will treat each entry in PATH as a separate word, splitting the string at every colon.

```bash
IFS=':'  # Setting IFS to colon
echo $PATH
```

> Example 2: Saving Original IFS <br>
It's a common and good practice to save the original value of IFS before changing it, and restore it afterward.

```bash
old_IFS=$IFS        # Save the original IFS
IFS=':'       		# Change IFS
# ... operations with custom IFS ...
IFS=$old_IFS  	    # Restore original IFS
```


## Pathname Expansion (Globbing)
Pathname expansion allows you to use wildcard (通配符) characters to match filenames and paths, which is extremely useful for operating on multiple files or directories with similar names or patterns.

Key Wildcards in Globbing:

* `*` - matching any string of characters
    ```bash
    ls *.txt  
    # Lists all files ending with .txt in the current directory
    ```

* `?` - matching any single character
    ```bash
    ls file?.txt  
    # Matches file1.txt, file2.txt, etc., but not file10.txt

    ls file??.txt 
    # Matches file10.txt, file11.txt, etc., but not file1.txt, or file.100.txt
    ```

* `[...]` - matching any one of the enclosed characters. A range of characters can be specified with a hyphen(-) (连字符)
    ```bash
    ls file[1-3].txt
    # Matches file1.txt, file2.txt, and file3.txt

    ls file[!1-3].txt # ! means 'NOT'
    # Matches file4.txt, but not file1-3.txt
    ```


## Redirection
Redirection in the shell allows us to control where the input and output of commands go. You can redirect the output of a command to a file, or use a file as the input for a command.

Common Redirection Operators:

* `0<` or `<` Redirects standard input from a file
    ```bash
    grep "Hello" < hello.txt  
    # Uses hello.txt as input for grep
    # uses the ‘grep’ to search for the string "Hello" in the file hello.txt
    ```

* `1>` or `>` Redirects standard output to a file (overwrites the file)
    ```bash
    echo "Hello, World!" > hello.txt  
    # Writes to hello.txt (overwrites)
    ```

* `2>` Redirects standard error to a file
    ```bash
    ls non_existing_dir 2> error.log
    # Redirects error message to error.log
    ```

* `&>` Redirects both standard output and standard error to a file
    ```bash
    ls good_dir &> output.log         
    # Redirects both output and error to output.log
    ```

* `>>` Redirects standard output to a file (appends to the file)
    ```bash
    echo "Welcome" >> hello.txt
    # Appends to hello.txt
    ```