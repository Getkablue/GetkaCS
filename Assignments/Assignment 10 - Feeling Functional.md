
In this assignment we'll consider functional programming from a basic perspective.

Code Analysis

``` cpp
void function1() { std::cout << "func1" << std::endl; }
void function2() { std::cout << "func2" << std::endl; }

void doXthenY(std::function<void(void)> f1, std::function<void(void)> f2) {
	f1();
	f2();
}
void doNTimes(std::function<void(void)> f, unsigned int n) {
	for (int i = 0; i < n; i++) {
		f();
	}
}

int squareNumber(int input) { return input**2; }
void printNumber(int input) { std::cout << input << std::endl; }

void passFourAndPassResult(std::function<int(int)> f, std::function<void(int)> g) {
	g(f(4));
}
// std::function is C++ only

// C-style function pointer syntax
int *functionPtr(int) = &squareNumber;

// Bind variables into a function (C++ only)
auto printSeven = std::bind(printNumber, 7);
// Note it doesn't have to be complete, either:
int multiplyTwoIntegers(int a, int b) { return a * b; }
auto multiplyByNine = std::bind(multiplyTwoIntegers, 9);

int main() {
	doXthenY(function1, function2);
	doNTimes(function1, 3);
	passFourAndPassResult(squareNumber, printNumber);

	// C style
	int result = (*functionPtr)(9);
	printNumber(result);

	// Back to C++

	printSeven();
	printNumber(multiplyByNine(8));
	
}
```

1. **Before you compile and run this**, what results do you expect to see?
2. What does `std::function` represent? What are its template parameters? How would you write a function which takes as a parameter another function which operates on three `int`s and returns a `Color`?
3. How else might you use `std::function`? Think of your own example of how you might use this. Are `std::functions` normal variables? Is there anywhere you can't use them?
4. What does `std::bind` do? 
5. Can you bind to function parameters out of order?
6. We used `auto` to have the compiler infer the type for the variable `multiplyByNine`. What type is it actually?
7. How might you use `std::bind`? Come up with your own example. 
8. In the above example code we have bound to free functions. Can you bind to a member function (i.e. a function bound to a specific object)? If so, how? Why might you do this? What does it represent?
9. Write a higher-order function which accepts a function parameter `func` and calls `func` on each value 1-100. It should then apply another function parameter, `func2`, which operates on all those values and produces a single value. When all this is done, it should print that value and return the result of another function parameter `func3` on that value. 