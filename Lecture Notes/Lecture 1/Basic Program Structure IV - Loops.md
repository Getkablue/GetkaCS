Often when solving a problem, your task involves either doing some set of steps until you some solution state is reached, or performing a set of steps for multiple items in a collection. **Loops** help us accomplish both of these. Code segments in a loop act as a hotspot of activity -- Even if we have to perform a certain set of steps 8 times, 50 times, 300 times, 9000 times... we only need to write the process once, and one additional segment to set up the looping behavior.

Looping is essential for not only problems where a common set of steps needs to be executed a known number of times, but also where the number of iterations is based on something that isn't known until runtime. 

There are generally two types of loops, the *while-loop* and the *for-loop*. 

## While-loops
A *while-loop* continues to execute as long as some condition is true. The general form is like:
``` c
while (condition) {
	statements;
}
```

But a loop eventually needs to end. We can break out of a loop either immediately with the statement "break;", or at the end of the current loop iteration, by changing the condition so that it is False. 
```c
bool gotCorrectAnswer = false;
int currentAnswer = 0;
while (!gotCorrectAnswer) {
	currentAnswer++;
	printf("Trying another answer...\n");
	if (currentAnswer == 42) {
		gotCorrectAnswer = true;
		printf("Found the correct answer!\n");
	}
}
printf("Done.\n");
```
OR
```c
int currentAnswer = false;
while (true) {
	currentAnswer++;
	printf("Trying another answer...\n");
	if (currentAnswer == 42) {
		printf("Found the correct answer!\n");
		break;
	}
}
printf("Done.\n");
```
Which one is preferable depends on your situation and what exactly you need to accomplish.

There is also something called a do-while loop:
``` c
do {
	doSomething();
} while (condition);
```

This type of loop will always run at least once, whereas the original while loop will check its condition before it ever starts. Sometimes this makes a difference.

Game engines usually have a main loop that they run inside. All the magic happens during the individual iterations of the loop. Something along these lines is at the core of every engine
```c
bool quit = False;
while (!quit) {
	processCollisions();
	updatePlayerState();
	updateEnemyState();
	render();
	if (keyboard.QuitKey.isPressed()) { quit = True; } 
}
```

## For-loops
A *for-loop* is a little more complex. A for loop contains an *initialization statement, a continuation condition, and an iteration statement* all at once. The basic form is:
```c
for (initialization; condition; iteration) {
	statements;
}
```
This syntax is pretty stupid -- this is one of the few cases in C/C++ where a semicolon does not indicate a new statement in sequence. Rather, here, it separates the initialization statement, the continuation condition check, and code to run at each iteration.

One of the most common patterns in the C/C++ world is using a for loop for dealing with a collection of objects. In this case, we'll use the collection type called "std::vector", and declare that it contains integers. We'll elaborate on this later, but for now, just understand that it is an ordered collection of objects.

```cpp
std::vector<int> myNumbers = {44, 23, 37, 9, 3, 99}; // Set up the vector
int sum = 0;
for (int i = 0; i < myNumbers.size(); i++)
{
	sum += myNumbers[i]; 
}
```

There are a lot of new things here. Again, take it line by line.
The first line just creates a std::vector with some numbers. 

The for-loop does the following: first, creates a new integer variable, which we use like a counter, and initializes it to 0. The second statement compares the current counter to the size of the collection. We loop over the items for as long as the counter is less than the size of the collection (i.e., we only do this for as many items as there are). At the end of each iteration we increment i by 1. 
In the inner body of the loop, we use the "+=" operator. This sets the left side equal to its *current value plus the value of the right hand side*. In this case, on the right hand side, we use the *indexing operator \[\]* to find the "i-th" element of the collection (i.e., the current element we are on).
This loop sums all the numbers in the vector.

You can get more creative with the initialization, condition and iteration parts of the for-loop, but the above pattern is extremely common for a reason.

Loops can be *nested*, to achieve looping behavior within loops. This pattern is common, say, for 2-dimensional arrays of values (like images), but also for cases where each you need to perform an operation between every combination of elements between two sets.

For example:

```cpp
std::vector<int> fibonacci = {1, 1, 2, 3, 5, 8, 13};
std::vector<int> primes = {1, 2, 3, 5, 7, 11};
std::vector<int> results; // Initializes to empty
for (int i = 0; i < fibonacci.size(); i++) {
	for (int j = 0; j < primes.size(); j++) {
		results.push_back(fibonacci[i] * primes[j]); // innermost loop body
	}
}
```

The above code creates two collections of numbers, and one empty collection for results.
*For each* element of "fibonacci", we get a multiplication result *for each* element of "primes" and push it to the results collection. For two collections of size *n* and *k*, we execute the innermost loop body *n \* k* times, so we will have that many elements in "results" at the end.

For completeness, and to demonstrate a feature which is common in newer languages, I must tell you about the "range-based for-loop", which exists in later versions of C++.  This will only function in C++ mode, not C, and only if your compiler supports newer features:

``` cpp
std::vector<std::string> fruits = {"apple", "banana", "orange"};
for (auto& item : myNumbers) {
	std::cout << item << std::endl;
}
```
The range-based for-loop lets you express the idea more succinctly, and generically, of working over every element of a collection. But it only works for collections that implement certain operations. In any case, the inner body of the loop must be possible to execute for the type of object being iterated over (otherwise, your compiler will yell at you).

## Things useful across loops
The `break` statement will cause you to break out of the current loop. The current loop is exited immediately.

The `continue` statement will cause execution flow to immediately jump to the beginning of the *next* iteration. Conditions are still checked as normal. You might use this any time you don't need the current loop iteration to complete and just want to skip forward. For example, let's say you need to do some expensive/slow processing for each element of a collection, but there is an easy heuristic which returns the right result for some of the elements. When that heuristic is correct, you just *continue* afterwards, so you don't waste processing time running the more expensive processing.

You can always `return` from inside a loop, even if nested. The function immediately exits. This is useful when you are looking for whether a certain thing is in a collection -- as soon as you find that thing, you can just immediately `return true`, you don't need to run the rest of the loops.






