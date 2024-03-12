# Debugging & Debugger Tools

## Debugging strategies + tactics
1. Don't debug! It's an inefficient way to find fix bugs <br />
    a) Prevent bugs from happening, when possible ---> static + <br />
    b) Make them easy to find when they do happen ---> dynamic checking <br />

2. Test cases (TDD, Test Driven Development) <br />
    * remember to maintain the tests! <br />

3. Use alternative platform (like Rust, Java, Python, but not C/C++) <br />

4. Defensive Programming <br />

### Defensive Programming Techniques
1.  **Runtime checking of your own** <br />
2.  **Assertions**: <br />
    ```c
    #include <assert.h>
    ...
    assert(0<=i && i<N)
    ...
    ```
    > The expression inside parenthesis can be anything you like but don't have side-effect <br />
    If the expression above returns FALSE, the program will print out a message and crash <br />

3. **Exception handling:** <br />
- This is built in `#include <setjmp.h>`
- This is usually not considered a good behaviour in C

4. **Traces & logs**
- `strace` in linux 

5. **Checkpoint + restart**
- `checkpoint();`, takes the entire state of the program in RAM in binary form.
- `restart(f)`, takes the entire state of the program outof a binary file and put it to program
    > With this approach, we don't have to re-run the whole program from 0 <br />
    Intead, we can `restart(f)` from the most recent `checkpoint()`

6. **Barricades** <br />
say we have a big messy program that talks to outside world, and it is attacked by being sent **bad data**<br />

-   The focus of barricades:
    -   clean up the core (which is relatively small than the messy part)
    -   assuming we have reliable data structure (organzied and cleaned up)
    -   we have a barricade between the core and the messy part that uses api to check incoming data and exporting data.
    >   As time increases, we should try to migrate the barricade <br />
    >   Firewall is hardware approach, and barricade is a software approach

7. **Interpreter** <br />
-   The basic idea here is like a barricade, but we have **bad code** this time
-   Let's say we have a Chrome browser, and have bad code as JavaScript and Chrome has a JavaScript interpreter written in C++. <br />
    The interpreter takes in the JavaScript code and read it carefully, which is like a code barricade
    -   A common attack from bad JavaScript code is **time attack**
> Besides, plugins were written in C++ but now in JavaScript b/c memory issue

7. **Virtual Machines** <br />
-   An **interpreter** that exams **machine code**

## How to debug

### Aside terminology:
failure: program behaviour is wrong; User-visible problem <br />
fault: latent bug in program that could cause failure under certain circumstances <br />
error: mental mistake by the developer <br />
error --> fault --> failure 

And there is not standard what **bug** is referring to

### Debugging Principles
-   Don't guess! guessing doesn't scale to large program 
-   More systematic <br />
    1. Stablize the failure (reproduce the bug)
    2. Locate the failures cause/source
    3. Fix the bug 
    > It is pretty common that the first two steps being most time-consuming

## Debugger Tool
This is going to be organized around GDB <br />
Here are some reaons why GDB:
- low-level debugging (either CS 33, CS 35L, CS 111)
- lot of features (perhaps the oldest debugger still widely used)

However, there is another common tool: `print` statement <br />

### GDB
`GDB` is a program, which puts itself into your program<br />
> A process is a program in execution. <br />

There are two approaches to use debugger
-   Insolate `GDB` from your process (not the other way around, which is an one-way insolation) <br />
-   Another one is to put debugger and process together, (like we are doing a `print` statement)
    -   slightly less relaible but simplier 

A `GDB` can:
-   stop your process (not executing, frozen)
-   read your process memory + registers
-   write your memory + registers while frozen (like changing from x=27 to x=28)
-   continue your programs

#### **Getting Started**
```shell
gcc -g
gcc -g3 
```
> This help generatind debugging info in .o files & executables <br />
    debugging info contains: <br />
    -   where function, global variables are <br />
    -   what there types are

One thing to mention, when compile with flag `-O2`, i.e., optimization, program runs faster but it is harder to debug <br />
But with flag `-O0`, it is easier to debug but program runs slower <br />
However, we cannot debug with `-O0` and run with `-O2`, because which changes the machine code behaviour <br />

And `-Og` optimizes for debuggability, not as fast as `-O2`, but offer a choice to debug with optimization

