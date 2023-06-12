The source code for your program is an interesting thing. It isn't, strictly speaking, the finished deliverable product (that would be the executable or system produced by the source code). But in some ways, source code is actually *more* valuable than the finished product -- after all, without source code, you can't build the deliverable again, nor can you make changes (at least, not without advanced decompilation and reverse-engineering techniques...)

In a complex project, good, clear source code is as good as gold. Especially as people change requirements, goals shift, and new use cases need to be considered, the source code inevitably mutates and grows. Often this occurs in ways that leave it looking like a plate of spaghetti, entangled in a large incomprehensible web, or grown into a single tumorous mass, where a ton of functionality is baked into one bloated component (I guess these are the "meatballs" in spaghetti code...).

Programming paradigms and principles attempt to make code more comprehensible, not just by enabling a specific metaphor for your program's operation, but also by putting code into a sort of common format. Not everyone may be familiar with that format, at least initially, but keeping code consistent throughout the project makes it easier to understand on the whole, especially if your team can stick to patterns.

This sometimes leads to hard "rules" which are mandated during code review. For example, some groups will demand that programmers *never* use raw pointers, and *must* use smart pointers. Others disable all features from newer versions of C++ and demand you stick to a previous standard (often this is for contractual reasons, believe it or not, but sometimes it's just to make sure the project never needs to migrate). These rules are all up to taste, at the end of the day. Sometimes you just need ugly code to get the job done.

Changing the architecture and/or implementation of code to fit some style, to make future design easier, or even to fit performance needs, is often called *refactoring*. Refactoring can be a celebrated thing, or it can introduce new bugs. Think carefully about how you spend this effort -- it is easy to get obsessed with having a "perfect" design when really you should be spending time creating your solution.

Paradigms are basically the highest level of design style, where they are just about the overarching structure and metaphors employed. Principles and patterns describe intermediate-level design techniques within those paradigms, and then at the lowest level "code style" usually refers to smaller decisions like variable/function names, where to place curly braces, how comments should be placed in code, etcetera. 

Later in this lecture series we will review some specific paradigms which will be of particularly high value to you in greater depth. A basic understanding of those will help you understand many projects that currently exist.

### The Most Common Principle

This is up for debate, but I would argue that the most common and useful principle in daily programming, especially in team projects, is **DRY**: Don't Repeat Yourself.  

This principle states that anywhere code is manually copied or written multiple times, some form of design mistake has been made. A reasonable sense of this rule uses the Rule of Three: If you repeat the same block of code 3 times or more, refactor it. 

Obviously, patterns are patterns for a reason. You don't need to make *everything* in your code fully generic (in fact, doing this is often a way to be *more* confusing). But if you find yourself doing common tasks repeatedly, and they have the same *meaning* to your program, try to isolate them into a separate, reusable function. If you do, when bugs occur in that logic, you'll only have to fix them *once* instead of 3+ times as they become apparent.

You will sometimes run into situations where the language and compiler *require* you to repeat yourself. If you are interested, *templates* in C++ offer you a way to write code suitable for many types at once, without needing to repeat yourself. This is an advanced topic and has been placed in an extra note in this series.

Code that is required to be repeated, especially large, clunky blocks of code that aren't very expressive, is often called *boilerplate*. These are sometimes needed when working with an existing system which requires things in a specific format, or when using complex language features. But since boilerplate often has nothing actually to do with your own program logic, too much of it can lead to very cluttered looking code that is hard to read.

*Readability* just describes how easy it is to infer the meaning of your code at a glance. Some languages are considered innately more readable, while others (especially those with strict syntax rules) are less so. 

For instance, if you can tell me what either of the following mean at a glance, you get some massive brownie points:
``` c
void (*fun_ptr)(int, int*) = &fun;  
```
``` cpp
struct T {
	template<typename Integer,
             std::enable_if_t<std::is_integral<Integer>::value, bool> = true>
    T(Integer) : type(int_t) {}
};
```

(Actually, these use language features we haven't really looked at, but even for me these hurt to look at).

Keeping your code clear is so important that we often prefer clear code to fast code. You should prioritize keeping your code simple, clear and readable, and only compromise that for performance when it is really needed. When it is, you mark it as such and explain in comments *why* this code is the way it is.

Some advanced techniques, such as macros and template metaprogramming, allow you to basically define new custom language features of your own. Almost by definition, nobody outside of your project will know about these. You sort of end up writing a dialect of your programming language. Especially popular dialects often ascend to quasi-language status. Some examples of this in the C++ world are the Qt toolkit/framework, used for a ridiculous amount of GUI applications, and the Unreal Engine.

# Paradigms

Most likely, any exposure you have had to programming up until now has been of a particular style -- instructions are written line by line, in order, and that is exactly how the instructions are executed. Your program consists of an entry point, and proceeds from there, following instructions you have written, perhaps jumping to functions. This is known as an *imperative* (also sometimes *procedural*) programming paradigm / style.

At this point you may be wondering:  "what other way there could possibly be?"

And you'd be reasonable in doing so, especially given what we have learned about how a processor executes instructions.

There are other programming paradigms, including object-oriented programming (OOP), functional programming, declarative programming, data-driven programming, aspect-oriented programming, and more. Some of them overlap, and most software incorporates elements of multiple paradigms. Some paradigms don't exist until they are built on top of other paradigms ("declarative" is often a fancy way of saying "I can write this in very few lines" -- of course, the meaning of those lines has to be specified first...). 

Some languages explicitly endorse one paradigm as part of their design. For example, Haskell is a strictly functional language. Other languages, such as C++ and Python, aim to be multi-paradigm (perhaps even all-paradigm). We know that these languages are Turing complete, so of course, any program *can* be written in any paradigm (* well, with a few exceptions...), but some types of problems map better to certain paradigms. And sometimes you just have a particular style you like. Much of the hubbub about programming languages is really more about how *nice* it is to write code in, rather than about any actual difference in capability. 

And honestly, that actually does mean a great deal. Software projects are often extremely complex, and the people who work on them generally get compensated quite well -- developer time is not cheap. Anything that makes source code easier to work on or easier to understand is extremely valuable. On the other hand, performance is also often critical. In high-speed trading software, a change in milliseconds of performance could cost a firm millions of dollars. In medical or aerospace software, performance and reliability are responsible for lives. Being sure that a bug won't arise during a life-saving operation or during a flight is an ethical requirement. Languages historically try their hardest to balance these concerns. Those which don't become dust in the wind.

## Common Paradigms

*Object-oriented* programming is the paradigm we will most focus on in this course, to the point of having multiple segments on it. Object-oriented programming, in short, focuses on *bundling code and data*. In OOP, the principle is to reason about programs as a number of objects interacting with one another. Thinking about parts of your program in this way often makes it easier to reason about, because you have an object metaphor for different pieces of functionality. It is also useful for things like simulations and game engines, where these constructs can directly represent "things" in the game world. It's also used very often in GUI software. A reasonable understanding of OOP will take you very far.

*Functional* programming (which honestly is kind of a terrible name) treats functions as first-class members of the language. That is, functions themselves can be passed around like data and used by other functions. Functional programming advises against the use of state, and prefers for functions to produce output without changing their input in any way ("immutability"). Sometimes this is seen as some 4D chess gigabrain stuff, and there is some truth to that ("monads are just monoids in the category of endofunctors", c'mon guys...) but it's really not as bad as it sounds, and avoiding reliance on state can actually avoid some nasty bugs. 

*Event-driven programming* is a paradigm where we are centered on the idea of events and how we react to them. Naturally, this is very useful for systems which need to react to user input such as GUI applications. In this paradigm, we often write *handlers* which handle events as they arise, and write *callbacks*, which are functions that will be executed in response to other events. We will get some practice with this in the lab series.

*Data-driven* programming is a paradigm in which we tell the computer what kinds of data we want to match (i.e. to find and work on) and what kind of processing needs to be applied to it to produce a result. Often this is used in systems relying on databases, where the bulk of our work is in retrieving data which already exists and doing some combination of filtering and transformation/cleanup. 

There are other paradigms to be aware of, but the above four will be most useful for us in our exploration. Other paradigms tend to be more experimental.



