# Debugging

## Debuggin strategies + tactics
1. Don't debug! It's an inefficient way to find fix bugs <br />
    a) Prevent bugs from happening, when possible ---> static + <br />
    b) Make them easy to find when they do happen ---> dynamic checking <br />

2. Test cases (TDD, Test Driven Development) <br />
    * remember to maintain the tests! <br />

3. Use alternative platform (like Rust, Java, Python, but not C/C++) <br />

4. Defensive Programming <br />

### Defensive Techniques
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