In this assignment, we'll briefly cover some topics from throughout Lecture 2, and help to reinforce some ideas about object composition. However, specialized topics from Lecture 2 will instead be in the form of labs.

Hence, the goal of this assignment is to instead test your capability to both analyze more complex problems and synthesize more complex solutions. 

## Part 1: Code Review Questions

``` cpp
#include <iostream>
#include <vector>

class Animal {
protected:
    std::string name;

public:
    Animal(std::string name) : name(name) {}

    virtual void makeSound() {
        std::cout << "Generic animal sound!" << std::endl;
    }
};

class Dog : public Animal {
public:
    Dog(std::string name) : Animal(name) {}

    void makeSound() override {
        std::cout << "Woof! Woof!" << std::endl;
    }
};

class Cat : public Animal {
public:
    Cat(std::string name) : Animal(name) {}

    void makeSound() override {
        std::cout << "Meow! Meow!" << std::endl;
    }
};

int main() {
    std::vector<Animal*> animals;
    animals.push_back(new Dog("Buddy"));
    animals.push_back(new Cat("Whiskers"));

    for (const auto& animal : animals) {
        animal->makeSound();
    }

    return 0;
}

```


Questions:

1. Explain the purpose of the `Animal` class. What is the significance of making the `makeSound` function virtual?
    
2. What are the benefits of using inheritance in the `Dog` and `Cat` classes? How do these classes extend or specialize the functionality of the `Animal` class?
    
3. Analyze the `main` function. What is the purpose of the `std::vector<Animal*> animals` container? Why is it used instead of a simple array or another container?
    
4. What output will the code produce when executed? Explain why.
    

Proposed Changes:

1. The code snippet lacks proper memory management. Suggest modifications to ensure that memory allocated with `new` is properly deallocated.
    
2. The current implementation of the `makeSound` function in each derived class duplicates the code for printing the sound. Propose a more efficient approach to eliminate code redundancy.
    
3. The code doesn't provide a way to add new animal types dynamically. Propose modifications to allow the addition of new derived classes without modifying the existing code.
    
4. The `name` member variable in the `Animal` class is not utilized. Suggest a modification to allow setting and retrieving the name of each animal.

## Part 2: Code-Writing Questions

### Question 1: Composition Example

Implement a simple bookstore system using composition. Design three classes: `Book`, `Author`, and `Bookstore`.

The `Book` class should have the following member variables:

- `title`: Represents the title of the book.
- `price`: Represents the price of the book.

The `Author` class should have the following member variables:

- `name`: Represents the name of the author.
- `books`: A collection of books written by the author.

The `Bookstore` class should have the following member variables:

- `name`: Represents the name of the bookstore.
- `authors`: A collection of authors associated with the bookstore.

Define appropriate member functions in each class to perform the following operations:

- Adding books to an author's collection.
- Adding authors to the bookstore's collection.
- Searching for books by a specific author.
- Displaying all the books available in the bookstore.

Ensure that the classes are properly linked using composition. Does a `Bookstore` "have-a" `Book`? Does an `Author` "have-a" `Book`, or does a `Book` "have-an" `Author`?

Note: You don't need to implement any user interface. Just write the necessary classes and their member functions.

### Question 2: Shape Hierarchy

Design a class hierarchy for different geometric shapes using inheritance. Your hierarchy should include a base class called `Shape` and derived classes for specific shapes such as `Circle`, `Rectangle`, and `Triangle`. Each derived class should inherit from the `Shape` class and provide its own implementation for calculating area and perimeter.

You may use intermediate derived classes if you want.

Define the following member functions in the `Shape` class:

- `getArea()`: Returns the area of the shape.
- `getPerimeter()`: Returns the perimeter of the shape.

Make sure to include appropriate member variables and constructors in each class to represent the shape's properties (e.g., radius for a `Circle`).

Write a program that, first, asks the user for input shapes to create. The program should keep creating new shapes as long as the user keeps providing input, and should stop creating shapes when the user enters an empty line. Your program should prompt for any necessary information to define each shape, as well.

When the user is done providing input, the program should then print information about each shape in the same order they were provided by the user, including the calculated perimeter and area.



## Part 3: Analytical Questions

1. Explain the concept of inheritance in object-oriented programming. Provide an example to illustrate its usage and benefits, and be specific.
    
2. Compare and contrast composition and inheritance. When would you choose one over the other? Give an example to support your answer. And don't give me my own example. Please, come up with your own.
    
3. Describe the fundamental principles of object-oriented programming. How do these principles help in designing and developing robust software systems? Provide specific examples from your own perspective.
	
4.  Can a derived class access a base class function which was overridden?
5.  What can you say about memory structure between base and derived classes? If I have `Base`, `DerivedA` and `DerivedB`, where the last two both inherit from `Base` and add their own variables, what does their layout in memory look like? Draw a picture/diagram.
    