You should have read the Lecture 1 notes up through [[Basic Program Structure VI - Structs and Classes]].

1. Analytical Question: Consider the following code snippet:
```cpp
class Rectangle {
private:
    int width;
    int height;

public:
    Rectangle(int w, int h) {
        width = w;
        height = h;
    }

    int calculateArea() {
        return width * height;
    }
};

int main() {
    Rectangle rect(5, 10);
    int area = rect.calculateArea();
    std::cout << "Area of the rectangle: " << area << std::endl;

    return 0;
}

```

a. What is the purpose of the `private` access specifier in the `Rectangle` class? 
b. Explain the purpose of the constructor in the `Rectangle` class
c. What is the output of the program?

2. Code Writing Question: Write a class named `Student` with the following properties:

- `name` (string): to store the name of the student.
- `age` (int): to store the age of the student.
- `major` (string): to store the major of the student.

The class should have a constructor to initialize the `name`, `age`, and `major` properties. Additionally, provide a function named `displayDetails()` that prints the student's name, age, and major.


3. Code Writing Question: Write a class named `BankAccount` with the following properties:

- `accountNumber` (string): to store the account number.
- `balance` (double): to store the account balance.

The class should have a default constructor that initializes the account number as an empty string and the balance as 0. Additionally, provide functions named `deposit()` and `withdraw()` that allow the user to deposit or withdraw money from the account, respectively. The `withdraw()` function should check if the account balance is sufficient before allowing a withdrawal. 

4. Analytical Question: Consider the following code snippet:

``` cpp
class Car {
public:
    std::string model;
    int year;

    void printDetails() {
        std::cout << "Model: " << model << ", Year: " << year << std::endl;
    }
};

int main() {
    Car car1;
    car1.model = "Toyota Camry";
    car1.year = 2020;
    car1.printDetails();

    Car car2 = car1;
    car2.year = 2022;
    car2.printDetails();

    return 0;
}

```

a. What is the output of the program? 
b. Explain the concept of object copying. What is actually happening here?
	When I write `Car car2 = car1`; what am I actually doing? Visualize this and the memory involved.
c. What is the difference between a struct and a class in C++? What might affect whether you use a struct versus a class?