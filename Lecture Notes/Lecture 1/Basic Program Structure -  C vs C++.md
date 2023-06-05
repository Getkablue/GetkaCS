I will include here some brief notes to help you identify C versus C++ and to reason about the differences.

As implied by the names, C came first. C++ emerged later as an attempt to improve upon C. C++ can sometimes be thought of as a set of extensions to C, or a "superset" of C.

There are elitists (read: dumbasses) on both sides of the C/C++ divide who will argue endlessly about the virtues of either language over the other. Do not be swayed by that horseshit. You use the right tool for the job. If C code does the job efficiently and you understand how it works, use it. If C++ offers the better tools and you understand them, use it.

In general, almost all C code will compile when provided to a C++ compiler. The reverse is not true. However, the binaries produced by C++ compiler will be different than those produced by a strict C compiler. There may also be differences in warnings produced. Neither of these are all that important for this course. In general, C knowledge will directly translate to C++ knowledge, although C++ may have better ways of doing things in some cases.

Some general clues:

1. Any time you see the "`std::`" prefix, you are probably dealing with C++.
2. If you see generics, indicated by a typename with another typename in angle brackets, like `vector<int>`, you are dealing with C++.
3. If you see a `class` declaration you are dealing with C++. Structs exist in both languages.
4. If you see `auto` , you're in C++.

