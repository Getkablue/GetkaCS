Let me start these notes with a disclaimer. 

This cluster of concepts is a difficult one, and causes trouble for many programming students. There are both theoretical and practical hurdles here (for example, pointer syntax is ass, and human language is quite bad at describing the differences between reference and value). Furthermore, these are all sort of entangled together, and so must be presented together for us to get the full picture. For this reason, this section may be quite long. 

However, if you can get a strong grasp on these concepts, a lot of doors open to you in programming and many systems you can work with will start to make more intuitive sense. Please reach out at any time for help with this section or the corresponding assignments. I also strongly encourage you to seek out help elsewhere on the internet to find something that makes sense to you. This applies to this entire course, but here it is especially important. 

**Upon reading these notes, several cutscenes will play in sequence.  It is recommended that you set aside sufficient time to view these scenes in their entirety.**

## Pass-by-value and Pass-by-reference

So far, we have written functions which take mostly primitive types, and we have learned what it means to pass or return `const` or non-`const` values. But we recently learned how to make more complex, mutable types.  There is an **extremely important** concept we need to talk about before we really get to using these.

Look at the following program and think. What do you expect to see in the console output?

``` cpp
#include <iostream> 
struct Color {
	int r, g, b;
};

int myFunction(int a, int b) {
	a = a + b;
	return a;
}

void myFunction2(Color color, int blue) {
	color.b = blue;
	std::cout << "Color: (r =" << color.r << ", g = " << color.g << ", b = " << color.b << ")" << std::endl; 
	return;
}

int main() {
	int someValue = 1;
	int someOtherValue = 6;
	// At this point, someValue is 1 and someOtherValue is 6. No need to log here.
	int result = myFunction(someValue, someOtherValue);
	std::cout << "Result: " << result << std::endl;
	std::cout << "SomeValue: " << someValue << std::endl;

	int blueValue = 159;
	Color myColor = {0, 0, 0};
	// At this point, color.r = 0, color.g = 0, color.b = 0. No need to log here.
	myFunction2(myColor, blueValue);
	std::cout << "Color: (r =" << myColor.r << ", g = " << myColor.g << ", b = " << myColor.b << ")" << std::endl; 
	
}
```

After you know what you expect to see, run the code above. Does something seem... wrong?

I know, I know. You feel violated. What just happened seems to defy any form of sense. Take deep breaths. It will be OK. In my experience, this is one of the hardest things to explain to beginners, but it does eventually make sense.

The phenomenon that explains what you are seeing is called "pass-by-value" semantics. To pass *something* by value means that it is the *value of the thing itself* that matters, not the symbol which stores it. When we talk about mathematical functions which operate on a number, we are passing in that number "by value". If we mentally "get the square root of 16", i.e. call the function "sqrt(16)", nothing *actually happens* to the 16. If the sqrt function can be considered to have consumed anything, then it consumed "a temporary number" with the *value* 16. Similarly, if we run `a = 3; a = 5;`, it's not like we *destroyed the number 3*. 3 was just a value that `a` took on, and any number of variables are free to take on the value 3.

"Pass by value" works the same way. It treats parameters as being freely interchangeable with their values. Therefore, by default, a function copies whatever was passed into it and performs all operations on that copy instead. This is useful for mathematical functions and functions working on small, easily-copyable primitives which essentially *are* their values (also known as "value types"). Among a value type, any two objects with the same value *are literally the same thing*. 4 *is* 4, 0.00001 *is* 0.00001 -- "a", "b" and "c" can all refer to the same number separately. 

On the other hand, some types of data represent something *by virtue of existing as an independent reference*. For example, let's say we have a struct called `ColoredCircle` which represents (you guessed it) a geometric circle which has a color. If we instantiate a `ColoredCircle`, we are saying that a new `ColoredCircle` exists. Even if two circles in existence at the same time have the same origin point, radius and color, we still say that two different circles exist. At some point, it may be meaningful to change one particular aspect of the `ColoredCircle` , such as growing its radius. Hence, we want to refer to colored circles by *which one we want to operate on*. A *reference type* allows us to manage this. A reference type can be passed into a function to operate on that thing in a way that modifies the *original thing*, rather than on a "value copy".  By analogy, even if there is a shadow version of me which has all the same values for its data fields as I do, you are still capable, as a human, of referring to me and my shadow clone separately, because whatever type I am has *reference semantics* to you. Whenever you assign a name to a thing, that name is a *reference* to the underlying thing.

In contrast to pass-by-value is a type of parameter-passing known as "pass by reference". When you pass by reference, you pass in a reference type instead of a value type. This allows changes you make to parameter objects to affect the original objects. You can think of it as passing in *the name* or *the address* of a thing, rather than the thing itself.

In C and C++, functions determine how each type is used in their own context. For example, if I were writing a function that modifies a color, I mark the parameter as a *reference* to a Color using an ampersand (&), instead of a Color as a *value*:
``` cpp
void myColorFunction(Color& color, int blueValue) {
	color.b = blueValue;
}
```
Note that not much actually had to change -- in the function body, we use the `color` reference just like we might use a `Color` object! 

If it helps, you can think of a Color& parameter as taking *a variable name which points to a Color* instead of a Color itself. For instance, if I pass a Color variable named `myColor` into the function, the function body would "become":
``` cpp
	myColor.b = blueValue;
```
The function isn't *actually* changing at all, to be clear. But it's close enough that it may help you understand the concept. 

Symbols, in general, are references to another thing -- meaning that names are generally references. When we say `int a`, we are saying there is a symbol `a` which refers to an integer value. In this sense, any variable or const is "actually" a reference to a value. But we don't often think about them like that, because the symbol `a` can be immediately resolved to its value. For example, `int b = a;` means "the value referred to by `b` becomes the value referred to by `a`". Depending on how you think about it, you might think of `int& b = a;` such that the symbol `b`  *is the symbol* `a`

Try modifying the previous code sample to take references as input, rather than values, and run the code again. What do you see? Happy now?

References can be `const`-qualified. If you do, it acts just like how a `const` would -- you cannot modify the object through a `const` reference.

You can also `return` a reference. If you do, that reference acts as another way to modify or access an object, just like any other reference. To get a reference to an object in general, put an ampersand before its name. For example, consider the following code:

``` cpp
Color myColor = { 0, 0, 0 };

Color& getModifiedColorReference(Color& color, redValue) {
	color.r = redValue;
	return color; // with no ampersand, function may try to return the color value instead
}

//...
Color& another = getModifiedColorReference(myColor, 255);
std::cout << "MyColor: (r =" << myColor.r << ", g = " << myColor.g << ", b = " << myColor.b << ")" << std::endl; 
std::cout << "Another: (r =" << another.r << ", g = " << another.g << ", b = " << another.b << ")" << std::endl; 
another.g = 160;

// What do you expect to see?
std::cout << "MyColor: (r =" << myColor.r << ", g = " << myColor.g << ", b = " << myColor.b << ")" << std::endl; 
std::cout << "Another: (r =" << another.r << ", g = " << another.g << ", b = " << another.b << ")" << std::endl; 
//...
```

Complete and run the above code as an exercise. What do you expect to see, and what do you actually see?

Your compiler can make some inferences, based on return types and variable types, whether or not you are working with a reference or value. In any case, the compiler will usually give good error messages about these -- use these to guide you. For example, if your function return type is `Color&`, then `return color` will return the reference to color. If the function return type is just `Color`, then `return color` will yield a value instead, via implicit ref-to-value conversion.

One key example of a big problem that a compiler will not catch:

```cpp
Color& CreateANewColor(int r, int g, int b) {
	Color color = {r, g, b};
	return color;
} 
```

Is it clear what the problem is here? Think about this, and try to compile and run it. Why won't this work?

Remember rules about scope? 

When a variable has out of scope, any reference to it is, by definition, invalid. Any attempt to then use that reference is then causing behavior which is undefined (and will, most likely, cause a segmentation fault error). Here, we are returning a variable which *was* a local.

Of course, you can also use references in a more straightforward way (credit to W3schools for this example):
```cpp
#include <iostream>
 
int main () {
   // declare simple variables
   int    i;
   double d;
 
   // declare reference variables
   int&    r = i;
   double& s = d;
   
   i = 5;
   std::cout << "Value of i : " << i << std::endl;
   std::cout << "Value of i reference : " << r  << std::endl;
 
   d = 11.7;
   std::cout << "Value of d : " << d << std::endl;
   std::cout << "Value of d reference : " << s  << std::endl;
   
   return 0;
}
```

References are a useful and safe way to handle more advanced problems, especially those where we need to selectively modify complex objects. It's worth noting that, if an object is very large, then the copying triggered by pass-by-value can be rather computationally expensive. Pass-by-reference avoids this, so it can actually be more efficient, too.

As a general rule, just think, when you are writing your function: Am I operating on the *value* of this parameter?Would it be OK if I got an identical copy to work on? Or am I operating on a *specific object* of this parameter? This rule of thumb will help guide you.

## Pointers

Pointers are a type of object in programming which represent the location (or *address*) of another object in memory. **Do not overcomplicate this idea.** You can "dereference" a pointer to get the object it points at. Much like references, this is useful when you need to operate on an object, and you are not sure at compile time which object will need to be operated on. It also allows you to change variables "at a distance" -- as long as you have a pointer to an object, you can modify that object even if that object is directly accessible by your function.

In essence, a pointer is like a reference, but not necessarily bound to a variable or const which exists -- instead a pointer relies on the fact that values are stored in actual places in memory. Instead, a pointer points to a location in memory, and can be used to manipulate that location. A pointer is declared with a typename followed by an asterisk (\*). Here we just use `int`s.

``` c
// Create an integer
int a = 99;
// Get the pointer to that integer
int* ptr = &a;

// Dereference the pointer to do things with it.
// Yeah, the dereference operator is also the same symbol used to designate a pointer. UGH!
*ptr = 4;

std::cout << "Value of a: " << a << std::endl; 
a = 64;
std::cout << "Value in deref'd pointer: " << *ptr << std::endl;

// You can also do arithmetic with pointers, or increment them. These are easy ways to cause segmentation faults unless you know what you are doing.

ptr++; // "shift pointer forward by 1"
int* newPtr = ptr + 4; // "shift pointer forward by 4 ints"
// At this point, ptr and newPtr do not point at a valid location... be careful.
int someValue = *newPtr; // Might crash here with segmentation fault
std::cout << "Value in someValue: " << someValue << std::endl; // if not, enjoy junk!
```

Remember, a pointer is *itself* a type of data (underneath the hood, usually just a memory address in the form of integer of the same size as the system processor's architecture supports). Hence, you can have a *pointer to a pointer*, and so on:
``` cpp
int a = 4;
int* aPtr = &a;
int** aPtrPtr = &aPtr;
int*** aPtrPtrPtr = &aPtrPtr;
//... You can go crazy doing this.
```

As an exercise, try to visualize the above snippet. For each variable, show its address in memory and its value. You can make up memory addresses, or, for bonus points, find a way to print them from the code so you can see their actual values.

The above examples use what are colloquially known as a "raw pointers". Pointers are one of the lowest-level constructs you will commonly manipulate. Remember, the entirety of your loaded executable program (and in fact, any program running on your machine) is just a chunk of memory. Hence, pointers can do extremely powerful and dangerous things. Modifying consts, modifying function code... nothing is really out of the question with pointers. For many advanced applications, especially systems programming, they are essential.

If you dereference a pointer which points to a memory location which does not actually contain the object you are attempting to get and try to do something with it, the result is *completely undefined behavior*. The programming language gives you absolutely *no guarantees* about what happens in this scenario. I will tell you that in reality, the most likely behavior is that you either get a corrupted version of the value you intended to get (caused by the interpretation of junk data as a value), or the operating system's memory protection will become evident and your program will crash. *But as a programmer you are not guaranteed any of this*. Your program crashing is the *best case scenario*. Data corruption is insidious and often hard to detect (the compiler certainly can't do so) and is responsible for many of the most infamous bugs (missingNo. says "H̷̥̘̺͔͖̬̪̞͔̺̳͇͆͑̇̍̀͆̌͌̄̓̒͘̚̕i̴̧̟̤͚͈̺͉̦͙̥̭͓̖̍͋͐̌̿̽̈́̀̓̆͛̇̐ͅ".)

Seriously:  missingNo., M' and Glitch City in Pokemon, World -1 in Super Mario Bros.... Many of those classic glitches are due to a pointer, which assumes memory is loaded in a certain place. That data may not actually be loaded there under certain conditions, which means you end up reading *something else* and interpreting it as a Pokemon or as a level. These days, many operating systems and compilers include built-in memory protection -- basically, the OS "hides" which memory addresses your process is actually running in, and parts of memory get marked as readable, writable, executable, or some combination. The result is that each process has a sort of "managed sandbox" in which it operates. Attempting to write read-only memory will cause a crash. Attempting to write to memory which hasn't been reserved will cause a crash. You get the picture. As I mentioned, unless you are doing real low-level systems or reverse engineering work, these things are all there to help you and prevent things like missingNo (and if you are doing those things, there are tools to disable or work around them). Imagine having something that destructive on your personal computer, not just a Gameboy...

Many newer languages, like Rust, attempt to handle pointer safety at the language level (which C and C++ do not), in order to be safer. The problem of accidentally dereferencing a null pointer has been referred to as "The Billion Dollar Problem". Later in this section we will discuss so-called "smart" pointers, which is an attempt by the C++ committee to resolve many of the issues with raw pointers. 

## Memory Allocation

Sometimes when writing a program, you need to allocate space in memory for yourself to work with. You may not have realized it, but if you have been following along, you have been allocating memory all this time. Every time you declare a variable in a function, you are also assigning space for that variable *on the stack*. 

"The stack" is a complicated topic that could use a section on its own. For now, know that a *stack* is a type of data structure with variable size, which acts like a stack of plates: the first item in is the last item out. Surely you have noticed this pattern: When a function calls another function, the latter function must exit first. Only then may the former function exit. 

*The stack* refers to a specific stack: the one that is specified by the C/C++ specification and used to call functions. This stack does not exist in the language itself, but rather exists in how functions are translated from C/C++ into machine code. The gist of is that each function call adds a "stack frame" of the proper size onto the stack. The "height" of the stack depends on how long the chain of function calls is, going all the way back to `main()`. Anyway, each function allocates space *on the stack* when it is called to contain all of its local variables. For the sake of completeness, global variables and consts go to another segment baked into the executable which has a known size. When a function exits, it is removed from the stack, and that space becomes available again. We might not know how many times a function will be called, or how high the stack will go, but we do know how much space each function takes per call. 

Hence, variables declared in a function are *allocated statically* on the stack -- i.e., in a way that does not depend on runtime parameters. In addition, the stack is constantly changing -- what do we do if we need to allocate memory that will last longer than a function's lifetime? We can't use the stack for that.

For cases where the amount of space we need is determined at runtime, or where the memory needs to exist for a longer lifetime than the function that allocates it, we need *dynamic allocation*. Memory allocated dynamically does not go on the stack. Instead, it goes to "the heap", which is another type of data structure. The operating system allocates heap space for your process when you run it. Thus, the heap acts as a repository for all dynamically allocated memory.

In C, you call the function `malloc` to allocate memory on the heap. In order to use this, you actually need to know how much space to create. To find this, we often use the built-in `sizeof` function to find the size of an object in bytes:

```c
// Allocate enough space for 100 integers, get a pointer to the first address
int* ptr = (int*) malloc(100 * sizeof(int));

// Check if the memory has been successfully allocated by malloc or not
if(ptr == NULL) {
	printf("Memory not allocated.\n");
	exit(1);
}
```
The above code calls `malloc` and requests space sufficient to store 100 integers. On a 64-bit system this will be 800 bytes, on a 32-bit system this will be 400 bytes.  If `malloc` is successful, it will return a generic (void) pointer to the first address. Then this code *casts* the result to a pointer-to-integer with `(int*)`. *Casting* a value of one type to another type basically tells the compiler explicitly to perform a conversion. At this point, we check if the pointer returned by `malloc` is NULL (i.e. all 0s), which is the sign that `malloc` failed. Since we have no idea what to do if no memory is available, we just exit with error code 1. Assuming that's not the case, we can actually use our memory now.

But we only *allocated* the memory, we didn't fill in the space with any meaningful data. In fact, if used as-is, we will probably be reading junk data from the system. To actually assign values to this space, we have to dereference the pointer and move the pointer around in the allocated space:

```c 
int* beginning = ptr;
for (int i = 0; i < 100; i++) {
	// Yeah, the dereference operator is also the same symbol used to designate a pointer. UGH!
	*ptr = i;
	ptr++;
}
```

The above code does a couple things. First, it keeps track of where our space begins in `beginning`. It iterates through 1-100, and at each iteration sets the space *pointed at by the pointer* to have a value based on our iteration counter. It uses the pointer increment operator in each iteration, which shifts the pointer forward in memory to point to the next location for an `int`. From here, the values 1, 2, 3... 100 are laid out linearly in memory. You could then use the pointer `beginning` to iterate over the values again if they need to be read for some reason.

In C++, you can also use the *new* operator to allocate just enough space for another object and instantiate it at the same time. If we made a new class called Object, then:

```cpp
Object* obj = new Object(); // new creates a pointer to the newly created object
obj->doSomething();
```



Both `malloc` and `new` ultimately request the operating system to provide more memory to the process. (Actually, you can do fancy stuff to re-define `malloc`/`new` to allocate memory some different way... but that's way too advanced for this course). It is worth noting that this process is both relatively slow and, more importantly, *can fail*. The operating system can tell you "No, there's not enough space left". How your program handles that is up to you. On the other hand, we can call `malloc` or `new` inside a function to create an object, then access that object outside of the function.

Naturally, what we just described is very powerful: you can allocate space for all sorts of stuff used in your program, and access it from *anywhere* provided you have a handle to that memory! Unfortunately, by invoking dynamic allocation, you give up most safeguards, too. The compiler has no way to verify that you are using the heap properly -- you are taking responsibility for this yourself when you dynamically allocate. 

More importantly, if you leave memory allocated, the program does not release it automatically. That memory will never be returned to the operating system until you deallocate it (also known as freeing it). Keeping memory allocated longer than it is needed, or worse, never deallocating it at all, is known as a *memory leak*. As your program keeps allocating memory without freeing it, it continually "leaks" memory into the process's memory pool which the OS cannot reclaim until the program terminates (by crash or otherwise). This is already bad for programs running as processes... but if you're writing driver or device code, this memory often *will never be reclaimed until the system restarts*. Yuck!

Even worse, if your code loses track of which memory it has allocated, (i.e. doesn't maintain a handle to that memory), you have no reliable ways of identifying that memory again so that you can free it. Basically, if you ever lose track of memory, it's lost forever. To combat these problems, some languages, such as Java, use *garbage collection*, or GC. This type of system automatically tracks which objects in memory are still being referenced or used, and occasionally cleans up the memory which isn't in use. Unsurprisingly, this process is expensive and tends to manifest as stutters during program execution (Minecraft, anyone?)

Neither C nor C++ use garbage collection natively, so we're stuck writing our own code to free our dynamic allocations. In C, you pretty much always need to use `free` explicitly to free memory. In C++, you can use the `delete` operator to free the memory for a given object pointer, and can often manage this using object destructors.

In C:
```c
free(ptr); // frees all the memory from the initial allocation.
```

You may be wondering how `free` can know *how much* needs to be freed. It turns out that malloc stores some additional information *before the start of the pointer it gives you* which marks how much space was initially allocated, and free simply reads this from the memory location in order to know which memory to free (see [this stack overflow answer for details](https://stackoverflow.com/questions/1518711/how-does-free-know-how-much-to-free)). Because of this, it is important to give to `free` the same pointer which was initially given back by `malloc`.

In C++:
```cpp
delete obj;
```
Note that, if an object stores a pointer to another object, that pointer will *not* automatically be deleted as part of the object destructor when the storing object goes out of scope (or whatever else causes it to be destructed).  Instead, if you have an object that manages some pointed-at object like this, you should place the `delete` call in the object destructor.


### Smart Pointers

In order to retain the usefulness of pointers while reducing the possibility for misuse, C++ introduced the idea of "smart" pointers in newer versions. These smart pointers are object types which have semantics that the compiler can verify, and automatically destruct the underlying object whenever they expire. They can manage any type of object using C++'s powerful generics system --  a `std::shared_ptr<Circle>` manages an object of type `Circle`.

For example, a `std::shared_ptr` uses *reference counting* to know when the object can be deleted. Whenever something else grabs a reference to a `std::shared_ptr`,  the reference count goes up. When an object holding a reference is destructed, the reference count goes down. When the reference count reaches 0, the shared pointer can self-destruct and delete the underlying object.  

A `std::weak_ptr` is like a shared pointer, but has a "weak hold" on the object. This is useful for scenarios where objects in `std::shared_ptr`s hold cyclic references to one another. If two objects hold `std::shared_ptr`s to one another, neither will never actually destruct! The `std::weak_ptr` avoids this problem -- if the only references left to an object are `std::weak_ptr`s, that object will still destruct itself.

There is also the `std::unique_ptr` . This smart pointer enforces a "one reference only" rule. In order to pass around this type of pointer, you have to pass it into functions with a special function, `std::move`, and return the same `std::unique_ptr`.

Because a smart pointer to a type of object is its own type of object, functions must be explicitly written to accept these if you want to take full advantage of their features.

## Conclusion

Obviously, this is a lot to take in. Don't worry about specifics so much for now. Instead, focus on understanding the concepts involved. I am not going to expect you to be using smart pointers like a professional just yet. Stick to some basic principles:
Use references instead of pointers whenever possible, use smart pointers instead of raw pointers whenever possible, and only dynamically allocate memory when needed.

















