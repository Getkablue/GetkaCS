If you completed [[Assignment 2 - Environment Setup]], then you have successfully created your first program. Hello World!

Let us now dive into the basic structure of this program. We start with the C version, "helloworld.c":

```c
#include <stdio.h>
int main() {
	printf("Hello world!\n");
	return 0;
}

```
You will notice this shares a lot of structure with the C++ version, "helloworld.cpp":
``` cpp
#include <iostream>
int main() {
  std::cout << "Hello, World!" << std::endl;
  return 0;
}
```

For the simplest possible program there's actually quite a lot going on here! Remember from the computing review that code in userspace does not exist in isolation -- the OS does a great deal of work setting everything up for your code to run. But even just looking at it as written, it's complex... Let's try to break it down line-by-line. Don't worry if the terminology below is confusing for now. We will go over each mechanism soon. 

``` c
#include <stdio.h>
```
or in C++:
``` cpp
#include <iostream>
```
This line (1) is a *preprocessor directive*. This line tells the compiler toolchain: "Before you compile this, I want you to include the code required for using standard I/O (input/output) streams". We will need this functionality to write to the console in a basic manner. Behind the scenes, you are literally including another file: the C++ "#include \<iostream\>" line gets replaced with the contents of a file called "iostream.h" during a step called *preprocessing*, likewise for "<stdio.h>". Note the ".h" extension, for "header". Your compiler comes pre-packaged with many of these header files, and you will eventually write your own. For now, let's move to the next line.

``` C
int main() {
```

This line declares our *entrypoint*. Remember how there are libraries and executables? An entrypoint is basically what differentiates the two. With an entrypoint, your program has a place to begin. By convention in the C and C++ world, this entrypoint is always a *function* called "main" -- when the OS loads up your program, it will look for the entrypoint and start there after doing some setup. The prefix "int" says that this function returns an int -- that is, its output is an integer value. The open-closed parentheses, "()", indicate that this thing called main is a function which takes no arguments. Finally, the open curly brace, "{", indicates the start of the definition for the function.
(You can actually place the { on a new line on its own, or at the beginning of the next line. For C and C++ this does not matter.) Moving on...

```c
printf("Hello World!\n");
```

```cpp
std::cout << "Hello, World!" << std::endl;
```

This line is a *statement*. This is the first part of our code we have seen that will actually be executed. So what is going on here? Why is this line so different between C and C++?
Spoiler alert: "return 0" in the next line isn't printing to the console. So that means this line is responsible for that. 
In C, we call a function called "printf" and pass it the text we want to print. "\\n" is just a newline character, so in total we will print "Hello World!" and then go to the next line.
We didn't define printf, though. Therefore, printf is either defined in the stdio.h file, or it is a builtin function that the compiler knows. But whatever that function is, it prints any text we pass it to the console.
In C++, iostream.h defines some other symbols: the symbols "std::cout" and "std::endl", as well as an operator << . Through context clues and documentation online, we can infer that std::cout is shorthand to refer to the console output, and std::endl is just a stand-in for the "new line" character. This is our first glimpse at object-oriented programming -- the console is treated as an object, which can be communicated to using the operator. In essence, "std::cout" is an *object metaphor* for the console, and the operator << is an *action metaphor* for *sending text to that console*. Creating symbols for *objects* like this is a very powerful tool which allows us to write very expressive code. We will explore this in more depth later.

(Actually, the way this was designed in C++ is generally considered bad design, because << does not mean anything like that in any other context... a story for another day).

At the end of the line, we cap it with a semicolon to indicate the end of a statement. **This is critical in C/C++ and forgetting one can cause compilation errors that will be extremely cryptic for a beginner. Always check this first.**

```c
return 0;
```

This line is another statement. It says that the function will return the value: 0. By convention, the program entrypoint is supposed to return an integer value. We will talk more about returns in the notes on functions, but for now it suffices to say that a function can *return* some data to the thing that called it. Hence, writing "return 0;" simply says, terminate this function and yield the value 0 to the thing that invoked this function.

When a program entrypoint (i.e. main()) returns a value, this is considered an "exit code" for the process. These exit codes are generally used to determine whether or not a program completed successfully, or whether it exited with an error ("errored out" or "failed"). 

Finally, we end with:
```C
}
```
This closes the definition of main(). Since main() is the only function in our program, this is also the end of the file. We will review rules about definitions, declarations and scope soon.

## Python

Yup, the Hello World program for Python is *seriously* just 

```python
print("Hello World!")
```

That's as simple as it gets! Because the Python interpreter is its own process, it has its own entrypoint. When it reads a script file, like helloworld.py, it starts at the top of the file and just works its way down. Since this is the first line, it executes it, then when it hits the end of the file without any errors, it returns successfully (with exit code 0.)

Of course, this is simpler. One of the reasons why Python gained so much popularity so quickly is that it is generally simple, straightforward and usually easy to understand. However, this comes at a cost.  As we continue in the course, the many ways in which this is the case will become more apparent. 

Now, I will do the most villainous thing so far in this course. Say goodbye to Python for a while. That's right, we won't be touching it for some time.

Sometimes, the obscuration of the details can lead to an incorrect understanding, or just poorly-formed programs in general, even if it seems convenient and easy. There is also a performance overhead, but this course isn't really about performance optimization. The reason why we teach C/C++ in this course is to help ensure that you first understand the fundamentals and their semantics. That way, you'll have some idea of what is going on when Python behaves counterintuitively, and you'll be able to pick up any other programming language **much** more easily if you understand these concepts.

For the above reason, I will be mentioning Python only briefly, when relevant, until later in the course. When we return to it, you will be much better equipped to solve problems, and you will better appreciate the conveniences that Python affords. From there, you should be well-equipped to pick up almost any programming language out there. Trust me, if you understand C and C++, most other languages will be a piece of cake.

