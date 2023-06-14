Comments are text used to annotate source code, to explain the meaning of certain segments or to communicate something to others looking at it. Code comments are "skipped over" during compilation or interpretation.
Most languages provide some kind of way to write comments inside the source code itself as free text. This is very helpful for remembering what snippets of code do if you are returning to a project after some time, or for clarifying code that is confusing.

In this course I will often also use comments to stand in for other segments of code which may be required for the current example, but for which the implementation is not important.

Below are some examples to help you recognize comments. 

```C
// This is a comment in C and C++

/*
 This is 
 a multiline
 comment in C and C++.
 All lines inside the asterisk-slashes are comments.
*/


printf("Hello World!\n"); // You can comment at the end of a line, too

```

``` python
# This is a single-line comment in Python

print("Hello World!") # You can also add comments to the end of lines

'''
A block of text enclosed by triplets of single-quotes (???) is sometimes called a "docstring", and is an easy way to do multiline comments in Python
'''
```