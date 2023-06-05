You should have read the notes from lecture 1 up through [[Basic Program Structure V - Functions and Scope]].

Problem 1: Write a program that takes a string as input and counts the number of vowels in the string. The program should use a separate function that accepts a string as a parameter and returns the count of vowels.

Problem 2: Write a program that prompts the user to enter two integers and performs the following operations: addition, subtraction, multiplication, and division. Create separate functions for each operation, and each function should take two integer parameters and return the result.

Problem 3: Write a program that generates and prints a random password. The program should use separate functions to generate a random uppercase letter, a random lowercase letter, a random digit, and a random special character. The functions should return the generated characters, and the main program should combine them to form a password.

Problem 4: Consider the following program:
``` cpp
#include <iostream>

int a = 10;

void someFunction() {
    int b = 20; // Local variable

    std::cout << "Inside someFunction:" << std::endl;
    std::cout << "b: " << b << std::endl;
    std::cout << "a: " << a << std::endl;
}

int main() {
    int b = 30;
    int c = 40;

    std::cout << "Inside main:" << std::endl;
    std::cout << "b: " << b << std::endl;
    std::cout << "a: " << a << std::endl;
    std::cout << "c: " << c << std::endl;

    someFunction();

    return 0;
}

```
Answer the following questions (all of these fall under Problem 4):
1. What is the output of the program?
2. Explain the concept of scope in programming.
3. What is the scope of the `globalVariable` in the program?
4. What is the scope of the `localVariable`?
5. Can the `b` in the `someFunction()` function access the `b` in the `main()` function?
6. Can the `someFunction()` function access the `a`?
7. If we define a variable with the same name in a nested scope, which variable takes precedence?
8. What happens if we try to access a variable outside of its scope?
9. How can we make a variable accessible across multiple functions?
10. What are the advantages of using local variables over global variables?