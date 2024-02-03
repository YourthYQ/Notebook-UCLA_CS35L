# Shell


## Baisc Commands in Shell

### 1. Navigating and Manipulating the File System
* `~` - Home directory

* `$ cd` - Changes the current directory
    * `$ cd ..` - moves one directory up the hierarchy

* `^D` - Exit the current shell session

* `$ command_name --help` - Displays the help documentation (for a quick overview)

* `$ man command_name` - Display the manual pages (for a comprehensive understanding) (including all options, nuances, and examples)

* `$ pwd` - Print the current working directory path

* `$ ls` - List **files** and **directories** in the current directory
    * `-a` (show all files, including hidden)
    * `-l` (show files in long listing format) (more details)
    * `-i` (display the inode number of each file)
    * `-d` (show directories themselves) (only list their names without listing their contents)
    * `-r` (reverse order)
    * `-t` (sort by modification time)
    * `-h` (show human-readable sizes)

* `$ mv` - Move files or directories from current location to elsewhere **OR** Rename a file
    * `$ mv file1 file2... PATH` (Moving)
    * `$ mv oldname.txt newname.txt` (Renaming)

* `$ rm` - Remove(delete) files
    * `-r` (Remove **recursively**) (Delete a directory and **all of its contents**, including all files and subdirectories)
    * `$ rmdir` - Remove directories (Safer than `$ rm -r`, will not accidentally delete files)

* `$ mkdir directory_name` - Create a new directory

* `$ touch file_name` - Create a new file if it does not exist **OR** Updates the timestamp of an existing file

### 2. Working with File Contents
* `$ cat file_name` - Display the content of a file

* `$ cat file1 >> file2` - Append the content of file1 to file2

* `$ cat file1 file2 > file3` - Concatenate file1 and file2, storing the result into file3

### 3. Finding and Processing Files
* `$ grep` - Search for patterns within files
    * `-E` ()
    * `-r` ()
    * `-i` ()

* `$ find PATH` - Search for files in a directory hierarchy
    * `-type f` ()
    * `-type d` ()
    ```shell
    grep -E '^[^e]+$' /usr/share/dict/linux.words | wc -l
    find . -name "*.txt" -mtime -7 -exec cat {} \;
    ```

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

* `$ seq number` - Generate a sequence of numbers

* `$ shuf` - Generate random permutations of input lines
    * `-e` `--echo` ()
    * `-n` `--input-range` ()
    * `-r` `--head-count` ()
    * `-l` `--repeat` ()

* `$ which command_name` - Show the full path of the command's executable (find where it is located)

* `$ chmod + file_name` - Give executable permission to a file (allowing it to be run as a script)

* `$ ln source_file link_name` - Create links between files
    * `-s` ()


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