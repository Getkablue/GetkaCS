OK, "Error-driven Programming" isn't actually usually considered a programming paradigm.

But the truth is that we deal with errors all the time, even when we are computing in our brains rather than on our fancy thinking machine. And in fact, there are many different programming patterns for dealing with errors of various kinds, so perhaps it is right to call it a paradigm of thinking.

What is an error? An error can be an incorrect result, it could be a recoverable error that we can detect and continue from, or it could be an unrecoverable event that causes a crash.

In a numerical sense we might refer to *error* as being the *degree to which our results are wrong*. For instance, if we only have a ruler with inches on it, any calculation based on the ruler measurements we take cannot be magically more precise than the ruler. In a certain sense, the error in our measurement *propagates* through our calculations.

Perhaps you have noticed by now that we (often) use *floating-point* values in programming, which represent decimal numbers like 3.000 or 3.1459 or 4.000008 . As you already know, these values must be represented in memory as a sequence of binary data. Hence, if floats have a fixed size, then there is only a fixed amount of information representable by a float, and therefore floats can only be so precise. As a result, "floating point error" can accumulate, where tiny amounts beyond the precision of our floating-point data type are continually lost (or gained). Any amount "lost" or "gained" in floating-point arithmetic is basically due to the error in the representation of a decimal value. We sometimes refer to another data type, the "double", which historically represents twice as much binary data available for precision compared to a float. This is especially relevant for very tiny values which are changing rapidly during iteration -- imagine you are dividing by some very tiny number, like 0.00000001. If that number is subtracted by 0.000000002, it is possible, if our data type is not precise enough, that the denominator becomes "0.00000000"...which means we will now be dividing by 0.  Uh-oh.

In computing, we generally take an error to be something that prevents us from getting any result *at all*, even an incorrect one. "Division by zero" is one such error: division as we know it *does not produce a meaningful result if the denominator is zero*. Depending on the system, this calculation might just return a placeholder value  `NaN` (for Not-A-Number), might halt the program, or might just give you comically incorrect results. A compiler can only warn you about errors like this if the error is known at compile time. In most cases, we are interested in passing values known at runtime into mathematical operations like this, but the compiler cannot help us prevent division-by-zero errors at runtime.

## Kinds of Error

A recoverable error is an error that we can meaningfully recover from. It might make sense, if I make a game and a level fails to load, to boot the player back to the main menu and consider the situation handled.

An unrecoverable error is, as the name implies, not recoverable. This could be something like a corruption of program memory (maybe a stack overflow), or more generally it might be something that it just doesn't make sense for us to continue running from. For example, if your program queries a database for some information and the query is malformed, it doesn't make sense to continue (unless the rest of the program is long-lived and runs as a service, or something).

We might describe two common subtypes of error: a logical error and a runtime error. A runtime error might be something like "our program needs network connectivity but we cannot access the network" -- some condition expected at compile time is not being met at runtime. Sometimes these errors are generated automatically by the language runtime and OS (for example, Windows will automatically crash a program under certain conditions and tell you why -- perhaps it tried to load a nonexistent DLL). Other runtime errors can be explicitly raised by the programmer if the condition is detected.

A logical error is sort of equally nebulous. Any compiler error might be considered a logical error, because the compiler is checking that our program's logic makes sense. 

In a certain sense, compiler errors are far better than runtime errors. You, the programmer, can actually see what the compiler is complaining about and even have it point you directly to the problem. I would rather get 100 compiler errors that I can solve than 1 runtime error which will require extensive debugging. Every error that is made impossible by the compilation process is an error that the user will be guaranteed to never experience when they are running the program.

But there's also logical errors which are not and cannot be caught by the compiler. In these cases, we might explicitly raise logical errors in our program when a condition is reached which should be impossible to occur. This indicates to us that our program is wrong in some way, and is useful for complex systems where it's impossible for us to keep every factor in mind. For instance, if I have a function which branches based on some aspect of the input parameter, and in each of those branches I return a different result, then what happens if the input doesn't match any of those branch conditions? I might explicitly throw a logical error there to indicate to myself when it happens that I am not covering all cases.

There are two major means of detecting and handling errors. 

## Error Codes

The first is *error codes*. We've talked before about how programs you run from the command line return a value (hence why `int main()` instead of `void main()`). Getting any return value from a program other than 0 is conventionally used to indicate abnormal program exit (i.e. "failure"). Error codes work on this same principle. You just return from your function a number (or something similar), that represents whether the operation succeeded or, if not, its mode of failure.

You may then, based on the return value, handle the situation. For example, I might do something along these lines:

``` cpp
// For these examples, "1" is taken to mean "not enough memory",
// and "2" is taken to mean "network is unavailable".
int myErrorableFunction() {
	//... here these conditions are stand-ins for actual checks we might perform
	if (!systemHasEnoughMemory) {
		return 1;
	}
	if (!internetIsAccessible) {
		return 2;
	}
	// placeholder for the rest of the logic of the function
	doActualProcessingTask();
	return 0;
}

int main() {
	return myErrorableFunction();
}
```

But what if we need to return a value, too? We can only return one thing, how do we return both a result and our error code? It's simpler than it sounds. Take a parameter(s) by reference in your function, and put the result value(s) in those parameters:

```cpp
struct Color { int r, g, b; };

int myErrorableFunction(Color& result) {
	//... here these conditions are stand-ins for actual checks we might perform
	if (!systemHasEnoughMemory) {
		return 1;
	}
	if (!internetIsAccessible) {
		return 2;
	}
	// placeholder for the rest of the logic of the function
	result = calculateAverageFavoriteColorFromInternet();
	return 0;
}

int main() {
	Color fav;
	return myErrorableFunction(fav);
}
```

Using some advanced tricks, you could also return a sort of "wrapper object" around your result, which encodes *both* information about errors, if any, and the "actual result value" inside. This pattern is extremely common in more modern languages, like `Some<T>` or `Result<T>` in Rust which indicates a thing that can either have a value or be "nothing". Of course, these sorts of structures need to be defined in a way where they don't let you use them incorrectly -- if my operation produced no actual result due to an error, and I go ahead and try to use that result anyway, my program is ill-defined. Hence, languages like Rust **require** you to write code for each possible case. These features are slowly making their way to official C++ as well, but nothing stops you from writing your own or using a pre-built version.

Error-code based error handling, provided that the values are well-known and annotated, can be quite helpful and is a simple way to handle errors. However, you could debate endlessly where this error checking should actually go. And it can lead to messy code, especially with large amounts of error types.

## Preconditions, Postconditions and Invariants

Before we look at the next major error-handling scheme,  let's discuss what we've just seen.

Functions are basically units of our program, representing transformations of data and state. But not every function is meant to be called in every context. Some functions have requirements that need to be filled before they make sense to use -- "preconditions". For instance, a precondition of division is that the value we will use for the denominator is not 0. Sometimes we enforce preconditions explicitly, other times they are implicit. A precondition of almost any function or program is that the system has enough memory to execute it.

A function is said to generate the appropriate *postconditions*. That is, a function which has been executed in the proper context should cause the state and/or data to meet the new conditions we impose on it.

An *invariant* is something that is assumed across our function, something our function does not touch. If invariants are broken then our program cannot be correct.

Thinking carefully about the preconditions, postconditions and invariants for your functions can be very helpful. In general, it is best to write functions which do not assume that some part of state elsewhere is in a specific configuration. If your function is concerned with it, the function should include or require it somehow (perhaps as a parameter). This is also why we generally avoid global variables.

Anyway, it is our duty as programmers to know when we need to check these. But adding these checks all over our function code is messy, error-prone, annoying, and causes a performance hit. We can avoid these issues by generating "unit tests" for our code. Unit tests run our functions in a specific context and *test* to see how functions react to changes in preconditions, as well as if postconditions we expect are met and if invariants are respected. We may cover this in more depth in a lab, dependent on student interest.

## Exceptions

You may be aware of a common crash message on Windows: "Uncaught exception thrown at 0xDEED1212550" (of course, the memory address is a placeholder).

An *exception* is a way of responding to an *exceptional* situation. Using a keyword(`throw` in C++, `raise` in Python), we "throw" an exception from the point it occurs or where we first detect it. The program then *unwinds the call stack* as the exception "flies" back up through the program. When the exception meets an appropriate handling location, code will resume from where the exception was "caught".

We can specify multiple such "catch points" and the type of exception they handle. If an exception reaches the beginning of the program, it is considered "uncaught", and therefore unhandled, so the program will crash (relatively gracefully).

One more thing: In order to catch an exception, we must indicate that we are going to be catching from some code that *may* throw exceptions. So we wrap such code in a "try-block".

Refer to [this guide](https://cplusplus.com/doc/tutorial/exceptions/) for basics of C++ exceptions. You can throw any data type, but it's really recommended that you throw an instance of (some subclass of) `std::exception`, like `std::runtime_error` or `std::logical_error`, but of course there are many more. By constructing an exception before you throw it, you can include a lot more diagnostic information -- like error codes, a "plain english description" of the error, the context (parameters? source code line number?) of the call, and much more. Just make sure you catch by reference! This is an easy tripping up point but you will need to catch by reference to get non-default information. Another point is that if you catch a more generic parent, like `std::exception`, you will catch all derived exception types too. So you can always have more specialized exception-handlers deeper in your program, and more generic exception handlers higher up. 

In Python, you can use very similar structures, with `try:`, `catch:`, and `finally:` blocks. Uncaught exceptions will be printed to the console anyway in Python, which is nice.

Exceptions do have their limitations though. For one, you still need to decide where in your code this error handling is most appropriate, and how to do it -- it can't decide that for you. It's also somewhat difficult to reason about because it subverts the idea of the call stack. Finally, exceptions that arise *while another exception is flying* cause immediate program termination. This might happen if you have an object destructor which can throw an exception. The first exception unwinds the call stack, destroying the object that was created on the stack. In the destructor for that object, another exception gets thrown (for whatever reason), and KERBLAM! Your program is terminated. Note that this **does not** apply to throwing exceptions while handling an exception. In fact, this is good practice! You can catch the exception, log information, then re-throw it or even throw a different exception.

Some people insist that exceptions are the worst thing to happen to error handling and that you should never use them. I'm not convinced. They can be used poorly, yeah, but so can anything else. If they make it easier for you to write your program, use them.

## Conclusion
Most of the programs we will write in this course are for our own pleasure. When our program fucks up, we know it's basically on us, and we'll fix it until it works for us. Hence we might be incentivized to avoid much error handling, only fixing our software when we need to, because we can also trust ourselves not to use the program incorrectly.

But when it comes to making an actual software product, even basic distribution to others needs some form of error handling -- even if it's just to print information that you can use to replicate the issue when you're debugging. Unit testing frameworks can help to automate testing before you ever distribute your program, but setting them up is an engineering task in its own right.

Ultimately, it's up to you how rigorously you want to handle errors. Presented here are just the common ways of doing it.


