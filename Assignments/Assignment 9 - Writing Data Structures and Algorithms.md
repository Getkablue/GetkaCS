## Part 1: Conceptual Questions

1. Describe the characteristics and use cases of the following data structures: a) Array b) Linked List c) Stack d) Queue e) Binary Tree
    
2. Explain the difference between a linear data structure and a non-linear data structure. Provide examples of each.
    
3. What is the time complexity of the following operations for an array and a linked list? a) Accessing an element by index b) Inserting an element at the beginning c) Removing an element from the middle
    

## Part 2: Code-Writing Questions

Question 1: Stack Implementation

Implement a stack data structure in C++. The stack should support the following operations:

- `push(item)`: Adds an item to the top of the stack.
- `pop()`: Removes the top item from the stack and returns it.
- `top()`: Returns the top item from the stack without removing it.
- `isEmpty()`: Returns true if the stack is empty, false otherwise.

Question 2: Binary Search Tree (BST)

Implement a binary search tree (BST) in C++. The BST should support the following operations:

- `insert(key)`: Inserts a new node with the given key into the BST.
- `search(key)`: Returns true if a node with the given key exists in the BST, false otherwise.
- `remove(key)`: Removes the node with the given key from the BST.

Question 3: Algorithmic Problem

Write a function in C++ that takes an array of integers and returns the second-largest number in the array. If there is no second-largest number, return -1.

Ensure your solution has a time complexity of O(n), where n is the size of the array.

Question 4: Sparse Matrix

A sparse matrix is a matrix where the majority of its elements are zero. One way to represent a sparse matrix efficiently is by using the Compressed Sparse Row (CSR) format. In this format, three arrays are used: `values`, `columnIndices`, and `rowPointers`.

- The `values` array stores the non-zero values of the matrix.
- The `columnIndices` array stores the column index of each non-zero element.
- The `rowPointers` array stores the indices where each row starts in the `values` and `columnIndices` arrays.

Your task is to implement a class called `SparseMatrix` in C++ that represents a sparse matrix using the CSR format. The class should support the following operations:

- `SparseMatrix(int rows, int columns)`: Constructor that initializes an empty sparse matrix with the given number of rows and columns.
- `void setValue(int row, int column, int value)`: Sets the value at the specified row and column in the sparse matrix.
- `int getValue(int row, int column)`: Returns the value at the specified row and column in the sparse matrix. If the element is zero or out of bounds, return 0.
- `void print()`: Prints the sparse matrix in a readable format.

Note: You can assume that the input values for the sparse matrix are such that it remains sparse even after performing various operations.

Question 6: 

You are given a list of student records, where each record contains the student's name and their corresponding test scores. Your task is to design a solution to **efficiently** find the top K students with the highest average scores.

Suggest a suitable data structure that can be used to solve this problem efficiently. Describe the characteristics and advantages of the suggested data structure.

Implement the solution using the suggested data structure in C++.

Note: You can assume that K is a valid input, and there are no ties in average scores.
Hint: The standard library may have something that can help...

Question 7:
We will be revisiting the palindrome!
Somewhere previously in the homework, you wrote a program that would tell if a string was palindromic, returning True if a palindrome and False otherwise.
Presumably you used a solution that reversed the string and then checked equality between the strings. As discussed in class, this requires at least 3 passes of length N where N is the length of the string. 

Use a Stack data structure to solve the same problem in **one pass only**. 
(By the way, since you're here, try a priority queue for question 6.)