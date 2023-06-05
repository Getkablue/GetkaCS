In the previous notes, we have taken a simple program and broken it down line by line. Now, we will discuss common basic structures used to define programs of increasing complexity.

## Control Flow
*Control flow* is a 1st-level Arcane spell which... nevermind. *Control flow* is just an expression reflecting how a computer progresses through steps of your program. Like fluids, electricity or ants, it follows a path through the set of instructions. Even if your high-level program description doesn't include the low-level steps explicitly, it is still possible to describe the program as "executing a certain step" of your program at any given time. (In fact, we can even make a mapping between the steps in the actual executable back to lines in our source code -- this is known as "debug info", and we will experiment with this later.)

Try this now: Write down (as precisely as possible) a set of instructions for making a peanut butter and jelly sandwich. Don't just write "find some peanut butter and put it on" -- be specific and thorough. Then, follow the instructions (by pantomime if you must). While you are doing so, consider "where you are" in the program. This is the essence of control flow. Without a strong understanding of the order in which our code will be executed, we are very unlikely to make a successful program. The easiest programs to understand are linear sets of steps. However, this doesn't reflect the complexity of real problems. Very few real problems can be solved without any sort of conditional statement, jumping to a common point, or deferring to some other system for a result.

### Conditionals: If-else
An "if-statement" is the simplest conditional structure for control flow. It operates on an expression which is a condition, and contains a set of instructions. When control flow reaches the if-statement, if the condition evaluates to true, then the associated instructions will be performed. Otherwise they won't be. Conditions are ultimately evaluated via Boolean logic -- they are either true or false.

The format is something like:
``` c
if (condition) {
	statement;
}
```

or more concretely:
```c
bool hungry = true;
if (hungry) {
	eatSandwich();
}
```
In the above code, I will eat a sandwich if the value of "hungry" is True. Since I set hungry to True, and no code here changes the value of hungry, the if-statement will *always* execute the nested code, which calls eatSandwich(). If I set hungry to false, of course, I would not call eatSandwich().

After an if block, you can include an else block. Whatever is in this else block will execute if the if-condition was not met.
``` c
if (hungry) {
	eatSandwich();
} 
else {
	writeMoreNotes();
	writeMoreNotes();
}
```
Here, if I am hungry, I will eat a sandwich. If I am not hungry, I will write more notes. These two *code paths* are *mutually exclusive* in this structure -- I will not do both, only one or the other depending on the state of hungry.

You can optionally introduce any number of other conditionals to the if-else block. This is known in some other languages as "elif" or "else-if":
``` c
if (reallyHungry) {
	eatSandwich();
}
else if (aLittleHungry) {
	eatSnack();
}
else {
	writeMoreNotes();
	writeMoreNotes();
	writeMoreNotes();
}
```
Again, *all paths are mutually exclusive* here, with the top taking precedent. The "else if" conditional is only ever even checked if the first conditional fails. If I am really hungry, I'll eat a sandwich, if I'm not really hungry but I am a little hungry I will eat a snack, and if I am neither then I will write more notes. This mutual exclusivity and order is very important to keep in mind -- I wouldn't want to insert a check for "thirsty" into this structure unless, for some reason, eating and drinking were mutually exclusive. The "else" block will execute if, and only if, no other conditionals were true at the time of evaluation.

When reading your own conditional code, think as straightforwardly and precisely as possible about the conditions you are evaluating and in which order.  

Above, we used bools that currently exist as variables in order to form the conditional part of our statement. But actually, *any expression which returns, or which is convertible to bool* can be used as the condition. For example, the code "x > 5" is not a statement -- by itself, it does nothing.
But rather, "x > 5" is an *expression* which evaluates to a bool value. In this case, this expression is *True if x is greater than 5, False otherwise*. This value can be used in statements. For example:
```c
int heat = 5;
int heatCapacity = 10;

// Some code goes here that increases or decreases heat

if (heat > 5) {
	printf("Alert! High heat, be careful!\n");
}

if (heat > heatCapacity) {
	printf("OVERHEATED! Venting all systems!\n");
}
```

As an exercise, what output would you see if, when the if-statements are reached, heat has a value of 5? 6? 10? 11?

The comparison operators are a classic set of expressions are important to keep in mind:
```c
a > b // "a is greater than b"
a < b // "a is less than b"
a >= b // "a is greater than or equal to b"
a <= b // "a is less than or equal to b"
a == b // "a is (exactly) equal to b"
a != b // "a does not equal b"
```
The meaning of these should generally be clear, at least for numbers. The last two make sense for bools: (True == True is True), (True == False is False), (True != False is True)...

In general, ! before a boolean is the "negation operator" -- !a is True if a is not true.

In C and C++, many different data types can undergo "implicit boolean conversion". For example, integers can *implicitly* (unstated) be converted into bools (True if the integer value is 1 or greater, False if 0 or lesser). So the following code samples are effectively the same:
``` c
int poisonCounters = 1;
if (poisonCounters > 1) {
	printf("You are poisoned! Seek a medic!\n");
}
```
``` c
int poisonCounters = 1;
if (poisonCounters) {
	printf("You are poisoned! Seek a medic!\n");
}
```
This is generally considered to have been a bad design decision, because implicit conversions can cause your code to take on unintended meanings. This case is innocuous enough to be reasonably easy to read and understand in the implicit form, but in general you should be explicit with what you are doing whenever possible.

## Switches

A `switch` statement matches an input against multiple cases, and the first matched case is triggered. 

``` cpp
int a = 1;

void doOneThing() { std::cout << "OneThing" << std::endl;}
void doAnotherThing() { std::cout << "AnotherThing" << std::endl;}
void doSomeOtherThing() { std::cout << "SomeOtherThing" << std::endl;}
void fallDownAndCry() { std::cout << "WAAAAAAH" << std::endl;}

// ... some code that changes a

switch (a) {
	case 1:
		doOneThing();
	case 2:
		doAnotherThing();
	case 3:
		doSomeOtherThing();
	default:
		fallDownAndCry();
}
```

Try completing and running the above code. What happens?

Not what you expected, right? This happens because switches have *case fall-through* (admittedly a very odd default to have...). To fix this, add a `break;` statement to the end of each case's code. Or don't -- you can sometimes exploit the fall-through between cases to get some interesting behaviors. For example, based on the severity of an issue, you might apply all possible fixes, or just a few: 
``` cpp
int heat = 0;
int heatCap = 10;
heat = 99;

if (heat > heatCap) {
	int heatOverCap = 0;
	heatOverCap = heat - heatCap;
	switch (heatOverCap) {
		default:
			std::cout << "WAY WAY WAY too much heat! Exploding!" << std::endl;
			selfDestruct();
			break;
		case 3:
			std::cout << "WAY WAY too much heat! Venting all systems!" << std::endl;
			emergencyVentSystems();
		case 2:
			std::cout << "WAY too much heat! Throttling systems!" << std::endl;
			slowDownSystems();
		case 1: 
			std::cout << "Too much heat! Diverting power to fans!" << std::endl;
			increaseFanSpeed();
	} 
}
```

## Goto 
The `goto` keyword, when used in a function, allows you to jump to a labeled point inside that function. Labels are marked by placing a label and then a colon on a line inside the function, between two statements. 

For example:
``` cpp
int exampleFunction() {
	int a = 0;

step: 
	std::cout << "Executing the step" << std::endl;
	a += 5;
	if (a < 100) {
		goto step;
	}

	return a;
}
```

I know, this seems powerful. It is. A lot of people look down on its use, but it is a perfectly valid construct, and sometimes using it makes complicated problems a little less complicated. Just be mindful of how you use it.











