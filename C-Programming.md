# C Programming (Low Level Programming)
    White House says stop writing C program, and other not memory safe language 
    and intead use memory safe language
    like: no subscript error; *(char *) to nullptr; access free memory
> suggetsing language like Rust <br />
    however, if you use Rust with the keyword "unsafe", it is still "dangarous"

We are just writing low level code closed to bare machine

## C versus C++
C++ Not in C:
1. classess: 
    - polymorphism (you know exactly what a function does when you call it)
    - encapsulation (not much)
    - inheritance
2. structs with static data members, functions 
3. namespaces
4. overloading (just one function)
5. exception handling
6. builtin memory allocation (no `new`, `delete` operators)
7. cin, cout

in C:
- static vs. externerl name
- malloc() free()
- printf() scanf()...putchar()

> example of mallor: <br />
    ```c
    void* malloc(size_t n_bytes);
    *q = ...
    *p = malloc(sizeof *q);
    ```
>
> basically, `malloc()` does not care what kind of memory your are asking for

## Compilation (before execution)
```c
gcc -E foo.c >foo.i (just run the preprocessing)     (preprocessing)
gcc -S foo.i (generate assembling language)          (compiling)
gcc -c foo.s (generate .o file, run the assembling)  (assmbling)
gcc foo.o -o foo (generates executable file)         (linking)
```

- in foo.i, a **C file** <br />
    it just runs the macro part, basically the same as source code but replace macron parts. <br/>
     from:
     ```c
     #define X 2
     f(X)
     ```
     to
     ```c
     f(2)
     ```

- `gcc -S foo.i` generates `foo.s` <br />
    like:
    ```
    movq $27, %ordi
    call f
    movl %eax, %ebx
    ```
    which is not really machine code but a text file

- `gcc -c foo.s` generates `foo.o` <br />
    which is a `foo.s` but in binary format `foo.o` (hexidecimal)
    ```
    FC|D1|1B
    F1|?
    FC|O3|AB
    ```
    but we cannot run it still due to the `?` because we don't know where it is ddefined

- `gcc foo.o -o foo` generates the executable file <br />
    which fills the question mark part in `foo.s` and using library files <br />
    the CPU can only execute structures in the RAM but not in the file (secondary-storage)