This segment is completely optional. You can make a fully working, complex program without ever touching generics. But generics can be very useful for making especially flexible libraries, and `auto` is just plain nice to have.

We have briefly described a concept known as "generics", alternatively *generic programming*, in other parts of this course. For example, we described data structures which could contain any single type as their contents, like `std::vector<T>` which stores objects of type `T`.

In C++, `auto` is a keyword that allows the compiler to *infer* the type of the thing in question (and allows it to complain at you if it's too ambiguous to resolve). For example:
``` cpp
int returnSomeResult() { return 5; }

auto result = returnSomeResult();
```

To be clear, `auto` does not declare a variable that can change type. Instead, you can think of it that the compiler replaces `auto` with the correct type at compile time. This is extremely useful when either the type is too complicated for you to type out, or where you *don't actually know the type yourself* (these tend to overlap).

# Templates
Templates allow you to declare pieces of code (such as functions and classes) which will be expanded by the compiler to accommodate (potentially) any type. You do this by specifying the *template parameters*:
``` cpp
template <class T>
class AddableContainer {
	T value;
	
	T& getReference() {
		return value;
	};
	AddableContainer(T val) { value = val; }
	operator=(T val) { value = val; }
	operator+ (AddableContainer<T> rhs) { return value + rhs.value; }
};
```
In the above example, "T" is a template parameter which accepts the name of a class. Note that we use T in the rest of the class definition. What this means is that whatever type we decide to actually use for T, the compiler will attempt to generate the corresponding class, the *template instantiation* or perhaps *template specialization*. The code above also includes some conveniences to make our class easier to use. For example:
```cpp
AddableContainer<int> myContainer = AddableContainer<int>(5);
myContainer = 4;
# The compiler can do some limited type-inference for us
# (Note the lack of template specifier)
auto anotherContainer = AddableContainer(7.77);
```

But note something: even this simple class definition requires that two values of type T can be added with the + operator. If that operation isn't defined...
``` cpp
class Color { int r, g, b; };
AddableContainer<Color> other; // ERROR! Compiler cannot build this class!
```

Interesting -- the compiler *only* tries to instantiate the template for a given type *if we actually try to use it*. This can sometimes lead to confusing compilation errors. Anyway, it should tell us something that indicates, in so many words, that operator+ isn't defined for class Color. If we add that to Color, our compilation will go through just fine.

We can template across multiple types, even types which themselves are templated:
``` cpp
template <class T1, class T2>
class ContainsAnyTwoThings {
	T1 firstValue;
	T2 secondValue;
};

auto mine = ContainsTwoThings<std::string, std::vector<int>>;
```
And we can make template parameters which are not classes, like `unsigned ints`, and use them mostly like you might expect:
``` cpp
template <unsigned int N>
class MyOtherStrangeThing {
	int someFunction() {
		auto result = 0;
		for (int i = 0; i < N; i++) {
			result += 5;
		}
		return result;
	}
};
```
(We might also... I don't know, create an array of N elements of type T... you get the idea.)

Oh yeah, and you can add default template parameters that are used if no such parameter is provided.

Template parameters are known at compile time. This means that you can use template parameters to do some interesting things which require constants, but also means that you can't pass runtime variables in as template parameters. In current C++, the sorts of things you can use as template parameters are limited because only some types are considered to even *be* "knowable at compile time", but the more recent C++ standards plan to make this much more flexible, so that you could have, say, compile-time `Color`s or compile-time `Car`s. As you can see, this area is still actively evolving.

Of course, we can template a single function (whether inside or outside a class), too:
``` cpp
template <class T>
T* Create(int a, int b, int c) {
	T* ptr = new T();
	ptr->doSomethingWith(a);
	ptr->doAnotherThingWith(b);
	ptr->doYetAnotherThingUsing(c);
	return ptr;
} 
```

Maybe you can see now how auto might be *necessary*, not just a *convenience*. If we `return T1 + T2`, but the expression `T1 + T2` returns some third type, we may not have any idea what the hell that is. Hence, you could use `auto` in the return type, and C++ will fill it in for you.

Separating templated class definitions from their declarations is a little more complicated. If you have some template class, with some non-template functions inside it, you will need to template the definitions too.

Header:
``` cpp
#pragma once
template <class T>
class Color {
	T r, g, b;
	T getBrightness();
};
```

Implementation:
``` cpp
template <class T>
T Color::getBrightness() { return  r + g + b; }
```

It gets a little more complicated when there are templated functions inside template classes. As an exercise, try to figure this out!

Finally, we can also include *explicit template specializations* which override the implementation for a particular template parameter(s).

Of course, we want to be able to reason more strongly about *what kinds* of things our template types and functions might be useful for. Which brings us to... 

# Metaprogramming
Okay, *now* you're thinking like a wizard. Metaprogramming is writing code that determines how your program is programmed. At its most powerful, we might think of metaprogramming as a set of techniques that allow us to create very expressive programs while writing very little code.

There are a couple ways of doing this sort of thing. Earlier in lecture 2 I brought up *metaclasses*, for example, which is one such way. Preprocessor macros are a (very shitty) way of doing this, too.

Even the newest C++ standards don't have this... yet. The C++ committee already struggles to decide which features are well-defined enough to make it into the standard, and *many* features are proposed for every standard. They do NOT want to introduce a *specification-level* bug into a programming language! Any fixes to that bug would, by definition, be violations of the standard. It's even harder when there are multiple languages attempting to be C++ killers. I am confident that eventually, "real" C++ will pick up these features. The competitors have a lot of hurdles to get over before C++ can be considered dead. 

But that does *not* mean that you can't use these features now. *Circle* and *CppFront* are two projects which attempt to provide sample implementations of these features while retaining basic compatibility with the C++ language overall. Actually, they look like a good future for the C++ language in general, although I suspect you aren't here for me to ramble about this. Anyway, if you are here, you are probably at least vaguely interested in the advanced stuff, so maybe check those out and use them to play around with highly experimental features.

In Python, you can also experiment metaclasses right now using a built-in library... But Python also has no compiler. So the idea of changing code in response to some other factor *is* something you can do, but it's not as useful or extensive in Python because nothing can be checked before runtime.

A *metaclass* is a class whose instances are *other classes*. So when you use the keyword `class`, you are saying that you are creating a new type which *itself is an instance of a type-of-types called "class"*. All the rules associated with making a class, then, can be thought of as "parameters to the constructor for objects of type *class*" -- and the actual declarations/definitions therein are the values. Freaky! 

We can imagine that, besides this default metaclass, we could define our own metaclass... and instantiate that to define new types instead... But at this point, it's a little hard for us to imagine what that might even look like, let alone what it would accomplish. In C++, one such proposal uses so-called *static reflection*. 

*Reflection* in general is the capability for programs to use information about themselves. This takes the form of either *static* or *dynamic* reflection. As usual for stuff we mention here, *static* reflection occurs at compile time, *dynamic* reflection occurs at runtime. For instance, you might include information in a type which is usable at runtime to query its type. That might be useful, for example, if you have a collection of base-class pointers but at some point do actually need to know what type the current object "actually is". Or you could use it to see at runtime if a type is a child of another. But this is actually a weaker form of static reflection. 

With static reflection, our program can change as it compiles to respond to changes in other parts of our program. For instance, we might write code which acts differently on types which have a variable named "name" than on types which do not have such a variable. In fact, with static reflection, we can *inject new code in response to some condition*. For instance, we might use static reflection to inject a new const field called "`_className`" into every class (which can then be used for runtime reflection, too). Hence, the existence of dynamic reflection does not imply static reflection is possible, but the existence of static reflection also instantly means dynamic reflection is possible to implement yourself. Of course, this has other applications too.

Coming back to metaclasses, imagine if we had a type of class which required you, the programmer, to define certain fields. Or imagine if we had a type of class that would automatically be filled in with certain code if you named it a certain way. The actual possibilities here are pretty much endless. I, for one, would love to just write "Logged" in my class name and have the compiler automatically generate appropriate logging code for all my functions and variables.

We could also imagine code that automatically adapts to the interface provided by some lower-level code objects like libraries, and all of this happening at compile time with *no* runtime performance impact. Ah, perfect!

It's not that easy in practice, but metaprogramming opens the doors for these things to be done all in-language without needing any external tools or pre-parsers or metacompilers or whatever.

One example of this type of feature that *has* made it to the level of being included in the standard:

## C++ "Concepts"

The newer versions of the C++ standards include a concept known as, well, "Concepts". A "Concept" is basically a set of constraints which a type must fulfill in order to be considered an instance of that Concept.

The most basic form is something we have sort of already seen: a template in C++ will fail to compile if a class it is requested for doesn't support an operation used inside it. If our template uses `a + b` but the types of A and B don't support addition in that way, we will fail. A constraint could just be requiring that an expression like that compiles for those types. Or it could be reliant on some other information about the type... like whether it is convertible to bool, or whether it has "Bhengis" in the class name... again, we could be as dumb or intelligent with these as we like. 

Anyway, if a concept is comprised of these constraints, then *any type which meets these constraints is usable as that Concept*. If I declare that a function takes a `Number`, where `Number` is a concept with constraints equating to "must support addition with itself" and "must support subtraction with itself", then I can pass in ints, floats, or any custom type which fulfills those, into the parameter which accepts a `Number`.

We can compose increasingly more complex Concepts and allow our code to work at this very high level, without being concerned about specifics of the underlying types. Isn't this what we *really* set out to do at the beginning of our programming journey? Manipulating concepts, not specific technical implementations?






