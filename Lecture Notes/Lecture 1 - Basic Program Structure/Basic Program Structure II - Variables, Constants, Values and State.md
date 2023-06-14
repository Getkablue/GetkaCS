Often, even when we write an algorithm in pseudocode, we find that we need a "place to store" some information for use later. That is, we need *memory* of what has been done in the program so far, or at least a tiny part of it. For example, if I am multiplying two numbers via repeated addition, I need to keep track of, at least:
- How many additions I have done so far.
- What the result of the previous addition was.

These pieces of information are a part of the *state* of the program, and they change as the program progresses. We often think of "marking a place for" these pieces of information, and putting new values into that "place", and taking values from that "place", as part of our algorithm.

That "place" represents a *variable* piece of data in memory. For short, we call them *variables*. Each variable *has a value* at any given moment, but that value does not define the variable (at least, not for long). The variable is defined by its meaning, and usually given a name for easy reference. For instance, I might call the first variable in the multiplication example "counter" and the second variable "last_result". These variables are *mutable*, as their value can change.

There is another type of data: a *constant*. Some problems and algorithms might use a certain number, but that number never changes. For example, calculating the circumference of a circle from its diameter requires the number pi, which we know to have a value of roughly 3.14 . We never change pi, it's just sort of a "fact", a *constant value* that we find useful. In this way, constants are "baked into" our program -- they can't change, no matter what happens in the program. A constant can be said to be an *immutable value*.

Many hilarious (and tragic) bugs have arisen because someone, first, forgot to mark a named value as constant/immutable, and second, changed it somewhere in their program (sometimes someone overwrote it by accident at runtime, but that's more complicated). There was a time where, in Python, you could write:
``` python
False = True
```

And the interpreter wouldn't even complain about it. While it might not be clear exactly what that would end up doing, I hope it is crystal clear that "something which is False *cannot be True*" is a core assumption of our logic. Our code, and logic in general, is only as sound as the assumptions made to justify it. (Rest assured, the resulting bugs were absolutely insane.)

## Using variables and constants

When thinking about how to solve a problem, and in writing a program, we need to *declare* variables. A *declaration* is just a statement given by you, the programmer, that something exists. In pseudocode-ish statements, we might say "there is a variable called x". We also usually declare what kind of thing that variable is. In pseudocodish: "there is an integer variable called x". 

Often, we need a value for that variable to start at. We might say "Let x be a variable integer with a starting value of 8". This is *initializing* a variable. By contrast, a constant is only ever the same value that it was initialized with, so this is the *definition* of the constant.  If we *don't* initialize a variable, we have *no idea* what it contains, and anything that uses it is undefined in its behavior. (You can think of an uninitialized variable as being full of junk memory from other processes.)

In C and C++:
``` c
int age = 8; // Non-const variable declaration and definition
age++; // Increment age up by 1, no problem 

const int twizzlersPerBag = 32;
twizzlersPerBag++; // ERROR: Cannot modify a const value

int numberOfFinalFantasyTitles; // An uninitialized value
const int someOtherNumber; // ERROR: Undefined constant value
```

If we print the value of numberOfFinalFantasyTitles at this point, we might get 0, we might get -19997, we might get 1241510 -- any possible value for the type. Needless to say, under most circumstances this makes it impossible for you as a programmer to reason about the values it takes on. Some compilers will not even allow you to use a value which you don't explicitly initialize (or will otherwise complain with a warning).
Note that because this memory either is or is not zero-initialized at program start, and may (or may not) be based on memory from elsewhere in the operating system, this is not a sufficient source of "randomness", if that's what you want. 
Why isn't it possible for the unitialized const value to take on these "semi-random" values? Consts are baked into the executable. Their value is set at compile time, not run time. It doesn't make any sense for this const value to be "empty" at any point.

Of course, variables are useful because we can *assign new values* to them. The assignment operator in C/C++ is "=". This is also called "setting the value of" the variable, or "storing a value in" the variable. You can set a variable to be equal to another variable.
``` c
int age = 29;
age = 8;
age = 5;
int x = 26; // age: 5, x: 26
age = x; // age: 26, x: 26
x = 90; // age: 26, x: 90
```

Any expression on the right side is evaluated, and then assigned to the variable on the left side. The right side of the equation can be arbitrarily complex, as long as it is an expression which results in the correct type of value:

``` C
const int birthYear = 1996;
int currentYear = birthYear + age;
currentYear = birthYear + age - age + birthYear - birthYear + age;
currentYear = GetCurrentYearFromSystemClock(); // doesn't exist, but you could find one 
```
The above are acceptable because they produce a result which is either an integer or implicitly convertible to an integer, and all such values have the + and - operators defined. On the last line, we assume that the `GetCurrentYearFromSystemClock()` function exists and returns an integer (or something implicitly convertible to an integer).

When thinking about `const` and non-`const` values, consider that an `int` and a `const int` are two related, but separate, types of thing. One way to reason about it is that a `const int` is an int with all possible mutators (operations which can change it) removed. `const int` can be used in any context where an `int` can be used that does not involve changing it. If we want to make a mutable variable out of a const, we have to declare it separately and assign the *value* of the const to it.
Of course, this relationship holds for any type, not just integers.

This idea is very important: a named symbol, const or not, is a *reference* to a value, *not the value itself*. The compiler checks our program mostly by considering *how we are using references*, not by checking any individual values. 
Some expressions, like `a + b`, can be considered to create a "temporary reference" to their result, sometimes called an *rvalue*, because they usually show up on the right side of any assignment expression. Meanwhile, the *names* used in a program, like `c` in the statement `c = a + b;`, are *lvalues*. `a` and `b` are also lvalues in that context, but get converted to the rvalues they hold, because the addition operator uses the value those names hold.  This is why, say, `c = 4;` means something, but `4 = c;` does not. An assignment cannot take an rvalue on its left side because, much like in real life, redefining 4 does not do anything meaningful for us (and if it did, many of our other assumptions would no longer hold).

## Basic Debugging: Printing variable values
One of the most common ways of debugging or logging your program is to print the value of a variable. Until we learn how to properly use a debugger, you can use this simple method to log values:

```c
#include <stdio.h>

//...
int myInteger = 5;
float myFloat = 2.3;
char myChar = 'd';
printf("myInteger: %d", myInteger);
printf("myFloat: %f", myFloat);
printf("myChar: %c", myChar);

// You can pass multiple at a time, too:
printf("Integer: %d, Float: %f, Char: %c", myInteger, myFloat, myChar);
//...

```

The first parameter to printf is a "format string". The %, followed by a certain letter, inside the format string acts as a placeholder. These placeholders are replaced in the printed console line by the values. The specific letter used determines how each value is interpreted when printing. 

In C++, you can use the `<iostream>` header to do this a bit more easily, and don't need any funny business:
``` cpp
#include <iostream>
//...
std::cout << "MyInteger: " << myInteger << ", MyFloat: " << myFloat << std::endl;
//...
```

This method will be extremely useful in logging the behavior of your programs, and help you better understand what is going on. Sprinkle in console output commands like this very liberally while you are learning. 

You can also *get* a value from the command line using `scanf` in C or `std::cin` in C++. This is useful when you want to change values your program is using without recompiling. More generally, it's used to accept input from a user.
```c
int x;
// scanf, like printf, needs a format string to know how to interpret data.
// use Google to find these format speciifers.
scanf("%d", &x);

printf("The value of x is now %d", x);
```

``` cpp
int x;
std::cout << "Please enter an integer:" << std::endl;
// The following line uses the "extraction operator" >> to get input from the command line
std::cin >> x;
std::cout << "You entered: " << x << std::endl;
```
You may have noticed that C++'s methods of input and output are significantly more flexible. This is because C++ includes a lot of special rules that detect how types are used in these operators. The compiler automatically knows how to interpret data in this way because of *type deduction*. Basically, it uses context clues to determine what type needs to be used. We will discuss this eventually in an advanced chapter and learn how to enable similar functionality for our own code.
























