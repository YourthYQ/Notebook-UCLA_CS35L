# Bash Scripting

`#!/bin/bash` <br>
.sh file (Bash script file) (Shell executable file)

## Comparison Operators

### 1. Numeria Comparison Operators
* `-eq` Equal to
* `-ne` Not equal to
* `-gt` Greater than
* `-lt` Less than
* `-ge` Greater than or equal to
* `-le` Less than or equal to

### 2. String Comparison Operators
* `=` Equal to
* `==` Equal to (same as `=`)
* `!=` Not equal to
* `<` Less than, in ASCII alphabetical order
    * Used as `[[ "$a" < "$b" ]]` or in a **test command** with escaped syntax: `[ "$a" \< "$b" ]`
* `>` Greater than
    * Used as `[[ "$a" > "$b" ]]` or in a **test command** with escaped syntax: `[ "$a" \> "$b" ]`
* `-z` String is null, that is, has zero length
    * Used as `[ -z "$a" ]`

### 3. File Test Operators
* `-e` File exists (regardless of type)
* `-f` File exists and is a regular file
* `-d` File exists and is a directory
* `-r` File exists and is readable
* `-w` File exists and is writable
* `-x` File exists and is executable
* `-s` File exists and is not empty


## Overall Structure

### 1. Conditional Statements (if, else, elif, fi)
Condition Evaluations
> `[ "$a" \> "$b" ]` **Test Command** with escaped syntax <br>
> `[[ "$a" > "$b" ]]` **Extended Test Command** without escaped syntax

* `[]` (Test Command) <br>
* `[[]]` (Extended Test Command) <br>
    * `==` `&&` `||` `=~`

```bash
# Check if a file exists
if [ -e "myfile.txt" ]; then
  	echo "myfile.txt exists."
else
    echo "myfile.txt does not exist."
fi
```


### 2. Loop Structures (for, while, until)
* For-loop
    ```bash
    # Print numbers from 1 to 5
    for i in {1..5}; do
        echo “Number $i”
    done
    ```
* While-loop
    ```bash
    # Repeat while a condition is true
    count = 1
    while [ $count -le 5 ]; do
        echo “Count: $count”
        count = $((count + 1))
    done
    ```

### 3. Functions
* In shell scripting, we don’t specify parameters inside parentheses while declaring, parameters are passed to the function when it is called instead.
* While calling the function great() below, we use ”$1” to get the parameters (“$1, $2, $3…” is positional parameters similar in Python). This “1” refers to the first argument passed to the function.
* “$0” is a special case: it refers to the name of the script itself, not a function parameter

```bash
# function declaration and definition
great() {
	echo “Hello, $1!”
}

# function calling
great “Alice”
```


## Variable Expansion

### 1. Basic Variable Expansion
Simply prefix the variable name with a `$`

```bash
name = “Alice”
echo “Hello, $name!”
```

### 2. Brace Expansion
Useful for when you need to clearly delimit the variable name

```bash
greeting="hello"
echo "${greeting}world"  
# Without braces, it would try to expand `greetingworld`
```


## Command Substitution
Command substitution, denoted by `$(command)`, allows you to use the output of a command as an argument of another command

```bash
files=$(ls)
echo "Files in the directory: $files"
```

```bash
echo "The current directory is: $(basename $(pwd))"
# pwd returns the current directory path, basename extracts the last part of that path (the current directory name), and echo prints it out.
```


## Arithmetic Expansion
Arithmetic Expansion allows for the evaluation of an arithmetic expression

* denoted by `$((expression))` 
* only support for integer
* common operator like +, -, *, /, %
* Variables inside the $(( )) are treated as integers and their values are used in the arithmetic operation.

```bash
a=10
b=5
sum=$((a + b))
echo "Sum: $sum"  # Outputs: Sum: 15

product=$((a * b))
echo "Product: $product"  # Outputs: Product: 50
```


## Special Characters & Quoting Mechanisms
Quoting in shell scripting is a critical aspect for controlling how the shell interprets special characters and whitespace

### 1. Special Characters in shell
* `$`, `$()`, `$(())` - Dollar Sign
* `\` - Backslash
* `‘’` - Single Quotes
* `“”` - Double Quotes
* `*` - Asterisk <br>
```ls *.txt```
* `?` - Question Mark <br>
```ls file?.txt```
* `;` - Semicolon <br>
```echo "Start"; ls; echo "End"``` <br>
```# Running multiple commands sequentially```
* `|` - Pipe <br>
```cat file.txt | grep "search term"``` <br>
``` # Used for piping the output of first command as input to later one ```
* `&` Ampersand
* `>`, `<` , `=`
* `()`, `[]`, `{}`

### 2. Quoting Mechanisms 
Some techniques to deal with **Special Characters**: `\`, `''`, `""`

1. Backslash `\` - Escape Character
    ```bash
    echo \$PATH  
    # Outputs: $PATH

    echo "\\" 
    # Outputs: \
    ```
    In these examples, the backslash is used to prevent the shell from interpreting $ as the start of a variable and \ as a special character backflash itself.

2. Single Quotes `''` - Strong Quoting
    ```bash
    echo '$HOME is where the heart is'  
    # Outputs: $HOME is where the heart is
    ```
    Here, $HOME is not expanded to the user's home directory, as it is enclosed in single quotes.

3. Double Quotes `""` - Partial Quoting
    ```bash
    echo "Your home directory is $HOME"  
    # Outputs: Your home directory is /home/username
    ```
    In this example, `$HOME` is expanded to the user's home directory, it will return the **Absolute Path** of HOME.
