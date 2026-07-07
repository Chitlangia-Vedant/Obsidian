# [Compiling](https://cs50.harvard.edu/x/2024/notes/2/#compiling)

- _Encryption_ is the act of hiding plain text from prying eyes. _decrypting_, then, is the act of taking an encrypted piece of text and returning it to a human-readable form. q

- _VS Code_, the programming environment provided to you as a CS50 student, utilizes a compiler called `clang` or _c language_.
- If you were to type `make hello`, it runs a command that executes clang to create an output file that you can run as a user.
- VS Code has been pre-programmed such that `make` will run numerous command line arguments along with clang for your convenience as a user.
- You can attempt to enter into the terminal window: `clang -o hello hello.c`. You will be met by an error that indicates that clang does not know where to find the `cs50.h` library.
- Attempting again to compile this code, run the following command in the terminal window: `clang -o hello hello.c -lcs50`. This will enable the compiler to access the `cs50.h` library.
- Running in the terminal window `./hello`, your program will run as intended.
- While the above is offered as an illustration, such that you can understand more deeply the process and concept of compiling code, using `make` in CS50 is perfectly fine and the expectation!

## Compiling involves major steps, including the following:

  
First, _preprocessing_ is where the header files in your code, designated by a `#` (such as `#include <cs50.h>`) are effectively copied and pasted into your file. During this step, the code from `cs50.h` is copied into your program. Similarly, just as your code contains `#include <stdio.h>`, code contained within `stdio.h` somewhere on your computer is copied to your program.

Second, _compiling_ is where your program is converted into assembly code.

Third, _assembling_ involves the compiler converting your assembly code into machine code.
  
Finally, during the _linking_ step, code from your included libraries are converted also into machine code and combined with your code.

# [Command-Line Arguments](https://cs50.harvard.edu/x/2024/notes/2/#command-line-arguments)

- `Command-line arguments` are those arguments that are passed to your program at the command line. For example, all those statements you typed after `clang` are considered command line arguments. You can use these arguments in your own programs!

- Modify your code as follows:
    
    ```c
    #include <cs50.h>
    #include <stdio.h>
    
    int main(int argc, string argv[])
    {
        if (argc == 2)
        {
            printf("hello, %s\n", argv[1]);
        }
        else
        {
            printf("hello, world\n");
        }
    }
    ```
    
    Notice that this program knows both `argc`, the number of command line arguments, and `argv` which is an array of the characters passed as arguments at the command line.
    
- Therefore, using the syntax of this program, executing `./greet David` would result in the program saying `hello, David`.

