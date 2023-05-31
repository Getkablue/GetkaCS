In the previous notes, we have taken a simple program and broken it down line by line. Now, we will discuss common basic structures used to define programs of increasing complexity.

## Control Flow
*Control flow* is a 1st-level Arcane spell which... nevermind. *Control flow* is just an expression reflecting how a computer progresses through steps of your program. Like fluids, electricity or ants, it follows a path through the set of instructions. Even if your high-level program description doesn't include the low-level steps explicitly, it is still possible to describe the program as "executing a certain step" of your program at any given time. (In fact, we can even make a mapping between the steps in the actual executable back to our source code -- this is known as "debug info".)

Try this now: Write down (as precisely as possible) a set of instructions for making a peanut butter and jelly sandwich. Then, follow the instructions (by pantomime if you must). While you are doing so, consider "where you are" in the program. This is the essence of control flow. Without a strong understanding of the order in which our code will be executed, we are very unlikely to make a successful program. The easiest programs to understand are linear sets of steps. However, this doesn't reflect the complexity of real problems. Very few real problems can be solved without any sort of conditional statement, jumping to a common point, or deferring to some other system for a result.

### Conditionals: If-else


