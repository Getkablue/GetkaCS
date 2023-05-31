Often, even when we write an algorithm in pseudocode, we find that we need a "place to store" some information for use later. That is, we need *memory* of what has been done in the program so far, or at least a tiny part of it. For example, if I am multiplying two numbers via repeated addition, I need to keep track of, at least:
- How many additions I have done so far.
- What the result of the previous addition was.

These pieces of information are a part of the *state* of the program, and they change as the program progresses. We often think of "marking a place for" these pieces of information, and putting new values into that "place", and taking values from that "place", as part of our algorithm.

That "place" represents a *variable* piece of data in memory. For short, we call them *variables*. Each variable *has a value* at any given moment, but that value does not define the variable (at least, not for long). The variable is defined by its meaning, and usually given a name for easy reference. For instance, I might call the first variable in the multiplication example "counter" and the second variable "last_result". These variables are *mutable*, as their value can change.

There is another type of data: a *constant*. Some problems and algorithms might use a certain number, but that number never changes. For example, calculating the circumference of a circle from its diameter requires the number pi, which we know to have a value of roughly 3.14 . We never change pi, it's just sort of a "fact", a *constant value* that we find useful. In this way, constants are "baked into" our program -- they can't change, no matter what happens in the program. A constant can be said to be an *immutable value*.

Many hilarious (and tragic) bugs have arisen because someone, first, forgot to mark a named value as constant/immutable, and second, changed it somewhere in their program. There was a time where, in Python, you could write:
``` python
False = True
```
And the interpreter wouldn't even complain about it. While it might not be clear exactly what that would end up doing, I hope it is crystal clear that "something which is False *cannot be True*" is a core assumption of our logic. Our code, and logic in general, is only as sound as the assumptions made to justify it. (Rest assured, the resulting bugs were absolutely insane.)





