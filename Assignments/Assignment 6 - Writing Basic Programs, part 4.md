You should have read the Lecture 1 notes, up through [[Basic Program Structure VII - Pointers, References, and Memory Allocation]].

1. Analytical Question: 
   a. Explain the difference between pass-by-value and pass-by-reference in function parameters. 
   b. When would you choose pass-by-value over pass-by-reference, and vice versa? Provide examples.
   c. What is the difference between these two approaches in terms of memory usage?
    
2. Code Writing Question: Write a function named `swapNumbers` that takes two integer parameters by reference and swaps their values. The function should not return anything. Provide an example usage of the `swapNumbers` function in the `main` function.
    
3. Analytical Questions:  
   a. What is a pointer in C++?  
   b. Explain the concept of dereferencing a pointer. 
   c. How are pointers useful in programming? Give an example.
    
4. Code Writing Question: Write a function named `cubeNumber` that takes an integer parameter by reference and modifies its value to the cube of the original number. The function should not return anything. Provide an example usage of the `cubeNumber` function in the `main` function.
    
5. Analytical Question: 
   a. What is dynamic memory allocation in C++? 
   b. How is dynamic memory allocated using the `new` keyword? 
   c. How is dynamically allocated memory released using the `delete` keyword?
    
6. Code Writing Question: Write a function named `createArray` that takes an integer parameter representing the size of an array. The function should dynamically allocate an integer array of the given size and return a pointer to the first element of the array. Provide an example usage of the `createArray` function in the `main` function.
    
7. Analytical Question: 
   a. What is a memory leak in programming? 
   b. How can memory leaks be avoided in C++?
    
8. Question: Consider the following code snippet:
``` cpp
void processData(int* data) {
    int* temp = new int[5];
    temp = data;
    // Perform some operations with temp
    delete[] temp;
}

int main() {
    int* array = new int[5];
    // Initialize and process the array
    processData(array);
    delete[] array;

    return 0;
}

```

a. Does the code snippet contain a memory leak? If so, explain why. 
b. Is there anything else wrong with this code? If so, explain.
c. If there is a memory leak, propose a fix for it. 
d. What does `delete[]` do? Is it different from `delete` in general? If so, explain.