Often in problem solving we want to deal with a number of a certain kind of thing. For example, a problem might be "get me all individuals from this population who are taller than 5 feet and 11 inches." Both the data we are operating on, "the population", and the data we are returning, "those taller than 5'11", are collections of elements, where each element is of a certain type, "a person". Both collections are sized rather arbitrarily, and not necessarily something we necessarily know when we are being told the problem.

In C, we create *arrays* to hold multiple objects in contiguous segments of memory. Do this by adding brackets after the variable name (yeah, it's weird):
```c
int numbers[] = {25, 50, 75, 100};

// Now access elements of the array with the indexing operator
int fourthOne = numbers[4]; //OOPS! Can't do this! In programming, *most* things are "Zero-indexed", i.e their first element is at index 0.

int fourthOne = numbers[3]; // OK!
numbers[2] = 66;

// You can also declare arrays by their size and fill them in later:
int moreNumbers[3];
moreNumbers[0] = 1;
moreNumbers[1] = 2;
moreNumbers[2] = 3;

// Since C98, you can make arrays whose size depends on a variable:
int arraySize = 6;
int evenMoreNumbers[arraySize];

// But note that you can never change an array's size after creation.
```

You can also treat an `int*` as an array of ints using pointer manipulation -- a pointer to the start of a contiguous block of memory filled with ints is *what an array of ints is* anyway. I don't recommend that you manage arrays like this unless you absolutely have to.

In general, I don't recommend using arrays for most applications. Most of the time, the C++ features for this are better. There are still some cases where you'd want to use an array but this is generally for performance optimization.

In C++, we can create, and use, data structures which handle heap allocation for us, to generate collections of completely arbitrary size. The "Standard Template Library", or STL, which comes with C++ contains many good, fully generic implementations of these data structures, which can manage any type of data.

A generic is indicated by a typename, followed by another typename in angle brackets, like `std::vector<Color>`. In the next lecture we will discuss generics in more detail. For now, just know that they are a way for "container types" like `std::vector` to contain many different types, including ones we create.

```cpp
std::vector<int> vec; // Declare an (empty) vector of integers

std::vector<int> anotherVec = {55, 66, 77, 88, 99, 111, 222, 333}; /// Initialize by value-list

// Push elements to the end of the vector:
vec.push_back(1);
// Push to the beginning
vec.push_front(2);
// Get a value by removing it from the end...
int value = anotherVec.pop_back();
// Or from the beginning
int anotherValue = anotherVec.pop_front();
// anotherVec is now { 66, 77, 88, 99, 111, 222 }

// "Random" (i.e. indexed) access is also possible without removing any elements:
value = anotherVec[0] // value is now 66
anotherVec[0] = 9999;
//anotherVec is now { 9999, 77, 88, 99, 111, 222 }

// Vectors also define some useful conveniences:
int sizeOfMyVector = anotherVec.size(); // amount of elements in vector
bool myVectorIsEmpty = anotherVec.empty(); // True if empty, False otherwise

// We can grab "iterator handles" to the vector's beginning and end, and use them to loop
for (auto iterator = anotherVec.begin(); iterator != anotherVec.end(); iterator++) {
	// Iterators can be dereferenced to get the associated element 
	std::cout << "Current element: " *iterator << std::endl;
}

// We can remove elements by a specific index, or across a range:
anotherVec.erase(anotherVec.begin() + 2) // Remove the third element
// anotherVec is now { 9999, 77, 99, 111, 222 }
anotherVec.erase(anotherVec.begin(), anotherVec.begin()+2); // Remove the first 3 elements
// anotherVec is now { 111, 222 }

// We can also clear a vector easily:
anotherVec.clear();
// anotherVec is now { } (empty).
```

The `std::vector` type is a good, robust implementation which allows efficient random access, and keeps elements contiguous in memory, which helps with performance. It also defines some nice conveniences in multiple ways. In fact, I haven't covered everything `std::vector`s can do in the above. In general, it will serve your needs very well when it comes to typical applications. However, if you are constantly resizing vectors, other data structures may be preferable -- we will discuss these later.

We already talked about loops. For collections of arbitrary or unknown size (at least, those where size is not known at compile time), loops are essential for iterating over collections.
