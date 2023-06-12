These notes will focus on common ways of creating complex classes and objects and how to reason about them. We will learn some language features we have not considered yet.

First, some light review. A *class* is an abstract *type* of thing and is not (usually) considered a thing itself. You create a new *instance* of that type via *instantiation*. Those instances are *members* of that class, and those instances are specific things. 

So far, we have seen that we can *compose* objects by including them in the class description for another type of object. For instance, if we have a class `Car` and a class `Color`, the class `Car` can declare one (or more) variables of type `Color` (for instance, for the exterior paint and the interior). The `Color` is part of the information necessary to describe a `Car`. In this sense, the `Car` is said to have a "has-a" relationship to the `Color`(s). Whether or not we can meaningfully change the `Color` *itself*, or merely assign a new `Color` to the `Car`, depends on whether `Car` holds a `Color` *reference (or pointer)* or a `Color` *value*. In theory, if multiple Cars held reference to the same `Color` object, we could modify that `Color` and suddenly all cars holding references to it would look different. The car also probably "has-a(n)" engine, a transmission, a (set of) wheels...

There is another type of relationships between object types: the "is-a" relation. We differentiate this from normal language because "is" can mean very different things. We might say "the car is yellow" -- but that doesn't mean the car *is the color called yellow*, or even *a* color called yellow: we can't drive around on a color. An "is-a" relation models a type of thing that actually *is* another thing. For instance, my 2006 Kia Rio5 might be an instance of class `Car`, or it might be an instance of a more *specific type* of `Car` which is somehow derived from the *abstract idea* of a `Car`. `Car` itself might be derived from the idea of a `Vehicle`, which itself might be just a more specific form of `MovingThing`, which might be a specific type of `Thing`. Members/instances of a class are generally said to have an "is-a" relationship to the class they belong to: my 2006 Kia Rio5 *is a `Car`* and it *is a `Vehicle`*. 

But this process of defining *more specific types of thing* sure is interesting...

## Inheritance

*Inheritance* is a concept originally brought on by object-oriented design. Sometimes a type of thing is so abstract that there are multiple possible types of that thing. This is where inheritance comes in. A *child* (or *derived*) class can *inherit* from one or more *parent* (or *base*) classes, and has, at least, all fields of all of its parents. It can also *override*, i.e. redefine/reimplement, those fields. Of course, a single base/parent class can have multiple derived/child classes.

```cpp
class Base {
	int a = 5;
	int b = 6;
	void printAPlusB() { std::cout << a+b << std::endl; }
};

class Derived : public Base {
	Derived() { a = 12; b = 25; }
};

class AnotherBase {
	std::string serialNumber = "ABCD-1234-EFGH-5678";
	void verify() { requestVerificationFromServer(serialNumber); }
}

class AnotherDerived : public Base, public AnotherBase {
	AnotherDerived() { a = 20; b = 40; serialNumber = "5N33D5-533D-4ND-F33D"; }
}

Base myBase;
Derived myDerived;
AnotherDerived another;
myBase.printAPlusB();
myDerived.printAPlusB();
another.printAPlusB();
another.verify();
```

For example, we might define a class `Vehicle` which has three children: `GroundVehicle`, `AirVehicle` and `WaterVehicle`. To be clear, these can also be equivalently called subclasses or derived classes.

An abstract base class is one in which at least one field of the class is left undefined (in C++ terms, is "pure virtual"). We cannot instantiate such a class, because there is a portion of its functionality left undefined. First, we need to derive a class which defines/implements that field. For example, if `Vehicle` has a pure virtual function `goToLocation(x, y, z)`, we don't actually know how the vehicle will get there (what if we have a `TeleportingVehicle`?). But we might write definitions of that function for its children -- a `GroundVehicle` will seek a path over land and follow it, making "vroom" noises, an `AirVehicle` will take the shortest as-the-bird-flies path and make "whirr" noises, and the `WaterVehicle` will find the closest point accessible by water and go there making "splush" noises.

Let's take a moment to check ourselves conceptually. Think about these as an exercise and we'll review them at the end of the document:

- Is the *type* `GroundVehicle` a member of class `Vehicle`?
- Is my Kia Rio5 a member of class `GroundVehicle`?
- Is my Kia Rio5 a member of class `Vehicle`?
- What type(s) is my Kia Rio5 an instance of?
- Is my Kia Rio5 a child of anything?
- Is the class of which my Kia Rio5 is a member a child of anything? An instance of anything?
- If I instantiated a new `GroundVehicle` would it be a subtype (or child) of `Vehicle`?

It is common, especially in other languages which support them explicitly, to call base classes where all fields are undefined *interfaces*. In a sense, the interface defines what operations a type of thing must be able to perform, but does not mandate how they must be implemented or how they will ultimately be called. If your class inherits (or *implements*) an interface, you are declaring that your class will work in any situation where that interface is needed. Hence, if `Vehicle` is an interface, we can inherit from it and implement its methods to make *any kind of Vehicle we want* as a new subtype, and instantiate that subtype freely. 

Which brings us to an idea: What if multiple types of thing implement the same interface? Doesn't that mean we can use them interchangeably? What if, anywhere we needed a `Vehicle`, we could pass in a `GroundVehicle`, `AirVehicle` or `WaterVehicle` depending on our preferences?

## Polymorphism

Polymorphism is an idea that is often overcomplicated, so let's keep it simple. *Polymorphism* is a property of code that allows it to operate on multiple types of data, potentially in different ways for each type. There are two kinds of polymorphism, static and dynamic. 

In *static polymorphism* we simply have code which can definably take different types (which are known at compile-time). In this sense, overloading a function to take both integers and floats is polymorphism. In C++, templates are also statically polymorphic: we are saying we can generate a type or function that will take any type we need it to, so long as our compiler doesn't say it's impossible (i.e. it will result in us calling undefined operations). This is all known at compile time, and is hence static.

*Dynamic polymorphism* is when we need to execute different code on different types of objects, and the type of those objects is not known until runtime. For example, I might read a configuration file, and create new objects of different types based on the contents. I then need to execute code from those objects. Another example might be a player placing their various minions on the battlefield. It would be helpful to represent a minion as an object and place its behaviors in the associated class. But we don't know until runtime what kind of minions the player will choose to summon. Using *dynamic polymorphism*, we can implement all kinds of different minions and handle them all using a common set of operations. You may have guessed it, but that common set of operations could be the `IMinion` interface. It is common convention to prefix interface classnames with `I` to indicate their meaning.

In C++, we need to mark class methods as `virtual` in order to override them in derived classes. You can also mark a method as "pure virtual" by writing something like:
``` cpp
class IMinion {
// Yeah, this syntax is dumb as hell. It just means there is no definition.
	virtual void AttackEnemy() = 0;
// This one *does* have a definition, but can still be overridden in child classes.
	virtual void PrintMinionName() { std::cout << "Generic Minion" << std::endl; }
};
```

Which means that the method does not have any implementation in the current class. If a class has any pure virtual function then it is considered "abstract" and cannot be instantiated. Derived classes don't necessarily need to override it, but if they don't, they too will not be instantiable. Only classes which no longer have any pure virtual functions can be instantiated. To override, you should explicitly mark your method as overriding:
``` cpp
class SkeletonMinion : public IMinion { 
	// This class is still abstract, as we haven't defined AttackEnemy().
	void PrintMinionName() override { std::cout << "Skeleton Minion" << std::endl; }
};

class SkeletonWarriorMinion : public SkeletonMinion {
	// This class can be instantiated!
	void AttackEnemy() { rattleBones(); swingSword(); }
	void PrintMinionName() override { std::cout << "Skeleton Warrior Minion" << std::endl; }
};

class SkeletonArcherMinion : public SkeletonMinion { 
	// This one too!
	void AttackEnemy() { rattleBones(); fireBow(); }
	void PrintMinionName() override { std::cout << "Skeleton Archer Minion" << std::endl; }
};

```
The override keyword is not *required* by all compilers, but some will complain if you don't use it.
You can also mark a method as `final`, which indicates that the method cannot be overridden in a derived class.

You might've noticed the "public" in the inheritance. This is essentially reflecting, just like class visibility, which parts of the program "see" the inheritance. For most cases, I recommend using public inheritance. When you want private inheritance you will generally know it, but in general it is only useful for cases where the object needs the inherited behavior only for its own internal use (this is rare).

## In Practice

Often we have some collection of objects, or some specific variable that we look to, which contains an object of our polymorphic type. 
But note a few things:
1. If we have a `std::vector<SkeletonMinion>` or a `std::vector<GoblinMinion>`, our collection stores only one kind of minion... polymorphism is useless in this case.
2. If we store a `std::vector<IMinion>`, we are storing *values of type `IMinion`* -- also pointless, because `IMinion` has no operations actually defined! Even if you do have those methods defined, we can only ever call the generic `IMinion` version, not the specialized versions we want.

In fact, if you try to do something like #2 for an `IMinion` with functions which are not pure virtual, you will see that no matter how much you try to pass in a derived class, only the generic version gets called. Try this as an exercise.

So what should we do?

You may have guessed that we need to use a pointer or reference type instead of a value type, something like `std::vector<IMinion*>`. This may run slightly counter to intuition, but a pointer of any more specific type can be used in the place of any less specific type. So in practice we always use *pointers* to an interface type. This isn't a hack or violation -- by definition, the derived class must expose the same functionality (or more) as the base type, so there's no chance that we do something undefined. 

We then use the dereferencing access operator `->` to access the methods of the *actual object being pointed to*, not just those of the pointer's type. This uses some magic under the hood: any type with virtual methods secretly contains a table of which functions to call when used with `->`. This table lookup means that virtual functions actually incur a performance overhead: every time they are called, this table needs to be referenced to determine the *actual* type of the object. For most applications this overhead is completely negligible, and I encourage you not to worry about it for the purposes of this course. Only very high-performance software needs to worry about this.

If you use a `std::vector<IMinion*>` and push pointers to `SkeletonWarriorMinion` and `SkeletonArcherMinion` into it, you will find that you can loop over the vector and get the desired output. 

```cpp
std::vector<IMinion*> minions;
minions.push_back(new SkeletonArcherMinion());
minions.push_back(new SkeletonArcherMinion());
minions.push_back(new SkeletonWarriorMinion());

for (int i = 0; i < minions.size(); i++) {
	minions[i]->PrintMinionName();
	minions[i]->AttackEnemy();
}

// What do you expect for the output?
```

Likewise, let's say you have some variable which represents how you want to load saves. You might have a single interface, `ISaveGameLoader`, which is inherited/implemented by `ILocalFileSaveGameLoader` and `ICloudSaveGameLoader`. The variable which can hold either of these objects could be of type `ISaveGameLoader*`, and conditional logic determines which object you create (with `new` perhaps?) and place into the variable. This is nice, because it lets you keep the external code clean, and it means you don't need to have some giant if-statement tree of however many different ways there are to save your game. It means that code for saving to the cloud can be kept entirely separate from code to save your game to a local file.

If you design your objects to hold pointers to these sorts of interfaces, you can make highly flexible and customizable behaviors.



## Reviewing Exercise Questions
- Q: Is the *type* `GroundVehicle` a member of class `Vehicle`?
  A: **No**. `GroundVehicle` is a *child* of `Vehicle`, but is not an *instance* of Vehicle -- `GroundVehicle` is still a (sub)type of `Vehicle`, not a thing which belongs to the type.
- Q: Is my Kia Rio5 a member of class `GroundVehicle`?
  A: **Yes**. I might be a member of some subclass, like `WheeledGroundVehicle` , but my car is still ultimately "usable" as an instance of a `GroundVehicle`. 
- Q: Is my Kia Rio5 a member of class `Vehicle`?
  A: **Yes.** Again, I may be a member of a more specific subclass, but that necessarily includes that I am a member of the less specific class.
- Q: What type(s) is my Kia Rio5 an instance of?
  A: My car can be considered to be an *instance* of `GroundVehicle`, `Vehicle` and any other less specific classes they inherit from.
- Q: Is my Kia Rio5 a child of anything? Derived from anything? A subclass of anything?
  A: **No, no and no.** My car is an *instance* of at least one class, but my *car is not a class or type of thing* -- it is not possible to instantiate something from my car. Therefore, my car is not a subclass, a derived class or a child class.
- Q: Is the class of which my Kia Rio5 is a member a child of anything? An instance of anything?
  A: **Yes**, `GroundVehicle` (or whatever other class my car is most directly a member of) is a child of `Vehicle`. However, this second part is kind of a trick question: Considering just normal classes, the *class* `GroundVehicle` is not an instance of anything. But if you think about it, *a type*, itself, is a *type of thing*. You could say that every class is an instance of some "metaclass" of which every class is an instance. This might sound like philosophical mumbo-jumbo but it turns out this is actually a useful concept *sometimes*. 
- Q: If I instantiated a new `GroundVehicle` would it be a subtype (or child) of `Vehicle`?
  A: **No**, the instance of `GroundVehicle` would be an object, not a class, so it won't be a subtype or child (at least, assuming that GroundVehicle isn't a metaclass...)
   
While my points about metaclasses are real, don't worry about them too much. For the rest of this course we won't really touch on them (and most people look at you funny if you mention them anyway). Based on student interest we can investigate applications of those. 

## About These Relationships

We have discussed "is-a" and "has-a" type relationships. Of course, there are other kinds of semantic relationships we can define between objects. However, these two form a special class which describe the intrinsic meaning and structure of a class. Other relationships can be defined through what methods they expose to one another. For example, a class `Color` is responsible only for describing what it is, not for how others use it or relate to it. The latter category is perhaps infinitely large (Cars might have a Color of paint, a Person might have a favorite Color...). This notion of describing only what is necessary to represent a class is something we will touch on again.

You could even define a relationship as a type of object! A graph data structure is basically this but for more than two things. The sky is really the limit here.

## A Note on Languages

Languages are categorized as having strong typing, weak typing, static typing, dynamic typing, or duck typing.
These meanings are pretty poorly defined and have some overlap, but generally:

In statically typed languages, *variables* have an associated type. In dynamically typed languages, *values* those variables take on have associated types. In practice, this means that a variable with a given name may take on different types at different points during the program.

Sometimes weak typing is taken to mean that the language can implicitly convert between types freely. This also potentially opens the doors to type-related errors at runtime (sometimes called "type punning", since it's a sort of double-entendre of data). By contrast, strong typing is sometimes taken to mean that a type-related error cannot happen at runtime because of strict type checking done at compile time.

*Duck typing* is a special kind of typing following the motto: "if it looks like a duck and quacks like a duck, it's a duck". In this typing scheme, a variable or value has a specific type if it meets the interface of that type. A significant amount of Python code works this way. For example, any object can be printed like a string if it has a specific function defined which converts it to a string, and any object can be treated like an iterable (i.e. something which can be looped over, like a list) if it defines a certain set of functions. This opens up some interesting options but can also get messy.

You can use these sorts of mechanisms, implicit or explicit, to generate desirable behaviors.

## Operator Overloading
C++ and Python both allow you to actually define new operators for your own custom data types. Usually the operator \[\] is used for indexing into a collection, but there's no reason it has to mean that for your type beyond convention. You can even make your object "callable" by defining the operator ():

``` cpp
class MyAction {
	void operator() { std::cout << "Running MyAction" << std::endl }
};

MyAction action;
action();
```

Or you could return a reference to a member variable, to allow something else to modify a specific part of your object:
``` cpp
struct SeeminglyInfiniteStorage {
	// Really just 4 ints worth of storage, but they'll never know...
	int a = 0;
	int b = 0;
	int c = 0;
	int dummy = 0;

	int& operator[] (int index) {
	switch (index) {
	case 0:
		return a;
	case 1:
		return b;
	case 2:
		return c;
	default:
		return dummy;
	  } 
	}
};

SeeminglyInfiniteStorage obj;
obj[1] = 42;
obj[55] = 99;
obj[77] = 156;
int& someRef = obj[0];
std::cout << "A: " << obj.a << std::endl;
std::cout << "B: " << obj.b << std::endl;
std::cout << "C: " << obj.c << std::endl;
std::cout << "dummy: " << obj.dummy << std::endl;
someRef = 255;
std::cout << "A after: " << obj.a << std::endl;
```

You can get very creative with this stuff, and with advanced features you can get even more powerful. Hopefully you see how, even in a strict language like C++, you can basically create your own little dialect depending on your preferences. While I can't say that these sorts of features are used all the time, sometimes they just make sense for what you're modeling in code. For example, libraries which are used for matrix math override operators extensively to provide more intuitive ways of working with the data.  

## Conclusion

It should be clear by now that while these features of the language are well-fit for specific types of object metaphors, the actual meaning can be quite flexible, and names are just names. 

In the corresponding assignments, we will do some more case-studies and get you thinking about how complex systems might be modeled using these mechanisms. Next up in the notes, we will talk more about specific paradigms and principles.
