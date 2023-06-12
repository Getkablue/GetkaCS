At this point, you are probably a little fatigued dealing with just numbers all the time.

Thankfully, structs and classes are here to bring your programming skillset a little closer to reality. Structs and classes are (almost identical) ways to declare *new types of data*. These data-types allow you to create compositions of fields based on other types, and to allow these composed types to have their own semantics and operations.

But a struct or class is a *description of a type* of data. In most programming, we operate on *instances* of types of data (i.e. the data itself), not on the types themselves. Individual instances are also described as objects. So a class/struct is a blueprint or template for an object, and the instances of that struct/class are the objects themselves. By analogy, if Person is a class, then the person called Evan Smith is an instance of the Person class. That class may have a string field, called "name". For Evan Smith, the value of that field is the *string value* "Evan Smith". 

To be clear, only structs exist in C. 

In C++, both structs and classes exist, and in C++, classes and structs differ only by what is called "default visibility" which we won't worry about just yet. If you are curious, just know that it controls which parts of your program can access which parts of your structs/classes. Until we talk about that, just use structs.

In C, structs contain *only* data fields. In C++, structs (and classes) can also contain *member functions*, which are functions that are bound to specific *instances* of the struct/class.  

You access fields of a struct or class using the "dot syntax". The general pattern is `object.field` where object is the object instance you are interested in and field is the field you want from that instance. Note that fields themselves can be objects, so something like `player.equippedItems.chestpiece.defenseValue` is a valid way to access nested field values.

For example, I might declare a new type called Color, which contains 3 integer fields to denote the RGB values:
``` c
struct Color {
	int r;
	int g;
	int b;
};
```

r, g and b in this case are variables declared at class scope. Each *instance* of Color has its own separate values for r, g and b. Hence, variables declared like this are also sometimes known as *instance variables*. If you declared a const here, it would be the same across all instances of Color. For variables which are mutable, but are still shared across all instances of a class, you can use the `static` qualifier described in the last set of notes -- if a `static` variable is declared at class scope, it'll be shared across all instances.

In C, you must mark when a new variable is an instantiation of a struct:
``` c
struct Color myColor = {255, 255, 255}; // Instantiate the struct
myColor.r = 0; // Set the "r" field of the instance to 0
```
Otherwise in C++, things are mostly the same:
```cpp
Color myColor = {255, 255, 255}; // instantiate the struct
myColor.r = 0; // set the "r" field of the instance to 0
```

In C++, we can add member functions to the type. These usually represent common ways to use the data, or common ways to modify it. For example:
``` cpp
struct Color {
	int r, g, b; // same as the 3-line version
	int getTotalValue() const { return r + g + b; }
};
```
See that the member function, `getTotalValue()`, can access the variables `r`, `g` and `b` which were declared *at class scope*.

Note that there is something extra here: `getTotalValue` is marked as `const` in a place that seems odd -- it's not on either the return type or the parameters. `const` in this position means "this function does not modify the object it is a member of". If you don't label a function as `const` in this way, then that function cannot be called through a reference of type `const Color` . Marking functions and parameters as const appropriately will greatly help others looking at your code to understand your meaning, and will make it harder to make mistakes.

Of course, member functions can also be non-`const` if they need to modify the object:

``` cpp
struct Color {
// ...
  int doSomeStuff() { r = 255; g = 0; return 169 }
// ...
}
```

### Constructors & Destructors

In C++, we also often define at least one *constructor* function for the struct/class. This is a special type of member function, with the same name as your struct/class, which creates a new instance of the struct/class given some parameters (or none, if you so choose). It can be used to add any custom setup logic which is needed for your object. The return type of the constructor doesn't need to be specified, and you don't return anything from inside it. You just execute the code that constructs the current instance.
``` cpp
struct Color {
	// ... Rest of the color definition
	Color() { r = 0; g = 0; b = 0; }
	Color(const int r, const int g, const int b) { this->r = r; this->g = g; this->b = b; }
	//... and so on
} 
```
In the above example, we create a "default constructor", `Color()` , which initializes a Color whenever we don't use a more specialized constructor. We also define a *parametrized constructor* which takes three values. Note the use of  `this->` .
`this` in C++ always resolves to the current object pointer. We will discuss pointers in the next set of notes. For now, know that the `->` operator lets you access fields of a pointer. In this case, we use `this->` to disambiguate which r, g or b we are talking about: both the field in the current object and the parameter have the same name, so we indicate that we want the one in the current object. If we didn't do this, we'd probably get an error.  

A *destructor* is another special type of member function, this time designated with a tilde (~) before the class/struct name. You write a custom destructor when your objects need something special to happen when they go out of scope or are otherwise destroyed. The compiler writes a sane default destructor for you, and if your object is destroyed, so are all of its instance variables (i.e., if the instance variables are objects, their destructors will be called, too).

```cpp
class Color {
	//...
    ~Color() {
	    std::cout << "Color is being destroyed!" << std::endl;
    }
};
```

There are some fairly arcane rules in C++ about which member functions the compiler will automatically write for you, and under which conditions. For now, this is none of our concern, and the compiler will handle it most of the time.

## Visibility (`public`, `protected`, and `private`)

"Visibility" refers to which parts of your program can "see" other parts of your program. In the previous set of notes, we learned about scope, which is a type of visibility enforced to make sure your programs are well-formed. We can also set the visibility of object fields and methods (member functions), although this is more for design reasons than it is for program correctness. 

We discussed earlier that a function's declaration acts, in a way, as a *contract* of sorts. It tells us that if we provide a certain type of thing(s), the parameters, then we will get a certain type of thing in return, the return type. The implementation of the function is not exposed to the place calling the function, the caller just wants the returned values.
Visibility for objects is a way of extending this metaphor. If an object is a thing which provides some meaningful state and functionality, then the code using that object may or may not care about the specifics of the object. 

The "public interface" of an object is the set of variables and functions which are *exposed* to other parts of your code. By marking something as public, you are saying "I expect other parts of my code, or other developers using my object, to care about this thing". If they shouldn't care about it, then don't let them touch it -- make it *protected* or *private*. This principle of hiding information, is known as *encapsulation.*

*Protected* fields are accessible only to functions in the class itself, or to functions in *children* of the current class. *Private* fields are accessible only by functions in the current class, full stop.

You mark fields as `public`, `protected` or `private` by qualifying the variable or function declaration with them. You can also mark entire sections by using the same qualifier keyword followed by a colon, as seen in the example below.
For structs, the default visibility is `public`. For classes, the default visibility is `private`.

A common pattern is to make public functions which "wrap"  private/protected fields inside the class. These are sometimes known as "getters" and "setters". Sometimes a private field which is governed by a getter and setter is called a "property".
``` cpp
class Color {	
	// ... rest of the class definition ...
public:
	void setRed(int r) { this->r = r; }
	void setGreen(int g) { this->g = g; }
	void setBlue(int b) { this->b = b; }

	int getRed() { return this->r; }
	int getGreen() { return this->g; }
	int getBlue() { return this->b; }

   // ... rest of the class definition ...	
};
```
Of course, these are functions like any other, which means you can make them arbitrarily complex. For example, you could add code in the setters which makes sure that the RGB values are only within a certain range. 

In fact, do exactly that as an exercise and make sure it compiles.

For this course I will not be recommending that you use getters and setters, as they can clutter up code with boilerplate and sometimes introduce new bugs. But when you really need to validate input, or transform values as they are retrieved for some reason, you may want them.

## Static Classes

If you qualify a `class` as `static`, there will be only one instance of that class, and you will not be able to instantiate it. Instead, you will access its functions and variables by using `ClassName::field`. 

## Conclusion
Defining classes and structs is a key part of programming. By composing data into "bundles of meaning" in the form of objects, you make it easier to reason about your program at a higher level, and allow yourself to make meaningful abstractions. You are essentially building up the meaning of your program.

You have been given the tools to make your own data types and objects. However, the *best way to design your objects* is a **huge and complex topic**. We will revisit this in a future chapter on object-oriented design.

