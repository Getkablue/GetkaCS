This is going to be another short section. Not that this isn't useful -- it is. But the FP space is still evolving, and it is better for some applications than others. FP is generally used sparingly in mainstream languages but extensively in dedicated languages like F# and Haskell. However, you may find the ideas here useful!

In the previous section on Event-driven programming, we discussed the idea of passing functions into other functions. This idea extends pretty naturally into passing functions to objects, as well.

In the Functional Programming (FP) paradigm, functions are treated as "first-class members of the language", i.e. they can be passed around just as easily as data. We are encouraged to think about our programs in terms of higher-level functions which take lower-level functions, and we are encouraged to write functions which do not rely on state. Even constants in an FP paradigm can be considered as (very simple) functions which just return their value.

The concept of only operating on values which must be newly created whenever they are changed is called *immutability.* An object is *immutable* if it cannot be changed without creating a new copy (i.e. if it is a strict value type). By contrast, *mutable* objects can change while remaining the same thing. FP encourages operating whenever possible only on immutable objects. This way, there is no chance for functions to accidentally change their inputs or other objects, which would cause *side effects*. A *pure function* in FP is basically a mathematical function in that it has no side effects. Furthermore, if a function does not rely on some external state, then its logic can be totally verified -- there is no external state which could cause a function to change behavior. In this sense, we can verify the *correctness* of our programs much more easily! 

On the other hand, side effects are usually pretty useful to us as humans... I mean, we care about side effects. A function with side effects in FP is "impure". But sometimes they're needed, say to actually output our results. These are the "boundaries" of our system.

One more thing about the more "ivory tower" form of FP: it uses what is called an *algebraic type system* to help make that whole correctness verification thing even easier. 

Let's say I am writing a website that uses FP in the underlying code. I need to take some registration information and do some checking, create a user, and log that in a database (or whatever).  The algebraic type system, similar to inheritance, lets me easily declare when a type may or may not be substituted for another. For instance, a user might put in text for their email address. Until I know that it is actually an `EmailAddress`, it is just `Text`. So my function which parses that field takes `Text` and returns an `EmailAddress` (or otherwise returns a `Failure` of some kind). Then, I can easily make a new type `VerifiedEmailAddress` which, by the type system, can *only* be produced by a function which produces *specifically* a `VerifiedEmailAddress`. My later stages of processing need only accept `VerifiedEmailAddress`es, and in the process of writing this program I have basically made my compiler verify the security of my signup process. 



## Other Useful Tidbits

In some cases in FP, we are encouraged to write small *anonymous functions*, i.e. functions without names, and pass them directly into higher-level functions. These anonymous functions are also sometimes called *lambdas*.

``` python
myoperation = lambda x: x + 5
myoperation(4) # returns 9

myotheroperation = lambda x, y: x / y
myotheroperation(20, 5) # returns 4
```

You can do this in modern C++ too, but I leave that to you as an exercise. We might talk about it later depending on student interest.

A few common tools exist in the FP paradigm which you may find useful in general.

*Map*, *reduce* and *filter* are common higher-level functions and can be used to express many kinds of solutions. I use Python syntax here just to help you understand the common usage -- items enclosed in square brackets compose a list, like `[1, 2, 3]`.

`map(data, F)` means "apply the function F to every element of the data and return the results as a new collection". This is essentially a way to bulk-transform data in a very expressive way without messing with loops. For example, `map([1, 2, 3], square)` might return a new list with value `[1, 4, 9]`. The arity (number of parameters) of F should be 1.

`reduce(data, F)` means "proceeding from the beginning of the data, take the first elements and pass them as arguments to F, then take the result and pass that result and the next element as arguments to F ... and so on".
For example, `reduce([1, 2, 3], Add)` might sum the entire array, returning `6`.
Note that F in this case needs an arity of at least 2.

`filter(data, F)` means "return a new array of only the elements of data for which the result of F is True". For instance, `filter([1, 2, 3, 4, 5, 6], isEven)` might give only even numbers, returning `[2, 4, 6]`. Note that the arity of F should be 1.

In C++, these are equivalent to `std::transform`, `std::copy_if` and `std::accumulate`, respectively. These C++ versions often accept iterators instead of explicit data collections.

