Remember earlier when we were analyzing Hello World? We saw that the bulk of the work done in the simple program was inside curly brackets associated with the symbol "int main()", which is also the entrypoint of our program. More generally, the use of curly brackets in C and C++ is to designate *scope*. Scope is the concept which allows us to think in a structured way about how information is available to parts of our program.

## Functions

In mathematics, a function maps an input to some output. Think back to [[0.f  - Types of Data]] -- we can define a function that outputs a type of data called Dog from some number, or vice versa, or that outputs a number given a number... any kind of transformation is possible in principle, including one-to-many and many-to-one relations.  A function can accept multiple pieces of data as *arguments* or *parameters* and produce a single output, or may take one (or even zero) parameters/arguments and produce multiple outputs in the form of a set.

However, a mathematical function doesn't "do anything" to its input. Rather, it creates an output from the input without changing the input -- it's not like getting the square root of 4 *changes* the number 4. Rather, our function points to a new value based upon the value we put in. Furthermore, a mathematical function has no side effects. Nothing happens out in the world when we add 2 and 2 together. This type of function, which does not change its inputs and which has no side effects, is called a "pure function" in the programming world. 
In general though, side effects are useful to us in programming (like printing to the console). And it is useful sometimes to change things which already exist (i.e. "state"). We can loosen our definition to include these things, and this is what a typical "function" is in programming. In that sense, a function is just any set of steps, which may take some input and may produce some output.

"int main()" is a function. A function is a discrete chunk of your program. A set of steps can be made into a separate function by defining a return type, name and (optionally) parameters/arguments for that function, and placing those steps inside the *function body*. Then, those steps can be called by *calling* the function, optionally placing its *return value* into a variable.

A very basic function declaration (and definition) might be like this:
``` c
int myFunction() {
	// everything in here is the function body
	printf("Hello from myFunction()!");
	return 5;
}
```

Note that the function declaration and definition do not execute the function. They just declare what it is and what it does. For this reason, their position in the source code is pretty much irrelevant, they just need to be findable... although it's a good idea to keep functions in places that make sense.

A *return type* is the type of data your function returns. When a function returns a value, it exits, and yields the value back to the *caller* (the place which called the function itself). In this context, the function you called is the *callee*. A function can also have a return type of "void", in which case it doesn't return values, it just returns without passing back a value. "int" is the return type of myFunction above, meaning it returns an integer. A function can also have a "void" return type, indicating that it doesn't return any data to callers (in which case it is probably doing something through side effects, like rendering data to the screen, or printing to a console, or modifying some value elsewhere in the program).

The name of a function is ... its name, or the symbol it goes by, i.e. how you invoke it. Technically, for "int myFunction()", its name is "myFunction". You call a function by its name, plus an open parenthesis "(", plus any arguments you want to pass to the function, followed by the closed parenthesis ")". Since "myFunction" takes no parameters, you can just follow the name with ():

```c
myFunction(); // this is a statement and will call the function
// Because myFunction returns an int, we can store the return value in an int:
int someResult = myFunction();
```

Please be sure to include the () even if there are no parameters. The two code lines below mean two *massively* different things. We will discuss why soon, but for now, consider that *myFunction itself* is a thing. The *result of calling myFunction*, used on the second line, is a very different type of thing (and, while you are at this stage of learning, is almost certainly what you want). 

``` c
int someResult = myFunction; // Probably an ERROR, possibly strange results.
int someResult = myFunction(); // You probably want this.
```

You can add arguments to a function in its declaration/definition. Note that each argument must also be given a type:

```c
int calculateAge(int birthYear, int currentYear) {
	return currentYear - birthYear; // inaccurate, but close enough...
}
```
We can also specify default values:
``` cpp
int calculateAge(int birthYear = 1996, int currentYear) {
	return currentYear - birthYear;
}
```

The names of parameters or arguments to a function are names for the *input it accepts*. The name that the function calls its input has nothing to do with *the name of the input actually passed to it.* For example, even if I have a variable somewhere called `a`, I can declare and define a new function with a parameter called `a` -- the two do not interfere or collide in any way. Inside the function, `a` will refer to its input, not to anything external. And of course, if I have variables `a`, `b`, `c`, and `finkeldorf`, any of them could be passed into the function, assuming they are the right type. Think of a function's parameters/arguments as being slots that accept any data which fits the type required -- the name for those slots is just a convenience.

A function's *signature* is the combination of its (ordered) argument types. Two functions with different signatures can be given the same name, and they are still technically separate functions -- the compiler determines, to the best of its ability at compile time, which one you mean when you call the function by name. Functions with the same name, but multiple possible sets of parameters/arguments, are referred to as *overloaded functions*.

### Function Overloading
``` c
int squareFunction (int a) { printf("squaring an integer\n"); return a * a; }
float squareFunction (float b) { printf("squaring a float\n"); return b * b; }
// Both of the above are OK -- same name, different return type, different args.
// You could define another squareFunction which returns int and takes any other type of input as well. You just can't make two functions with the same name and same signature, or two functions which differ only in return type.

// Note that this is NOT valid in combination with the first function above
// (But could work fine with the second function).
// Different return type, but same signature otherwise.
float squareFunction(int a) { 
	printf("Squaring an integer and returning a float\n"); 
	return float(a * a); 
}
```

### Const-qualification
Much you can declare *const* variables (by *qualifying* the variable declaration), you can const-qualify function parameters and return types in the same way, although the meaning is slightly different. When declaring variables, *const* means that the variable only exists once and is immutable. 

In a function declaration, marking a *parameter* as `const` means that the function may not modify that parameter. One way to think about it is that `const` is a sort of "tag" on any other type. Any type can be implicitly converted to its own `const` version, so it's not a problem to pass a non-const int into a function that takes `const int`, but the opposite is not true: passing a `const int` into a function that expects a (normal non-const) `int` will not work, because the function doesn't guarantee that it will not modify the integer. A  `const int` cannot be implicitly be converted to a regular `int` that a function requires.

You can also `const`-qualify a return type. However, this isn't really relevant for primitive types and values being returned from the kind of functions we have been writing. It only really makes sense when returning a *reference* to some larger objects, which is not something we are ready to tackle yet. 

Our `squareFunction` might be more like:
```c
int squareFunction(const int a) { printf("Squaring!\n"); return a * a; }
```

### Declaration vs Definition
So far, I have used the terms "declaration" and "definition" together for functions. This is just because, at this stage, it is likely that your function declarations are also definitions. *Declaring* a function is a statement of existence. *Defining* a function is giving the implementation (i.e. the function body) for that function. Note that defining a function is automatically also a declaration if one does not exist yet.

These do not need to happen at the same time, though -- in fact, defining functions separately from their definitions is considered a best practice. Think of it this way: the declaration is the *interface*, or in some colloquial terms the *contract* the function fulfills. (Hopefully also the name helps explain what it does, but documentation also helps, especially when the function has side effects or preconditions of some kind). *How* it is accomplished is not really important to the code that calls the function -- the people relying on the function only care about what it does. As an analogy, if you hire someone to clean your house, you don't really care how they clean it: as long as the job gets done and they don't destroy your property in the process, you are happy. Soon we will experiment with this separation ourselves. We'll get there! For now, just focus on writing good functions.


## Scope

Any time you use a set of curly braces in C/C++, you are managing *scope*. 

Scope can be thought of as existing in a hierarchical structure which manages the lifetime of variables and other symbols. Every set of curly braces sends you into lower and lower scope, i.e. more specific. Higher levels of scope do not care, and in fact cannot see, variables in a lower scope. **Variables, functions, classes/structs and other symbols are usually only visible from the scope in which they are declared.**

"Global scope" refers to that which is available from anywhere in your program. Things that exist at global scope are globally accessible. This is in contrast to "local scope", where things are accessible only from directly related functionality. A consistent scope helps the language (and compiler) ensure that your program is well formed.

This all may be a bit abstract. Consider the following program:

```c 
#include <stdio.h>

const float pi = 3.1459;

float GetCircumferenceFromRadius(float radius) {
	printf("Calculating circumference...");
	return 2 * pi * radius;
}

int main() {
	float radiusA = 0.1;
	float radiusB = 2.5;
	float circumferenceA = GetCircumferenceFromRadius(radiusA);
	float circumferenceB = GetCircumferenceFromRadius(radiusB);
}
```

In the above code, we see two sets of curly braces, and each is associated with a function. Each function has its own *local scope*. The variable `radiusA` is not accessible from anywhere outside of main. Even functions which main() calls, like `GetCircumferenceFromRadius`, *cannot access `radiusA`* unless explicitly passed the value. Here, we pass `radiusA` as a parameter to `GetCircumferenceFromRadius`. Parameters of a function naturally exist within that function's local scope, so passing values as parameters transports them across these scope boundaries.

Once we exit from `main`, the variables inside it are *destructed*. In general, once you go "out of scope", any things declared in that scope are destructed and no longer exist. Local variables in a function are destructed once the function exits. 

Note that "`pi`" in this program exists globally and can be accessed anywhere. It exists at *file scope*, so any function we write in this file (or in any file which *includes* this file) can access the constant `pi`.

"File scope" is generally a very high level scope -- anything that is just declared somewhere in a file **not inside a class/struct, function or smaller scope**, exists at "file scope". If it's declared at file scope, it is accessible anywhere that is in, or includes, that file. For the purposes of a program, it always exists, but may need to be found somewhere in a "namespace" (see the section below for a brief note on namespaces). But basically anything at file scope can be accessed anywhere.

"Function scope" refers to variables and other symbols which exist "inside" a function. Variables in function scope exist only as long as that function is being executed -- once it ends, they cease to exist (there are exceptions to this rule but it is not important now.) For example, across multiple calls to `GetCircumferenceFromRadius`, the "radius" variable exists once for each call, and ceases to exist once the function returns. There is no way for different calls to `GetCircumferenceFromRadius` to access the same value (without the aforementioned exceptions to the rule). Note that we also cannot declare functions inside other functions without yet another exception to the rule. Even if we do, they are only visible inside the scope in which they are declared.

"Class scope" or "struct scope" refer to variables, functions and other symbols which exist "inside" a class or struct. We have not tackled these yet, but they are basically groupings of data and functions which act as new, custom data types. We will learn more about these (very) soon.

However, scope rules apply to smaller scopes too. Consider the following situation:

```c

def myComplicatedFunction(float parameter1, float parameter2, float parameter3) {
	// ... A lot of code ...

	int someLocalVariable = 3;

	if (parameter1 > 6.3)
	{
		float innerVariable = 2;
		innerVariable = innerVariable * someLocalVariable;
		// Do some stuff with the two variables...
	}

	// ... even more code ...
	return innerVariable * 2.0; // ERROR! innerVariable doesn't exist in this scope
}
```
If compiled, you receive an error at the return line because `innerVariable` does not exist. Why? because it was declared inside the curly braces for the if-statement. If you need a variable in an outer scope, declare it in an outer scope. This is simply fixed by moving the declaration of `innerVariable` to above the if-statement. On the flip side, `someLocalVariable` exists throughout the entire function, even inside inner scopes, because it was declared at the top-level of the function.

## The `static` Keyword

Remember how functions cannot access the same local variable between different calls of the same function?

Remember the exception I mentioned?

The `static` keyword is an example of a *qualifier*. A qualifier is just a term added onto another declaration to change its behavior in some defined way. A qualifier you've already seen is the *const* qualifier. 
Marking a variable as `static` causes it to be shared across all instances of the scope it is declared in. For instance:
``` cpp
int functionA() {
	static int executionCount = 0;
	executionCount++;
	return executionCount;
}
```
If we call `functionA()` above twice, the first call will initialize the static variable integer `executionCount` and increment it by 1, then return the value 1. The second call will *not reinitialize the value*. It will just increment `executionCount` again and return 2.



