## Computer Program
A **computer program** (also commonly called an **application**) is a set of instructions that the computer can perform in order to perform some task. The process of creating a program is called **programming**. 

Programmers typically create programs by producing **source code** (commonly shortened to **code**), which is a list of commands typed into one or more text files

## Standard Library
C++ comes with an extensive library called the **C++ Standard Library** (usually shortened to _standard library_) that provides a set of useful capabilities for use in your programs. One of the most commonly used parts of the C++ standard library is the _iostream library_, which contains functionality for printing text on a monitor and getting keyboard input from a user.

## Unbuffered  output
The opposite of buffered output is unbuffered output. With unbuffered output, each individual output request is sent directly to the output device.

Writing data to a buffer is typically fast, whereas transferring a batch of data to an output device is comparatively slow. Buffering can significantly increase performance by minimizing the number of slow transfers that need to be performed when there are multiple output requests.

## Return values and side effects

Most operators in C++ just use their operands to calculate a return value. For example, `-5` produces return value `-5` and `2 + 3` produces return value `5`. There are a few operators that do not produce return values (such as `delete` and `throw`). We’ll cover what these do later.

Some operators have additional behaviors. An operator (or function) that has some observable effect beyond producing a return value is said to have a **side effect**. For example, when `x = 5` is evaluated, the assignment operator has the side effect of assigning the value `5` to variable `x`. The changed value of `x` is observable (e.g. by printing the value of `x`) even after the operator has finished executing. `std::cout << 5` has the side effect of printing `5` to the console. We can observe the fact that `5` has been printed to the console even after `std::cout << 5` has finished executing.

Operators with side effects are usually called for the behavior of the side effect rather than for the return value (if any) those operators produce.



## Status Code
The C++ standard only defines the meaning of 3 status codes: 0, EXIT_SUCCESS, and EXIT_FAILURE. 0 and EXIT_SUCCESS both mean the program executed successfully. EXIT_FAILURE means the program did not execute successfully.
EXIT_SUCCESS and EXIT_FAILURE are preprocessor macros defined in the \<cstdlib> header:

```cpp
#include <cstdlib> // for EXIT_SUCCESS and EXIT_FAILURE
int main()
{
return EXIT_SUCCESS;
}
```

If you want to maximize portability, you should only use 0 or EXIT_SUCCESS to indicate a successful termination, or EXIT_FAILURE to indicate an unsuccessful termination.

## Why doesn’t iostream have a .h extension?

 To explain requires a short history lesson.

When C++ was first created, all of the files in the standard library ended in a _.h_ suffix. Life was consistent, and it was good. The original version of _cout_ and _cin_ were declared in _iostream.h_. When the language was standardized by the ANSI committee, they decided to move all of the names used in the standard library into the _std_ namespace to help avoid naming conflicts with user-declared identifiers. However, this presented a problem: if they moved all the names into the _std_ namespace, none of the old programs (that included iostream.h) would work anymore!

To work around this issue, a new set of header files was introduced that lack the _.h_ extension. These new header files declare all names inside the _std_ namespace. This way, older programs that include `#include <iostream.h>` do not need to be rewritten, and newer programs can `#include <iostream>`.

**The header files with the _.h_ extension declared their names in the global namespace, and may optionally declared them in the _std_ namespace as well.

**The header files without the _.h_ extension will declare their names in the _std_ namespace. Unfortunately and stupidly, these header files may optionally declared those names in the global namespace as well.

**In addition, many of the libraries inherited from C that are still useful in C++ were given a _c_ prefix (e.g. _stdlib.h_ became _cstdlib_).

## What is std::size_t?

Consider the following code:

```cpp
#include <iostream>

int main()
{
    std::cout << sizeof(int) << '\n';

    return 0;
}
```

On the author’s machine, this prints:

```Output
4
```

Pretty simple, right? We can infer that operator sizeof returns an integer value -- but what integer type is that return value? An int? A short? The answer is that sizeof (and many functions that return a size or length value) return a value of type `std::size_t`. **std::size_t** is defined as an unsigned integral type, and it is typically used to represent the size or length of objects.

Amusingly, we can use the `sizeof` operator (which returns a value of type `std::size_t`) to ask for the size of `std::size_t` itself:

```cpp
#include <cstddef> // for std::size_t
#include <iostream>

int main()
{
	std::cout << sizeof(std::size_t) << '\n';

	return 0;
}
```

Compiled as a 32-bit (4 byte) console app on the author’s system, this prints:

```Output
4
```

`std::size_t` is defined in a number of different headers. \<cstddef> is the best one to $include$, as it contains the least number of other defined identifiers.

Much like an integer can vary in size depending on the system, `std::size_t` also varies in size. `std::size_t` is guaranteed to be unsigned and at least 16 bits, but on most systems will be equivalent to the address-width of the application. That is, for 32-bit applications, `std::size_t` will typically be a 32-bit unsigned integer, and for a 64-bit application, `std::size_t` will typically be a 64-bit unsigned integer.

`sizeof` must be able to return the size of an object (in bytes) using a value of type `std::size_t`. Therefore, the largest object creatable on a system has a size (in bytes) equal to the largest value `std::size_t` can hold. If it were possible to create a larger object, `sizeof` would not be able to return its size, as it would overflow the range of `std::size_t`.

Therefore, any object with a size (in bytes) larger than the largest value an object of type `std::size_t` can hold is considered ill-formed (and will cause a compile error).

For example, the range of a 4-byte unsigned integral type is 0 to 4,294,967,295. Assuming `std::size_t` is 4 bytes in size, then an object of type `std::size_t` can hold an integral value from 0 to 4,294,967,295. This means the largest object creatable on such a system is 4,294,967,295 bytes (and calling `sizeof` on that object would return exactly 4,294,967,295).


## Output manipulators

We can override the default precision that std::cout shows by using an `output manipulator` function named `std::setprecision()`. **Output manipulators** alter how data is output, and are defined in the _iomanip_ header.

```cpp
#include <iomanip> // for output manipulator std::setprecision()
#include <iostream>

int main()
{
    std::cout << std::setprecision(17); // show 17 digits of precision
    std::cout << 3.33333333333333333333333333333333333333f <<'\n'; // f suffix means float
    std::cout << 3.33333333333333333333333333333333333333 << '\n'; // no suffix means double

    return 0;
}
```


```Output
3.3333332538604736
3.3333333333333335
```

Because we set the precision to 17 digits using `std::setprecision()`, each of the above numbers is printed with 17 digits. But, as you can see, the numbers certainly aren’t precise to 17 digits! And because floats are less precise than doubles, the float has more error.

## Rounding errors make floating point comparisons tricky

Floating point numbers are tricky to work with due to non-obvious differences between binary (how data is stored) and decimal (how we think) numbers. Consider the fraction 1/10. In decimal, this is easily represented as 0.1, and we are used to thinking of 0.1 as an easily representable number with 1 significant digit. However, in binary, decimal value 0.1 is represented by the infinite sequence: 0.00011001100110011… Because of this, when we assign 0.1 to a floating point number, we’ll run into precision problems.

You can see the effects of this in the following program:

```cpp
#include <iomanip> // for std::setprecision()
#include <iostream>

int main()
{
    double d{0.1};
    std::cout << d << '\n'; // use default cout precision of 6
    std::cout << std::setprecision(17);
    std::cout << d << '\n';

    return 0;
}
```

```Output
0.1
0.10000000000000001
```
On the top line, std::cout prints 0.1, as we expect.

On the bottom line, where we have std::cout show us 17 digits of precision, we see that d is actually _not quite_ 0.1! This is because the double had to truncate the approximation due to its limited memory. The result is a number that is precise to 16 significant digits (which type double guarantees), but the number is not _exactly_ 0.1. Rounding errors may make a number either slightly smaller or slightly larger, depending on where the truncation happens.

Rounding errors can have unexpected consequences:

```cpp
#include <iomanip> // for std::setprecision()
#include <iostream>

int main()
{
    std::cout << std::setprecision(17);

    double d1{ 1.0 };
    std::cout << d1 << '\n';

    double d2{ 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 }; // should equal 1.0
    std::cout << d2 << '\n';

    return 0;
}
```

```Output
1
0.99999999999999989
```
Although we might expect that d1 and d2 should be equal, we see that they are not. If we were to compare d1 and d2 in a program, the program would probably not perform as expected.

One last note on rounding errors: mathematical operations (such as addition and multiplication) tend to make rounding errors grow. So even though 0.1 has a rounding error in the 17th significant digit, when we add 0.1 ten times, the rounding error has crept into the 16th significant digit. Continued operations would cause this error to become increasingly significant.
## ASCII

**ASCII** stands for American Standard Code for Information Interchange, and it defines a particular way to represent English characters (plus a few other symbols) as numbers between 0 and 127 (called an **ASCII code** or **code point**).

Here’s a full table of ASCII characters:

| Code | Symbol | Code | Symbol | Code | Symbol | Code | Symbol |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0 | NUL (null) | 32 | (space) | 64 | @ | 96 | ` |
| 1 | SOH (start of header) | 33 | ! | 65 | A | 97 | a |
| 2 | STX (start of text) | 34 | ” | 66 | B | 98 | b |
| 3 | ETX (end of text) | 35 | # | 67 | C | 99 | c |
| 4 | EOT (end of transmission) | 36 | $ | 68 | D | 100 | d |
| 5 | ENQ (enquiry) | 37 | % | 69 | E | 101 | e |
| 6 | ACK (acknowledge) | 38 | & | 70 | F | 102 | f |
| 7 | BEL (bell) | 39 | ’ | 71 | G | 103 | g |
| 8 | BS (backspace) | 40 | ( | 72 | H | 104 | h |
| 9 | HT (horizontal tab) | 41 | ) | 73 | I | 105 | i |
| 10 | LF (line feed/new line) | 42 | * | 74 | J | 106 | j |
| 11 | VT (vertical tab) | 43 | + | 75 | K | 107 | k |
| 12 | FF (form feed / new page) | 44 | , | 76 | L | 108 | l |
| 13 | CR (carriage return) | 45 | - | 77 | M | 109 | m |
| 14 | SO (shift out) | 46 | . | 78 | N | 110 | n |
| 15 | SI (shift in) | 47 | / | 79 | O | 111 | o |
| 16 | DLE (data link escape) | 48 | 0 | 80 | P | 112 | p |
| 17 | DC1 (data control 1) | 49 | 1 | 81 | Q | 113 | q |
| 18 | DC2 (data control 2) | 50 | 2 | 82 | R | 114 | r |
| 19 | DC3 (data control 3) | 51 | 3 | 83 | S | 115 | s |
| 20 | DC4 (data control 4) | 52 | 4 | 84 | T | 116 | t |
| 21 | NAK (negative acknowledge) | 53 | 5 | 85 | U | 117 | u |
| 22 | SYN (synchronous idle) | 54 | 6 | 86 | V | 118 | v |
| 23 | ETB (end of transmission block) | 55 | 7 | 87 | W | 119 | w |
| 24 | CAN (cancel) | 56 | 8 | 88 | X | 120 | x |
| 25 | EM (end of medium) | 57 | 9 | 89 | Y | 121 | y |
| 26 | SUB (substitute) | 58 | : | 90 | Z | 122 | z |
| 27 | ESC (escape) | 59 | ; | 91 | [ | 123 | { |
| 28 | FS (file separator) | 60 | < | 92 | \|124 | \| |  |
| 29 | GS (group separator) | 61 | = | 93 | ] | 125 | } |
| 30 | RS (record separator) | 62 | > | 94 | ^ | 126 | ~ |
| 31 | US (unit separator) | 63 | ? | 95 | _ | 127 | DEL (delete) |

Codes 0-31 are called the unprintable chars, and they’re mostly used to do formatting and control printers. Most of these are obsolete now. If you try to print these chars, the results are dependent upon your OS (you may get some emoji-like characters).

Codes 32-127 are called the printable characters, and they represent the letters, number characters, and punctuation that most computers use to display basic English text.

## Escape sequences

|Name|Symbol|Meaning|
|---|---|---|
|Alert|\a|Makes an alert, such as a beep|
|Backspace|\b|Moves the cursor back one space|
|Formfeed|\f|Moves the cursor to next logical page|
|Newline|\n|Moves cursor to next line|
|Carriage return|\r|Moves cursor to beginning of line|
|Horizontal tab|\t|Prints a horizontal tab|
|Vertical tab|\v|Prints a vertical tab|
|Single quote|\’|Prints a single quote|
|Double quote|\”|Prints a double quote|
|Backslash|\\|Prints a backslash.|
|Question mark|\?|Prints a question mark.  <br>No longer relevant. You can use question marks unescaped.|
|Octal number|\(number)|Translates into char represented by octal|
|Hex number|\x(number)|Translates into char represented by hex number|

## What about the other char types, `wchar_t`, `char8_t`, `char16_t`, and `char32_t`?

`wchar_t` should be avoided in almost all cases (except when interfacing with the Windows API). Its size is implementation defined, and is not reliable. It has largely been deprecated.

Much like ASCII maps the integers 0-127 to American English characters, other character encoding standards exist to map integers (of varying sizes) to characters in other languages. The most well-known mapping outside of ASCII is the Unicode standard, which maps over 144,000 integers to characters in many different languages. Because Unicode contains so many code points, a single Unicode code point needs 32-bits to represent a character (called UTF-32). However, Unicode characters can also be encoded using multiple 16-bit or 8-bit characters (called UTF-16 and UTF-8 respectively).

`char16_t` and `char32_t` were added to C++11 to provide explicit support for 16-bit and 32-bit Unicode characters. These char types have the same size as `std::uint_least16_t`, `std::uint_least32_t`, and `unsigned char` respectively (but are distinct types). In theory, this means on an esoteric architecture that `char16_t` and `char32_t` could be larger than the number of expected bits.

You won’t need to use `char8_t`, `char16_t`, or `char32_t` unless you’re planning on making your program Unicode compatible. Unicode and localization are generally outside the scope of these tutorials, so we won’t cover it further.

## std::int8_t and std::uint8_t likely behave like chars instead of integers

Most compilers define and treat `std::int8_t` and `std::uint8_t` (and the corresponding fast and least fixed-width types) identically to types `signed char` and `unsigned char` respectively. Now that we’ve covered what chars are, we can demonstrate where this can be problematic:

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    std::int8_t myInt{65};      // initialize myInt with value 65
    std::cout << myInt << '\n'; // you're probably expecting this to print 65

    return 0;
}
```

Because `std::int8_t` describes itself as an int, you might be tricked into believing that the above program will print the integral value `65`. However, on most systems, this program will print `A` instead (treating `myInt` as a `signed char`). However, this is not guaranteed (on some systems, it may actually print `65`).

If you want to ensure that a `std::int8_t` or `std::uint8_t` object is treated as an integer, you can convert the value to an integer using [[11. Introduction to type conversion and static_cast#An introduction to explicit type conversion via the static_cast operator|static_cast]] :

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    std::int8_t myInt{65};
    std::cout << static_cast<int>(myInt) << '\n'; // will always print 65

    return 0;
}
```

In cases where `std::int8_t` is treated as a char, input from the console can also cause problems:

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    std::cout << "Enter a number between 0 and 127: ";
    std::int8_t myInt{};
    std::cin >> myInt;

    std::cout << "You entered: " << static_cast<int>(myInt) << '\n';

    return 0;
}
```

```Output
Enter a number between 0 and 127: 35
You entered: 51
```
Here’s what’s happening. When `std::int8_t` is treated as a char, the input routines interpret our input as a sequence of characters, not as an integer. So when we enter `35`, we’re actually entering two chars, `'3'` and `'5'`. Because a char object can only hold one character, the `'3'` is extracted (the `'5'` is left in the input stream for possible extraction later). Because the char `'3'` has ASCII code point 51, the value `51` is stored in `myInt`, which we then print later as an int.

In contrast, the other fixed-width types will always print and input as integral values.

## Type qualifiers

A **type qualifier** (sometimes called a **qualifier** for short) is a keyword that is applied to a type that modifies how that type behaves. The `const` used to declare a constant variable is called a **const type qualifier** (or **const qualifier** for short).

As of C++23, C++ only has two type qualifiers: `const` and `volatile`.

### #Extra 
The `volatile` qualifier is used to tell the compiler that an object may have its value changed at any time. This rarely-used qualifier disables certain types of optimizations.

In technical documentation, the `const` and `volatile` qualifiers are often referred to as **cv-qualifiers**. The following terms are also used in the C++ standard:

- A **cv-unqualified** type is a type with no type qualifiers (e.g. `int`).
- A **cv-qualified** type is a type with one or more type qualifiers applied (e.g. `const int`).
- A **possibly cv-qualified** type is a type that may be cv-unqualified or cv-qualified.

## Optimization can make programs harder to debug

When the compiler optimizes a program, variables, expressions, statements, and function calls may be rearranged, altered, replaced with a value, or even removed entirely. Such changes can make it hard to debug a program effectively.

At runtime, it can be hard to debug compiled code that no longer correlates very well with the original source code. For example, if you try to watch a variable that has been optimized out, the debugger won’t be able to locate the variable. If you try to step into a function that has been optimized away, the debugger will simply skip over it. So if you are debugging your code and the debugger is behaving strangely, this is the most likely reason.

At compile-time, we have little visibility and few tools to help us understand what the compiler is even doing. If a variable or expression is replaced with a value, and that value is wrong, how do we even go about debugging it? This is an ongoing challenge.

To help minimize such issues, debug builds will typically turn down (or turn off) optimizations, so that the compiled code will more closely match the source code.

## The performance of inline code

Beyond removing the cost of function call, inline expansion can also allow the compiler to optimize the resulting code more efficiently.

However, inline expansion has its own potential cost: if the body of the function being expanded takes more instructions than the function call being replaced, then each inline expansion will cause the executable to grow larger. Larger executables tend to be slower (due to not fitting as well in memory caches).

The decision about whether a function would benefit from being made inline (because removal of the function call overhead outweighs the cost of a larger executable) is not straightforward. Inline expansion could result in performance improvements, performance reductions, or no change to performance at all, depending on the relative cost of a function call, the size of the function, and what other optimizations can be performed.

Inline expansion is best suited to simple, short functions (e.g. no more than a few statements), especially cases where a single function call can be executed more than once (e.g. function calls inside a loop).

## What about `std::is_constant_evaluated` or `if constexpr`?

Introduced in C++20, `std::is_constant_evaluated()` (defined in the <type_traits> header) was intended to allow differentiation of behavior when a function is evaluating at runtime vs compile-time, so you could do something like this:

```cpp
#include <type_traits> // for std::is_constant_evaluated()

constexpr int someFunction()
{
    if (std::is_constant_evaluated()) // if evaluating at compile-time
        // do something
    else // runtime evaluation
        // do something else
}
```

However, as [the paper that proposed this feature](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0595r2.html) indicates, the standard doesn’t actually make a distinction between “compile time” and “run time”. So instead, `std::is_constant_evaluated()` returns a `bool` indicating whether the current function is executing in a constant-evaluated context. A **constant-evaluated context** (also called a **constant context**) is defined as one in which a constant expression is required (such as the initialization of a constexpr variable). In cases where the compiler is required to evaluate a constant expression at compile-time `std::is_constant_evaluated` will `true` as expected.

However, the compiler may also choose to evaluate a constexpr function at compile-time in a context that does not require a constant expression. In such cases, `std::is_constant_expression` will return `false` even though the function did evaluate at compile-time. So `std::is_constant_evaluated` really means “the compiler is being forced to evaluate this at compile-time”, not “this is evaluating at compile-time”.

Introduced in C++23, `if constexpr` is shorthand for `if (std::is_constant_evaluated)` (that also does not require the <type_traits> header). It has the same issue.

## Table of operator precedence and associativity

| Prec/Ass | Operator                                                                                                                                                                                               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Pattern                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 L->R   | ::  <br>::                                                                                                                                                                                             | Global scope (unary)  <br>Namespace scope (binary)                                                                                                                                                                                                                                                                                                                                                                                                                      | ::name  <br>class_name::member_name                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 2 L->R   | ()  <br>()  <br>type()  <br>type{}  <br>[]  <br>.  <br>->  <br>++  <br>––  <br>typeid  <br>const_cast  <br>dynamic_cast  <br>reinterpret_cast  <br>static_cast  <br>sizeof…  <br>noexcept  <br>alignof | Parentheses  <br>Function call  <br>Functional cast  <br>List init temporary object (C++11)  <br>Array subscript  <br>Member access from object  <br>Member access from object ptr  <br>Post-increment  <br>Post-decrement  <br>Run-time type information  <br>Cast away const  <br>Run-time type-checked cast  <br>Cast one type to another  <br>Compile-time type-checked cast  <br>Get parameter pack size  <br>Compile-time exception check  <br>Get type alignment | (expression)  <br>function_name(arguments)  <br>type(expression)  <br>type{expression}  <br>pointer[expression]  <br>object.member_name  <br>object_pointer->member_name  <br>lvalue++  <br>lvalue––  <br>typeid(type) or typeid(expression)  <br>const_cast<type>(expression)  <br>dynamic_cast<type>(expression)  <br>reinterpret_cast<type>(expression)  <br>static_cast<type>(expression)  <br>sizeof…(expression)  <br>noexcept(expression)  <br>alignof(type) |
| 3 R->L   | +  <br>-  <br>++  <br>––  <br>!  <br>not  <br>~  <br>(type)  <br>sizeof  <br>co_await  <br>&  <br>*  <br>new  <br>new[]  <br>delete  <br>delete[]                                                      | Unary plus  <br>Unary minus  <br>Pre-increment  <br>Pre-decrement  <br>Logical NOT  <br>Logical NOT  <br>Bitwise NOT  <br>C-style cast  <br>Size in bytes  <br>Await asynchronous call  <br>Address of  <br>Dereference  <br>Dynamic memory allocation  <br>Dynamic array allocation  <br>Dynamic memory deletion  <br>Dynamic array deletion                                                                                                                           | +expression  <br>-expression  <br>++lvalue  <br>––lvalue  <br>!expression  <br>not expression  <br>~expression  <br>(new_type)expression  <br>sizeof(type) or sizeof(expression)  <br>co_await expression (C++20)  <br>&lvalue  <br>*expression  <br>new type  <br>new type[expression]  <br>delete pointer  <br>delete[] pointer                                                                                                                                   |
| 4 L->R   | ->*  <br>.*                                                                                                                                                                                            | Member pointer selector  <br>Member object selector                                                                                                                                                                                                                                                                                                                                                                                                                     | object_pointer->*pointer_to_member  <br>object.*pointer_to_member                                                                                                                                                                                                                                                                                                                                                                                                   |
| 5 L->R   | *  <br>/  <br>%                                                                                                                                                                                        | Multiplication  <br>Division  <br>Remainder                                                                                                                                                                                                                                                                                                                                                                                                                             | expression * expression  <br>expression / expression  <br>expression % expression                                                                                                                                                                                                                                                                                                                                                                                   |
| 6 L->R   | +  <br>-                                                                                                                                                                                               | Addition  <br>Subtraction                                                                                                                                                                                                                                                                                                                                                                                                                                               | expression + expression  <br>expression - expression                                                                                                                                                                                                                                                                                                                                                                                                                |
| 7 L->R   | <<  <br>>>                                                                                                                                                                                             | Bitwise shift left / Insertion  <br>Bitwise shift right / Extraction                                                                                                                                                                                                                                                                                                                                                                                                    | expression << expression  <br>expression >> expression                                                                                                                                                                                                                                                                                                                                                                                                              |
| 8 L->R   | <=>                                                                                                                                                                                                    | Three-way comparison (C++20)                                                                                                                                                                                                                                                                                                                                                                                                                                            | expression <=> expression                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 9 L->R   | <  <br><=  <br>>  <br>>=                                                                                                                                                                               | Comparison less than  <br>Comparison less than or equals  <br>Comparison greater than  <br>Comparison greater than or equals                                                                                                                                                                                                                                                                                                                                            | expression < expression  <br>expression <= expression  <br>expression > expression  <br>expression >= expression                                                                                                                                                                                                                                                                                                                                                    |
| 10 L->R  | ==  <br>!=                                                                                                                                                                                             | Equality  <br>Inequality                                                                                                                                                                                                                                                                                                                                                                                                                                                | expression == expression  <br>expression != expression                                                                                                                                                                                                                                                                                                                                                                                                              |
| 11 L->R  | &                                                                                                                                                                                                      | Bitwise AND                                                                                                                                                                                                                                                                                                                                                                                                                                                             | expression & expression                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 12 L->R  | ^                                                                                                                                                                                                      | Bitwise XOR                                                                                                                                                                                                                                                                                                                                                                                                                                                             | expression ^ expression                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 13 L->R  | \|                                                                                                                                                                                                     | Bitwise OR                                                                                                                                                                                                                                                                                                                                                                                                                                                              | expression \| expression                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 14 L->R  | &&  <br>and                                                                                                                                                                                            | Logical AND  <br>Logical AND                                                                                                                                                                                                                                                                                                                                                                                                                                            | expression && expression  <br>expression and expression                                                                                                                                                                                                                                                                                                                                                                                                             |
| 15 L->R  | \|  <br>or                                                                                                                                                                                             | Logical OR  <br>Logical OR                                                                                                                                                                                                                                                                                                                                                                                                                                              | expression \| expression  <br>expression or expression                                                                                                                                                                                                                                                                                                                                                                                                              |
| 16 R->L  | throw  <br>co_yield  <br>?:  <br>=  <br>*=  <br>/=  <br>%=  <br>+=  <br>-=  <br><<=  <br>>>=  <br>&=  <br>\|=  <br>^=                                                                                  | Throw expression  <br>Yield expression (C++20)  <br>Conditional  <br>Assignment  <br>Multiplication assignment  <br>Division assignment  <br>Remainder assignment  <br>Addition assignment  <br>Subtraction assignment  <br>Bitwise shift left assignment  <br>Bitwise shift right assignment  <br>Bitwise AND assignment  <br>Bitwise OR assignment  <br>Bitwise XOR assignment                                                                                        | throw expression  <br>co_yield expression  <br>expression ? expression : expression  <br>lvalue = expression  <br>lvalue *= expression  <br>lvalue /= expression  <br>lvalue %= expression  <br>lvalue += expression  <br>lvalue -= expression  <br>lvalue <<= expression  <br>lvalue >>= expression  <br>lvalue &= expression  <br>lvalue \|= expression  <br>lvalue ^= expression                                                                                 |
| 17 L->R  | ,                                                                                                                                                                                                      | Comma operator                                                                                                                                                                                                                                                                                                                                                                                                                                                          | expression, expression                                                                                                                                                                                                                                                                                                                                                                                                                                              |

## The order of evaluation of operands and function arguments is mostly unspecified

In most cases, the order of evaluation for operands and function arguments is unspecified, meaning they may be evaluated in any order.

Consider the following expression:

```cpp
a * b + c * d
```

We know from the precedence and associativity rules above that this expression will be grouped as if we had typed:

```cpp
(a * b) + (c * d)
```

If `a` is `1`, `b` is `2`, `c` is `3`, and `d` is `4`, this expression will always compute the value `14`.

However, the precedence and associativity rules only tell us how operators and operands are grouped and the order in which value computation will occur. They do not tell us the order in which the operands or subexpressions are evaluated. The compiler is free to evaluate operands `a`, `b`, `c`, or `d` in any order. The compiler is also free to calculate `a * b` or `c * d` first.

For most expressions, this is irrelevant. In our sample expression above, it doesn’t matter whether in which order variables `a`, `b`, `c`, or `d` are evaluated for their values: the value calculated will always be `14`. There is no ambiguity here.

But it is possible to write expressions where the order of evaluation does matter. Consider this program, which contains a mistake often made by new C++ programmers:

```cpp
#include <iostream>

int getValue()
{
    std::cout << "Enter an integer: ";

    int x{};
    std::cin >> x;
    return x;
}

void printCalculation(int x, int y, int z)
{
    std::cout << x + (y * z);
}

int main()
{
    printCalculation(getValue(), getValue(), getValue()); // this line is ambiguous

    return 0;
}
```


If you run this program and enter the inputs `1`, `2`, and `3`, you might assume that this program would calculate `1 + (2 * 3)` and print `7`. But that is making the assumption that the arguments to `printCalculation()` will evaluate in left-to-right order (so parameter `x` gets value `1`, `y` gets value `2`, and `z` gets value `3`). If instead, the arguments evaluate in right-to-left order (so parameter `z` gets value `1`, `y` gets value `2`, and `x` gets value `3`), then the program will print `5` instead.

>Tip
>
>The Clang compiler evaluates arguments in left-to-right order. The GCC compiler evaluates arguments in right-to-left order. You can run the above program on each of these compilers (e.g. on [Wandbox](https://wandbox.org/#)) and see for yourself.

The above program can be made unambiguous by making each function call to `getValue()` a separate statement:

```cpp
#include <iostream>

int getValue()
{
    std::cout << "Enter an integer: ";

    int x{};
    std::cin >> x;
    return x;
}

void printCalculation(int x, int y, int z)
{
    std::cout << x + (y * z);
}

int main()
{
    int a{ getValue() }; // will execute first
    int b{ getValue() }; // will execute second
    int c{ getValue() }; // will execute third

    printCalculation(a, b, c); // this line is now unambiguous

    return 0;
}
```

In this version, `a` will always have value `1`, `b` will have value `2`, and `c` will have value `3`. When the arguments to `printCalculation()` are evaluated, it doesn’t matter which order the argument evaluation happens in -- parameter `x` will always get value `1`, `y` will get value `2`, and `z` will get value `3`. This version will deterministically print `7`.

## The debate over use of break and continue

Many textbooks caution readers not to use `break` and `continue` in loops, both because it causes the execution flow to jump around, and because it can make the flow of logic harder to follow. For example, a `break` in the middle of a complicated piece of logic could either be missed, or it may not be obvious under what conditions it should be triggered.

However, used judiciously, `break` and `continue` can help make loops more readable by keeping the number of nested blocks down and reducing the need for complicated looping logic.

For example, consider the following program:

```cpp
#include <iostream>

int main()
{
    int count{ 0 }; // count how many times the loop iterates
    bool keepLooping { true }; // controls whether the loop ends or not
    while (keepLooping)
    {
        std::cout << "Enter 'e' to exit this loop or any other character to continue: ";
        char ch{};
        std::cin >> ch;

        if (ch == 'e')
            keepLooping = false;
        else
        {
            ++count;
            std::cout << "We've iterated " << count << " times\n";
        }
    }

    return 0;
}
```

This program uses a Boolean variable to control whether the loop continues or not, as well as a nested block that only runs if the user doesn’t exit.

Here’s a version that’s easier to understand, using a `break` statement:

```cpp
#include <iostream>

int main()
{
    int count{ 0 }; // count how many times the loop iterates
    while (true) // loop until user terminates
    {
        std::cout << "Enter 'e' to exit this loop or any other character to continue: ";
        char ch{};
        std::cin >> ch;

        if (ch == 'e')
            break;

        ++count;
        std::cout << "We've iterated " << count << " times\n";
    }

    return 0;
}
```

In this version, by using a single `break` statement, we’ve avoided the use of a Boolean variable (and having to understand both what its intended use is, and where its value is changed), an `else` statement, and a nested block.

The `continue` statement is most effectively used at the top of a for-loop to skip loop iterations when some condition is met. This can allow us to avoid a nested block.

For example, instead of this:

```cpp
#include <iostream>

int main()
{
    for (int count{ 0 }; count < 10; ++count)
    {
        // if the number is not divisible by 4...
        if ((count % 4) != 0) // nested block
        {
                // Print the number
                std::cout << count << '\n';
        }
    }

    return 0;
}
```

We can write this:

```cpp
#include <iostream>

int main()
{
    for (int count{ 0 }; count < 10; ++count)
    {
        // if the number is divisible by 4, skip this iteration
        if ((count % 4) == 0)
            continue;

        // no nested block needed

        std::cout << count << '\n';
    }

    return 0;
}
```

Minimizing the number of variables used and keeping the number of nested blocks down both improve code comprehensibility more than a `break` or `continue` harms it. For that reason, we believe judicious use of `break` or `continue` is acceptable.

## Prevent the user from typing invalid input in the first place.
Some graphical user interfaces and advanced text interfaces will let you validate input as the user enters it (character by character). Generally speaking, the programmer provides a validation function that accepts the input the user has entered so far, and returns true if the input is valid, and false otherwise. This function is called every time the user presses a key. If the validation function returns true, the key the user just pressed is accepted. If the validation function returns false, the character the user just input is discarded (and not shown on the screen). Using this method, you can ensure that any input the user enters is guaranteed to be valid, because any invalid keystrokes are discovered and discarded immediately. Unfortunately, std::cin does not support this style of validation.

## Let the user enter whatever they want into a string, then validate whether the string is correct, and if so, convert the string to the final variable format.

Since strings do not have any restrictions on what characters can be entered, extraction is guaranteed to succeed (though remember that std::cin stops extracting at the first non-leading whitespace character). Once a string is entered, the program can then parse the string to see if it is valid or not. However, parsing strings and converting string input to other types (e.g. numbers) can be challenging, so this is only done in rare cases.

## Treat extraneous input as a failure case

In certain cases, it may be better to treat extraneous input as a failure case (rather than just ignoring it). We can then ask the user to re-enter their input.

Here’s a variation of `getDouble()` that asks the user to re-enter their input if there is any extraneous input entered:

```cpp
double getDouble()
{
    while (true) // Loop until user enters a valid input
    {
        std::cout << "Enter a decimal number: ";
        double x{};
        std::cin >> x;

        // Check for failed extraction here (see section below)

        // If there is extraneous input, treat as failure case
        if (!std::cin.eof() && std::cin.peek() != '\n')
        {
            ignoreLine(); // remove extraneous input
            continue;
        }

        return x;
    }
}
```

The above snippet makes use of two functions we haven’t seen before:

- The `std::cin.eof()` function returns `true` if the last input operation (in this case the extraction to `x`) reached the end of the input stream.
- The `std::cin.peek()` function allows us to peek at the next character in the input stream without extracting it.

Here’s how this function works. After the user’s input has been extracted to `x`, there may or may not be additional (unextracted) characters left in `std::cin`.

First, we call `std::cin.eof()` to see if the extraction to `x` reached the end of the input stream. If so, then we know all characters were extracted, which is a success case.

Otherwise, there must be additional characters still inside `std::cin` waiting to be extracted. In that case, we call `std::cin.peek()` to peek at the next character waiting to be extracted without actually extracting it. If that next character is a `'\n'`, that means we’ve already extracted all of the characters on this line of input to `x`. This is also a success case.

However, if the next character is something other than `'\n'`, then the user must have entered extraneous input that wasn’t extracted to `x`. That’s our failure case. We clear out all of that extraneous input, and `continue` back to the top of the loop to try again.

## Constexpr lvalue references

When applied to a reference, `constexpr` allows the reference to be used in a constant expression. Constexpr references have a particular limitation: they can only be bound to objects with static duration (either globals or static locals). This is because the compiler knows where static objects will be instantiated in memory, so it can treat that address as a compile-time constant.

A constexpr reference cannot bind to a (non-static) local variable. This is because the address of local variables is not known until the function they are defined within is actually called.

```cpp
int g_x { 5 };

int main()
{
    [[maybe_unused]] constexpr int& ref1 { g_x }; // ok, can bind to global

    static int s_x { 6 };
    [[maybe_unused]] constexpr int& ref2 { s_x }; // ok, can bind to static local

    int x { 6 };
    [[maybe_unused]] constexpr int& ref3 { x }; // compile error: can't bind to non-static object

    return 0;
}
```

When defining a constexpr reference to a const variable, we need to apply both `constexpr` (which applies to the reference) and `const` (which applies to the type being referenced).

```cpp
int main()
{
    static const int s_x { 6 }; // a const int
    [[maybe_unused]] constexpr const int& ref2 { s_x }; // needs both constexpr and const

    return 0;
}
```
## Range-based for loops over C-style arrays are implemented using pointer arithmetic

Consider the following range-based for loop:

```cpp
#include <iostream>

int main()
{
	constexpr int arr[]{ 9, 7, 5, 3, 1 };

	for (auto e : arr)         // iterate from `begin` up to (but excluding) `end`
	{
		std::cout << e << ' '; // dereference our loop variable to get the current element
	}

	return 0;
}
```

If you look at the [documentation](https://en.cppreference.com/w/cpp/language/range-for) for range-based for loops, you’ll see that they are typically implemented something like this:

```cpp
{
    auto __begin = begin-expr;
    auto __end = end-expr;

    for ( ; __begin != __end; ++__begin)
    {
        range-declaration = *__begin;
        loop-statement;
    }
}
```
Let’s replace the range-based for loop in the prior example with this implementation:

```cpp
#include <iostream>

int main()
{
	constexpr int arr[]{ 9, 7, 5, 3, 1 };

	auto __begin = arr;                // arr is our begin-expr
	auto __end = arr + std::size(arr); // arr + std::size(arr) is our end-expr

	for ( ; __begin != __end; ++__begin)
	{
		auto e = *__begin;         // e is our range-declaration
		std::cout << e << ' ';     // here is our loop-statement
	}

	return 0;
}
```

Note how similar this is to the example we wrote in the prior section! The only difference is that we’re assigning `*__begin` to `e` and using `e` rather than just using `*__begin` directly!
## Cartesian coordinates vs Array indices

In geometry, the [Cartesian coordinate system](https://en.wikipedia.org/wiki/Cartesian_coordinate_system) is often used to describe the position of objects. In two dimensions, we have two coordinate axes, conventionally named “x” and “y”. “x” is the horizontal axis, and “y” is the vertical axis.

In two dimensions, the Cartesian position of an object can be described as an { x, y } pair, where x-coordinate and y-coordinate are values indicating how far to the right of the x-axis and how far above the y-axis an object is positioned. Sometimes the y-axis is flipped (so that the y-coordinate describes how far below the y-axis something is).

Now let’s take a look at our 2d array layout in C++:

```cpp
// col 0   col 1   col 2   col 3   col 4
// [0][0]  [0][1]  [0][2]  [0][3]  [0][4]  row 0
// [1][0]  [1][1]  [1][2]  [1][3]  [1][4]  row 1
// [2][0]  [2][1]  [2][2]  [2][3]  [2][4]  row 2
```

This is also a two-dimensional coordinate system, where the position of an element can be described as [row][col] (where the col-axis is flipped).

While each of these coordinate systems is fairly easy to understand independently, converting from Cartesian { x, y } to Array indices [row][col] is a bit counter-intuitive.

The key insight is that the x-coordinate in a Cartesian system describes which _column_ is being selected in the array indexing system. Conversely, the y-coordinate describes which _row_ is being selected. Therefore, an { x, y } Cartesian coordinate translates to an [y][x] array coordinate, which is backwards from what we might expect!

This leads to 2d loops that look like this:

```cpp
for (std::size_t y{0}; y < std::size(arr); ++y) // outer loop is rows / y
{
    for (std::size_t x{0}; x < std::size(arr[0]); ++x) // inner loop is columns / x
        std::cout << arr[y][x] << ' '; // index with y (row) first, then x (col)
```

Note that in this case, we index the array as `a[y][x]`, which is probably backwards from the alphabetic ordering you were expecting.

## Mailbox analogy for stack
The stack itself is a fixed-size chunk of memory addresses. The mailboxes are memory addresses, and the “items” we’re pushing and popping on the stack are called **stack frames**. A stack frame keeps track of all of the data associated with one function call.

## **std::auto_ptr, and why it was a bad idea**

Now would be an appropriate time to talk about std::auto_ptr. std::auto_ptr, introduced in C++98 and removed in C++17, was C++’s first attempt at a standardized smart pointer. std::auto_ptr opted to implement move semantics just like the Auto_ptr2 class does.

However, std::auto_ptr (and our Auto_ptr2 class) has a number of problems that makes using it dangerous.

First, because std::auto_ptr implements move semantics through the copy constructor and assignment operator, passing a std::auto_ptr by value to a function will cause your resource to get moved to the function parameter (and be destroyed at the end of the function when the function parameters go out of scope). Then when you go to access your auto_ptr argument from the caller (not realizing it was transferred and deleted), you’re suddenly dereferencing a null pointer. Crash!

Second, std::auto_ptr always deletes its contents using non-array delete. This means auto_ptr won’t work correctly with dynamically allocated arrays, because it uses the wrong kind of deallocation. Worse, it won’t prevent you from passing it a dynamic array, which it will then mismanage, leading to memory leaks.

Finally, auto_ptr doesn’t play nice with a lot of the other classes in the standard library, including most of the containers and algorithms. This occurs because those standard library classes assume that when they copy an item, it actually makes a copy, not a move.

Because of the above mentioned shortcomings, std::auto_ptr has been deprecated in C++11 and removed in C++17.

## The exception safety issue in more detail

For those wondering what the “exception safety issue” mentioned above is, here’s a description of the issue.

Consider an expression like this one:

```cpp
some_function(std::unique_ptr<T>(new T), function_that_can_throw_exception());
```

The compiler is given a lot of flexibility in terms of how it handles this call. It could create a new T, then call function_that_can_throw_exception(), then create the std::unique_ptr that manages the dynamically allocated T. If function_that_can_throw_exception() throws an exception, then the T that was allocated will not be deallocated, because the smart pointer to do the deallocation hasn’t been created yet. This leads to T being leaked.

std::make_unique() doesn’t suffer from this problem because the creation of the object T and the creation of the std::unique_ptr happen inside the std::make_unique() function, where there’s no ambiguity about order of execution.

This issue was fixed in C++17, as evaluation of function arguments can no longer be interleaved.