# Complete OOP in C++ — Basic to Advanced
### For Placements | Coding Interviews | Real-World Programming

---

## Table of Contents

1. [What is OOP?](#1-what-is-oop)
2. [Class & Object](#2-class--object)
3. [Access Modifiers](#3-access-modifiers)
4. [Encapsulation](#4-encapsulation)
5. [Abstraction](#5-abstraction)
6. [Constructor & Destructor](#6-constructor--destructor)
7. [Inheritance](#7-inheritance)
8. [Polymorphism](#8-polymorphism)
9. [Method Overloading vs Overriding](#9-method-overloading-vs-overriding)
10. [Interface vs Abstract Class](#10-interface-vs-abstract-class)
11. [SOLID Principles](#11-solid-principles)
12. [Real-World OOP Design](#12-real-world-oop-design)
13. [Mini Tests](#13-mini-tests)
14. [Full Revision Notes](#14-full-revision-notes)
15. [Top 50 Interview Questions](#15-top-50-interview-questions)
16. [Coding Interview Problems](#16-coding-interview-problems)

---

## 1. What is OOP?

### Simple English
OOP (Object-Oriented Programming) is a way of writing code by thinking in terms of **real-world objects**. Instead of writing a long list of instructions, you group related data and functions together into a unit called a **class**.

### Hinglish Explanation
Socho ek **Car** hai. Car ke paas kuch **properties** hain — color, speed, fuel. Aur kuch **actions** hain — start(), stop(), accelerate(). OOP mein hum inn saari cheezein ek jagah (class) mein rakhte hain. Yahi OOP ka core idea hai — **real duniya ki cheezein code mein represent karna**.

### Real-Life Example
| Real World | OOP Concept |
|---|---|
| Car (Toyota, Honda) | Object |
| Blueprint of Car | Class |
| Color, Speed, Fuel | Data Members (Properties) |
| Start, Stop, Brake | Member Functions (Methods) |
| Car follows traffic rules | Encapsulation / Abstraction |

### 4 Pillars of OOP

```
         OOP
          |
    ______|______
   |      |      |        |
Encap  Abstract  Inherit  Polymorph
```

| Pillar | Meaning |
|---|---|
| Encapsulation | Data hide karna (wrap data + functions) |
| Abstraction | Complexity hide karna (sirf important dikhao) |
| Inheritance | Ek class se doosri class properties lena |
| Polymorphism | Same function, different behavior |

### Why OOP?
- **Reusability** — Code baar baar likho nahi
- **Modularity** — Code pieces mein divide hota hai
- **Security** — Data ko protect kar sakte ho
- **Maintainability** — Bug fix aasaan hota hai

---

### Interview Questions — What is OOP?

**Q1. What is OOP?**
> OOP is a programming paradigm that organizes code around objects — entities that bundle data (attributes) and behavior (methods) together. It is based on 4 pillars: Encapsulation, Abstraction, Inheritance, Polymorphism.

**Q2. What are the 4 pillars of OOP?**
> Encapsulation, Abstraction, Inheritance, Polymorphism.

**Q3. Difference between Procedural and OOP?**
| Procedural | OOP |
|---|---|
| Code is a list of instructions | Code is organized around objects |
| No data hiding | Data hiding via encapsulation |
| Hard to scale | Easy to scale and maintain |
| C language | C++, Java, Python |

**Q4. Is C++ fully object-oriented?**
> No. C++ is not purely OOP because it supports procedural programming too. You can write C code inside C++. Java is closer to pure OOP (though it also has primitives).

**Q5. What is a class and what is an object? (Short)**
> Class is a blueprint. Object is the actual thing created from that blueprint.

**Q6. Can we have OOP without inheritance?**
> Yes. Encapsulation and Abstraction don't need inheritance. But inheritance is one of the key features that makes OOP powerful.

**Q7. What is the difference between OOP and Object-Based language?**
> Object-Based languages (like JavaScript before ES6) support objects and encapsulation but NOT inheritance or polymorphism. OOP languages support all 4 pillars.

**Q8. What is a message in OOP?**
> When one object calls a method of another object, that is called passing a message. e.g., `car.start()` is a message to the car object.

**Q9. What is coupling in OOP?**
> Coupling means how much one class depends on another. Low coupling is good — it means classes are independent.

**Q10. What is cohesion?**
> Cohesion means how focused a class is. High cohesion is good — one class should do one thing well.

---

## 2. Class & Object

### Simple English
A **Class** is like a **template or blueprint**. An **Object** is a real thing created from that template.

### Hinglish Explanation
Socho ek **Architect ka naksha (blueprint)** hai ghar banana ke liye. Woh blueprint ek **Class** hai. Jab tum us blueprint se asli ghar banate ho — woh **Object** hai. Ek blueprint se hazaron ghar ban sakte hain, waise hi ek class se hazaron objects ban sakte hain.

### Real-Life Example
- Class = Cookie cutter (biscuit banane ka saancha)
- Object = Actual biscuit

### Diagram
```
CLASS: Car
+---------------------------+
| - color: string           |  <-- Data Members (Private)
| - speed: int              |
| - fuel: float             |
+---------------------------+
| + start()                 |  <-- Member Functions (Public)
| + stop()                  |
| + accelerate(int speed)   |
+---------------------------+

OBJECTS:
car1 (Toyota, 120kmph, 40L)
car2 (Honda, 100kmph, 35L)
car3 (BMW, 200kmph, 50L)
```

### C++ Code Example

```cpp
#include <iostream>
#include <string>
using namespace std;

class Car {
private:
    string color;
    int speed;
    float fuel;

public:
    void setDetails(string c, int s, float f) {
        color = c;
        speed = s;
        fuel = f;
    }

    void start() {
        cout << color << " car is starting..." << endl;
    }

    void showDetails() {
        cout << "Color: " << color << ", Speed: " << speed << ", Fuel: " << fuel << endl;
    }
};

int main() {
    Car car1;
    car1.setDetails("Red", 120, 40.5);
    car1.start();
    car1.showDetails();

    Car car2;
    car2.setDetails("Blue", 100, 35.0);
    car2.start();
    car2.showDetails();

    return 0;
}
```

**Output:**
```
Red car is starting...
Color: Red, Speed: 120, Fuel: 40.5
Blue car is starting...
Color: Blue, Speed: 100, Fuel: 35
```

### Key Points
- Class ke andar **data members** hote hain (variables)
- Class ke andar **member functions** hote hain (functions/methods)
- Object create karna = **instantiation**
- `Car car1;` — yahan car1 ek object hai Car class ka

### this Pointer

```cpp
class Car {
private:
    string color;
public:
    void setColor(string color) {
        this->color = color;  // 'this' points to current object
    }
};
```

`this` pointer current object ko point karta hai. Jab parameter aur member variable ka naam same ho tab `this->` use karte hain.

---

### Interview Questions — Class & Object

**Q1. What is the difference between a class and an object?**
> Class is a blueprint/template. Object is an instance (real entity) created from that class. A class doesn't occupy memory; an object does.

**Q2. What is instantiation?**
> Creating an object from a class is called instantiation. e.g., `Car c1;`

**Q3. What is the `this` pointer?**
> `this` is an implicit pointer available inside every non-static member function. It points to the object that called the function.

**Q4. Can a class exist without any object?**
> Yes. A class is just a definition. Objects are created only when needed.

**Q5. How is memory allocated for class and object?**
> Class definition itself takes no memory. When an object is created, memory is allocated for its data members (not functions — functions are shared).

**Q6. Tricky: What is the size of an empty class?**
```cpp
class Empty {};
cout << sizeof(Empty); // Output: 1
```
> An empty class has size 1 byte. This is to ensure each object has a unique address.

**Q7. Can we have a class inside a class?**
> Yes, it is called a **nested class**. The inner class is defined inside the outer class.

**Q8. What is a static data member?**
> A static data member is shared among all objects of the class. It is declared with `static` keyword.

```cpp
class Counter {
public:
    static int count;
    Counter() { count++; }
};
int Counter::count = 0;
```

**Q9. What is the difference between struct and class in C++?**
| struct | class |
|---|---|
| Members are public by default | Members are private by default |
| Used for simple data grouping | Used for full OOP |

**Q10. Output-based question:**
```cpp
class Test {
    int x;
public:
    Test(int x) { this->x = x; }
    void show() { cout << x; }
};
int main() {
    Test t(5);
    t.show();
}
```
> Output: `5`

---

## 3. Access Modifiers

### Simple English
Access modifiers control **who can access what** inside a class. It is the first line of security in OOP.

### Hinglish Explanation
Ghar mein kuch rooms **public** hote hain (drawing room — sab aa sakte hain), kuch **private** (bedroom — sirf family), aur kuch **protected** (store room — family + trusted relatives). Isi concept ko C++ mein access modifiers kehte hain.

### Types

| Modifier | Within Class | Derived Class | Outside Class |
|---|---|---|---|
| `private` | YES | NO | NO |
| `protected` | YES | YES | NO |
| `public` | YES | YES | YES |

### C++ Code

```cpp
#include <iostream>
using namespace std;

class BankAccount {
private:
    double balance;       // only this class can access

protected:
    string accountType;   // this class + derived classes

public:
    string ownerName;     // everyone can access

    void setBalance(double b) {
        if (b >= 0) balance = b;
    }

    double getBalance() {
        return balance;
    }
};

int main() {
    BankAccount acc;
    acc.ownerName = "Rahul";         // OK - public
    acc.setBalance(5000);            // OK - public method
    cout << acc.getBalance();        // OK
    // acc.balance = 1000;           // ERROR - private
    return 0;
}
```

### Diagram
```
BankAccount
+------------------------------+
| private:                     |
|   balance (only this class)  |
+------------------------------+
| protected:                   |
|   accountType (+ child)      |
+------------------------------+
| public:                      |
|   ownerName (everyone)       |
|   getBalance()               |
+------------------------------+
```

---

### Interview Questions — Access Modifiers

**Q1. What are the three access modifiers in C++?**
> private, protected, public.

**Q2. What is the default access modifier in a class?**
> `private` in C++ class. `public` in struct.

**Q3. When to use protected?**
> When you want subclasses to access the member but not outsiders. Used in inheritance.

**Q4. Can we access private members from outside?**
> Not directly. But using **friend functions** or **public getter/setter methods**.

**Q5. What is a friend function?**
> A non-member function that has access to private/protected members of a class.
```cpp
class Box {
private:
    int width;
public:
    Box(int w) : width(w) {}
    friend void showWidth(Box b);
};
void showWidth(Box b) {
    cout << b.width; // allowed because of friend
}
```

**Q6. Tricky: What is the difference between private and protected in inheritance?**
> `private` members are NOT accessible in derived class. `protected` members ARE accessible in derived class but not outside.

**Q7. Can a derived class access private members of base class?**
> No, not directly. Only through public/protected methods of the base class.

**Q8. Output-based:**
```cpp
class A {
    int x = 10;
public:
    int y = 20;
};
int main() {
    A obj;
    cout << obj.y; // 20
    // cout << obj.x; // Error
}
```
> Output: `20`

---

## 4. Encapsulation

### Simple English
Encapsulation means **wrapping data and functions together** into a single unit (class), and **hiding the internal details** from outside. It is like a **medicine capsule** — medicine is inside, you just take it.

### Hinglish Explanation
Socho ek **ATM machine**. Tum card daalo, PIN daalo, paisa nikalte ho. Andar kya ho raha hai — circuitry, software, network calls — yeh sab tumhe pata nahi. Aur pata hona bhi nahi chahiye. Yahi **Encapsulation** hai — internal implementation chhupao, sirf interface dikhao.

### Real-Life Example
- **Mobile Phone** — Battery, circuit board andar hain (private). Power button, volume button bahar hain (public interface).
- **Bank Account** — Balance private hai. deposit() aur withdraw() public methods hain.

### Diagram
```
+---------------------------------------+
|         BankAccount (Capsule)         |
|   +-------------------------------+   |
|   |  PRIVATE (Hidden)             |   |
|   |  - balance: double            |   |
|   |  - pin: int                   |   |
|   +-------------------------------+   |
|   PUBLIC (Interface)                  |
|   + deposit(amount)                   |
|   + withdraw(amount)                  |
|   + getBalance()                      |
+---------------------------------------+
```

### C++ Code

```cpp
#include <iostream>
using namespace std;

class BankAccount {
private:
    double balance;
    string pin;

public:
    BankAccount(double initialBalance, string p) {
        balance = initialBalance;
        pin = p;
    }

    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "Deposited: " << amount << endl;
        }
    }

    bool withdraw(double amount, string enteredPin) {
        if (enteredPin != pin) {
            cout << "Wrong PIN!" << endl;
            return false;
        }
        if (amount > balance) {
            cout << "Insufficient balance!" << endl;
            return false;
        }
        balance -= amount;
        cout << "Withdrawn: " << amount << endl;
        return true;
    }

    double getBalance() {
        return balance;
    }
};

int main() {
    BankAccount acc(5000, "1234");
    acc.deposit(2000);
    acc.withdraw(1000, "1234");
    acc.withdraw(1000, "0000");
    cout << "Balance: " << acc.getBalance() << endl;
    return 0;
}
```

**Output:**
```
Deposited: 2000
Withdrawn: 1000
Wrong PIN!
Balance: 6000
```

### Benefits of Encapsulation
1. **Data Security** — balance ko directly change nahi kar sakte
2. **Validation** — withdraw mein check hai ki balance enough hai
3. **Flexibility** — internal implementation change kar sako bina external code touche kiye
4. **Maintainability** — easy to debug

---

### Interview Questions — Encapsulation

**Q1. What is encapsulation?**
> Encapsulation is the bundling of data (variables) and methods (functions) that operate on the data into a single unit (class), while restricting direct access to some components. It is achieved using access modifiers.

**Q2. What is data hiding?**
> Data hiding means making data members private so they cannot be accessed directly from outside the class. It's a part of encapsulation.

**Q3. Is encapsulation same as data hiding?**
> Not exactly. Encapsulation = bundling data + functions. Data hiding = protecting data from direct access. Data hiding is achieved through encapsulation.

**Q4. What are getter and setter methods?**
> Getters are public methods to READ private data. Setters are public methods to WRITE (with validation) private data.

**Q5. Why are setters useful?**
> Setters allow validation before setting a value. e.g., you can prevent setting age = -5.

**Q6. Real-world example of encapsulation?**
> ATM machine, TV remote, Car engine — you use the interface, you don't mess with internals.

**Q7. Tricky: Can we break encapsulation in C++?**
> Yes, using `friend` functions or `friend` classes. They can access private members. Also, pointers and type casting can sometimes bypass it.

**Q8. What happens if we don't use encapsulation?**
> Any part of code can modify data directly, leading to data corruption, hard-to-find bugs, and security vulnerabilities.

**Q9. Output-based:**
```cpp
class Test {
    int x = 0;
public:
    void setX(int val) { if(val > 0) x = val; }
    int getX() { return x; }
};
int main() {
    Test t;
    t.setX(-5);
    t.setX(10);
    cout << t.getX();
}
```
> Output: `10` (because -5 is rejected by validation)

**Q10. Can structs achieve encapsulation?**
> Technically yes, by using private members in struct. But conventionally, classes are used for encapsulation.

---

## 5. Abstraction

### Simple English
Abstraction means **showing only what is necessary** and **hiding the complex details**. You use something without knowing how it works internally.

### Hinglish Explanation
Tum **TV remote** use karte ho. Volume up karne ke liye button dabate ho. Par andar infrared signal kaise banta hai, microcontroller kaise kaam karta hai — yeh sab chhupa hai. Tumhe sirf **interface** (buttons) pata hai, **implementation** (circuit) nahi. Yahi **Abstraction** hai.

### Difference: Encapsulation vs Abstraction

| Feature | Encapsulation | Abstraction |
|---|---|---|
| What it does | Bundles data + methods, hides data | Hides implementation complexity |
| How achieved | Access modifiers (private/public) | Abstract classes, Interfaces |
| Focus | Data protection | Design simplification |
| Example | BankAccount hides balance | ATM hides how transaction works |

### Abstraction in C++ — Two Ways

**1. Using Abstract Class (with pure virtual function)**
**2. Using Interface (all pure virtual functions)**

### C++ Code

```cpp
#include <iostream>
using namespace std;

// Abstract class — Shape
class Shape {
public:
    virtual double area() = 0;     // Pure virtual function
    virtual double perimeter() = 0;

    void describe() {
        cout << "This is a shape with area: " << area() << endl;
    }
};

class Circle : public Shape {
private:
    double radius;
public:
    Circle(double r) : radius(r) {}

    double area() override {
        return 3.14159 * radius * radius;
    }

    double perimeter() override {
        return 2 * 3.14159 * radius;
    }
};

class Rectangle : public Shape {
private:
    double length, width;
public:
    Rectangle(double l, double w) : length(l), width(w) {}

    double area() override {
        return length * width;
    }

    double perimeter() override {
        return 2 * (length + width);
    }
};

int main() {
    Shape* s1 = new Circle(5);
    Shape* s2 = new Rectangle(4, 6);

    s1->describe();
    s2->describe();

    delete s1;
    delete s2;
    return 0;
}
```

**Output:**
```
This is a shape with area: 78.5397
This is a shape with area: 24
```

### Diagram
```
         Shape (Abstract)
         [area() = 0]
         [perimeter() = 0]
              |
    __________|__________
    |                   |
  Circle            Rectangle
  [area()]           [area()]
  [perimeter()]      [perimeter()]

User sees: shape->area()
User doesn't see: HOW area is calculated
```

---

### Interview Questions — Abstraction

**Q1. What is abstraction in OOP?**
> Abstraction means hiding the internal implementation details and showing only the essential features to the user. It is achieved using abstract classes and interfaces.

**Q2. What is a pure virtual function?**
> A pure virtual function is declared in base class with `= 0` and has NO implementation. Any derived class MUST implement it.
```cpp
virtual void draw() = 0;
```

**Q3. What is an abstract class?**
> A class that has at least one pure virtual function. It cannot be instantiated directly. It serves as a blueprint for derived classes.

**Q4. Can an abstract class have a constructor?**
> Yes. Abstract classes can have constructors. They are called when a derived class object is created.

**Q5. Difference between abstract class and interface?**
> (Covered in detail in Topic 10, but briefly:) Abstract class can have both pure virtual AND regular functions + data. Interface (pure abstract class in C++) has ONLY pure virtual functions.

**Q6. Tricky: Can we create an object of an abstract class?**
> NO. Compiler gives error. We can only create pointers/references to abstract class.
```cpp
Shape s;    // ERROR
Shape* s;   // OK
```

**Q7. Real-world example of abstraction?**
> Car's accelerator — you press it, car speeds up. You don't need to know about fuel injection, engine pistons, etc.

**Q8. What is the difference between abstraction and encapsulation?**
> Encapsulation = hiding DATA. Abstraction = hiding IMPLEMENTATION COMPLEXITY. Both work together.

**Q9. Output-based:**
```cpp
class Base {
public:
    virtual void show() = 0;
    void print() { cout << "Base"; }
};
class Derived : public Base {
public:
    void show() { cout << "Derived"; }
};
int main() {
    Base* b = new Derived();
    b->show();
    b->print();
}
```
> Output: `DerivedBase`

**Q10. What happens if a derived class doesn't implement all pure virtual functions?**
> The derived class also becomes abstract and cannot be instantiated.

---

## 6. Constructor & Destructor

### Simple English
- **Constructor** = Automatically called when object is CREATED. Initializes the object.
- **Destructor** = Automatically called when object is DESTROYED. Cleans up resources.

### Hinglish Explanation
- Constructor = Ghar mein **grihapravesh** — jab ghar banta hai tab hota hai, sab setup karo (furniture, bijli).
- Destructor = **Ghar todne se pehle** — sab cheez hata lo, saaf karo, tab todna.

### Types of Constructors

| Type | Description |
|---|---|
| Default Constructor | No parameters |
| Parameterized Constructor | Takes parameters |
| Copy Constructor | Creates copy of an object |
| Move Constructor | Moves resources (C++11) |

### C++ Code

```cpp
#include <iostream>
#include <string>
using namespace std;

class Student {
private:
    string name;
    int age;
    int* marks;   // Dynamic memory to show destructor

public:
    // Default Constructor
    Student() {
        name = "Unknown";
        age = 0;
        marks = new int[5];
        cout << "Default constructor called for Unknown" << endl;
    }

    // Parameterized Constructor
    Student(string n, int a) {
        name = n;
        age = a;
        marks = new int[5];
        cout << "Parameterized constructor called for " << name << endl;
    }

    // Copy Constructor
    Student(const Student& s) {
        name = s.name;
        age = s.age;
        marks = new int[5];  // Deep copy
        for (int i = 0; i < 5; i++) marks[i] = s.marks[i];
        cout << "Copy constructor called for " << name << endl;
    }

    void display() {
        cout << "Name: " << name << ", Age: " << age << endl;
    }

    // Destructor
    ~Student() {
        delete[] marks;  // Free dynamic memory
        cout << "Destructor called for " << name << endl;
    }
};

int main() {
    Student s1;
    Student s2("Rahul", 20);
    Student s3(s2);   // Copy constructor

    s1.display();
    s2.display();
    s3.display();

    return 0;
}
```

**Output:**
```
Default constructor called for Unknown
Parameterized constructor called for Rahul
Copy constructor called for Rahul
Name: Unknown, Age: 0
Name: Rahul, Age: 20
Name: Rahul, Age: 20
Destructor called for Rahul
Destructor called for Rahul
Destructor called for Unknown
```

Note: Destructors are called in **reverse order** of construction (LIFO).

### Initializer List

```cpp
class Point {
    int x, y;
public:
    // Using initializer list (efficient)
    Point(int a, int b) : x(a), y(b) {
        cout << "Point created: " << x << ", " << y << endl;
    }
};
```

### Constructor Overloading

```cpp
class Box {
    int length, width, height;
public:
    Box() : length(1), width(1), height(1) {}
    Box(int l) : length(l), width(l), height(l) {}
    Box(int l, int w, int h) : length(l), width(w), height(h) {}
};
```

### Shallow Copy vs Deep Copy

```
SHALLOW COPY                    DEEP COPY
obj1: ptr -----> [data]         obj1: ptr1 -----> [data copy]
obj2: ptr -----> same[data]     obj2: ptr2 -----> [data copy]

Problem: delete obj1 frees       Both independent — safe
data, obj2's ptr dangles!
```

---

### Interview Questions — Constructor & Destructor

**Q1. What is a constructor?**
> A constructor is a special member function that is automatically called when an object is created. It has the same name as the class and no return type.

**Q2. Can a constructor have a return type?**
> No. Constructors never have a return type, not even void.

**Q3. What is a destructor?**
> A destructor is a special member function called automatically when an object goes out of scope or is explicitly deleted. It has `~` prefix and no parameters.

**Q4. Can a class have multiple constructors?**
> Yes, through **constructor overloading** — multiple constructors with different parameters.

**Q5. What is a copy constructor?**
> A constructor that creates a new object as a copy of an existing object. Signature: `ClassName(const ClassName& obj)`.

**Q6. Tricky: What is the difference between shallow copy and deep copy?**
> Shallow copy copies pointer values (both objects point to same memory). Deep copy allocates new memory and copies content (independent objects). Deep copy is needed when class has pointer/dynamic members.

**Q7. When is copy constructor called?**
> 1. When object is initialized from another object: `Student s2 = s1;`
> 2. When object is passed by value to a function
> 3. When function returns object by value

**Q8. Can we have a virtual constructor?**
> No. Constructors cannot be virtual. Virtual mechanism requires an object to exist (vtable), but constructor creates the object.

**Q9. Can we have a virtual destructor?**
> Yes, and it's IMPORTANT to have virtual destructor in base class when using polymorphism.
```cpp
class Base {
public:
    virtual ~Base() { cout << "Base destroyed"; }
};
```

**Q10. Tricky Output:**
```cpp
class A {
public:
    A() { cout << "A "; }
    ~A() { cout << "~A "; }
};
class B {
    A a;
public:
    B() { cout << "B "; }
    ~B() { cout << "~B "; }
};
int main() {
    B obj;
}
```
> Output: `A B ~B ~A`
> (Member object A constructed first, then B. Destroyed in reverse: B first, then A)

---

## 7. Inheritance

### Simple English
Inheritance means a **child class** can reuse properties and methods of a **parent class**. The child gets everything from the parent and can add its own things too.

### Hinglish Explanation
Socho **Bank Account** ek parent class hai. **Savings Account** aur **Current Account** usse inherit karte hain. Dono ko basic features chahiye — balance, deposit, withdraw. Lekin Savings Account ka apna interest rate hai, Current Account ka overdraft. Inheritance mein common code ek jagah likho, baaki derived class mein add karo.

### Types of Inheritance in C++

```
1. Single:        A --> B
2. Multiple:      A, B --> C
3. Multilevel:    A --> B --> C
4. Hierarchical:  A --> B, A --> C
5. Hybrid:        Mix of above
```

### Diagram — Bank Account Example
```
         Account (Base)
         - balance
         + deposit()
         + withdraw()
         + getBalance()
              |
    __________|__________
    |                   |
SavingsAccount       CurrentAccount
- interestRate       - overdraftLimit
+ addInterest()      + overdraft()
```

### C++ Code

```cpp
#include <iostream>
using namespace std;

// Base Class
class Account {
protected:
    string owner;
    double balance;

public:
    Account(string name, double bal) : owner(name), balance(bal) {}

    void deposit(double amount) {
        balance += amount;
        cout << "Deposited " << amount << " to " << owner << "'s account" << endl;
    }

    virtual void withdraw(double amount) {
        if (amount > balance) {
            cout << "Insufficient funds!" << endl;
        } else {
            balance -= amount;
            cout << "Withdrawn " << amount << " from " << owner << "'s account" << endl;
        }
    }

    virtual void showDetails() {
        cout << "Owner: " << owner << ", Balance: " << balance << endl;
    }

    virtual ~Account() {}
};

// Derived Class 1
class SavingsAccount : public Account {
private:
    double interestRate;

public:
    SavingsAccount(string name, double bal, double rate)
        : Account(name, bal), interestRate(rate) {}

    void addInterest() {
        double interest = balance * interestRate / 100;
        balance += interest;
        cout << "Interest added: " << interest << endl;
    }

    void showDetails() override {
        Account::showDetails();
        cout << "Type: Savings, Interest Rate: " << interestRate << "%" << endl;
    }
};

// Derived Class 2
class CurrentAccount : public Account {
private:
    double overdraftLimit;

public:
    CurrentAccount(string name, double bal, double limit)
        : Account(name, bal), overdraftLimit(limit) {}

    void withdraw(double amount) override {
        if (amount > balance + overdraftLimit) {
            cout << "Exceeds overdraft limit!" << endl;
        } else {
            balance -= amount;
            cout << "Withdrawn " << amount << " (with overdraft if needed)" << endl;
        }
    }

    void showDetails() override {
        Account::showDetails();
        cout << "Type: Current, Overdraft Limit: " << overdraftLimit << endl;
    }
};

int main() {
    SavingsAccount sa("Rahul", 10000, 5.0);
    CurrentAccount ca("Priya", 5000, 2000);

    sa.deposit(2000);
    sa.addInterest();
    sa.showDetails();

    cout << endl;

    ca.deposit(1000);
    ca.withdraw(7000);   // uses overdraft
    ca.showDetails();

    return 0;
}
```

### Multiple Inheritance

```cpp
class Father {
public:
    void cook() { cout << "Father can cook" << endl; }
};

class Mother {
public:
    void sing() { cout << "Mother can sing" << endl; }
};

class Child : public Father, public Mother {
public:
    void play() { cout << "Child can play" << endl; }
};

int main() {
    Child c;
    c.cook();  // from Father
    c.sing();  // from Mother
    c.play();  // own
}
```

### Diamond Problem

```
        Animal
       /      \
    Dog        Cat
       \      /
        DogCat?

Problem: DogCat inherits Animal TWICE — ambiguity!
Solution: Virtual Inheritance
```

```cpp
class Animal { public: void eat() {} };
class Dog : virtual public Animal {};
class Cat : virtual public Animal {};
class DogCat : public Dog, public Cat {};
// Now only ONE copy of Animal
```

---

### Interview Questions — Inheritance

**Q1. What is inheritance?**
> Inheritance is a mechanism where a derived class acquires properties and behaviors of a base class. It promotes code reusability.

**Q2. What are the types of inheritance in C++?**
> Single, Multiple, Multilevel, Hierarchical, Hybrid.

**Q3. What is the diamond problem?**
> When a class inherits from two classes that both inherit from the same base class, the base class is included twice. This is the diamond problem. Solved using virtual inheritance.

**Q4. What is `virtual` inheritance?**
> Virtual inheritance ensures only ONE copy of the base class is present in the inheritance hierarchy, solving the diamond problem.

**Q5. Tricky: What is inherited and what is NOT inherited?**
> Inherited: public and protected members.
> NOT inherited: private members, constructors, destructors, assignment operators, friend functions.

**Q6. What is `super` equivalent in C++?**
> C++ uses `BaseClassName::method()` to call base class methods. Java uses `super.method()`.

**Q7. What is the order of constructor calls in inheritance?**
> Base class constructor is called FIRST, then derived class constructor.

**Q8. Can a derived class be a base class for another class?**
> Yes. This is **multilevel inheritance**.

**Q9. What is `protected` inheritance?**
> In `protected` inheritance, public members of base become protected in derived. In `private` inheritance, all base members become private in derived.

**Q10. Tricky Output:**
```cpp
class A {
public:
    A() { cout << "A "; }
};
class B : public A {
public:
    B() { cout << "B "; }
};
class C : public B {
public:
    C() { cout << "C "; }
};
int main() { C obj; }
```
> Output: `A B C` (base first, then derived)

---

## 8. Polymorphism

### Simple English
Polymorphism = **Many Forms**. The same function name can behave differently in different situations.

### Hinglish Explanation
Socho **`+` operator**. `2 + 3 = 5` (integer addition). `"Hello" + " World" = "Hello World"` (string concatenation). Same `+` operator, alag alag kaam. Yahi **Polymorphism** hai.

### Types of Polymorphism

```
        Polymorphism
             |
    _________|_________
    |                 |
Compile-Time      Run-Time
(Static)          (Dynamic)
    |                 |
Function          Virtual
Overloading       Functions
Operator
Overloading
```

### Compile-Time Polymorphism (Static Binding)
Decided at **compile time**. Faster.

**1. Function Overloading:**

```cpp
#include <iostream>
using namespace std;

class Calculator {
public:
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
};

int main() {
    Calculator calc;
    cout << calc.add(2, 3) << endl;         // 5
    cout << calc.add(2.5, 3.5) << endl;     // 6.0
    cout << calc.add(1, 2, 3) << endl;      // 6
}
```

**2. Operator Overloading:**

```cpp
class Complex {
public:
    int real, imag;
    Complex(int r, int i) : real(r), imag(i) {}

    Complex operator+(const Complex& c) {
        return Complex(real + c.real, imag + c.imag);
    }

    void display() {
        cout << real << " + " << imag << "i" << endl;
    }
};

int main() {
    Complex c1(3, 4), c2(1, 2);
    Complex c3 = c1 + c2;  // calls operator+
    c3.display();           // 4 + 6i
}
```

### Run-Time Polymorphism (Dynamic Binding)
Decided at **run time** using **virtual functions**. Slower (vtable lookup) but flexible.

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void sound() {
        cout << "Some animal sound" << endl;
    }

    virtual ~Animal() {}
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Woof! Woof!" << endl;
    }
};

class Cat : public Animal {
public:
    void sound() override {
        cout << "Meow! Meow!" << endl;
    }
};

class Cow : public Animal {
public:
    void sound() override {
        cout << "Moo! Moo!" << endl;
    }
};

void makeSound(Animal* animal) {
    animal->sound();  // Which sound? Decided at RUNTIME!
}

int main() {
    Animal* animals[3];
    animals[0] = new Dog();
    animals[1] = new Cat();
    animals[2] = new Cow();

    for (int i = 0; i < 3; i++) {
        makeSound(animals[i]);
    }

    for (int i = 0; i < 3; i++) delete animals[i];

    return 0;
}
```

**Output:**
```
Woof! Woof!
Meow! Meow!
Moo! Moo!
```

### How Virtual Functions Work (vtable)

```
Each class with virtual functions has a vtable (virtual table)

Animal vtable:           Dog vtable:
[sound] --> Animal::sound   [sound] --> Dog::sound

Animal* ptr = new Dog();
ptr->sound()
  --> looks at vtable of Dog
  --> calls Dog::sound()
  --> "Woof!"
```

---

### Interview Questions — Polymorphism

**Q1. What is polymorphism?**
> Polymorphism means "many forms." The same function or operator behaves differently based on the context. Two types: compile-time (static) and runtime (dynamic).

**Q2. Difference between compile-time and run-time polymorphism?**
| Compile-Time | Run-Time |
|---|---|
| Resolved at compile time | Resolved at run time |
| Function overloading, operator overloading | Virtual functions |
| Faster | Slightly slower (vtable) |
| Static binding | Dynamic binding |

**Q3. What is a virtual function?**
> A virtual function is a member function declared with `virtual` in base class and overridden in derived class. It enables runtime polymorphism. The correct function is called based on the actual type of the object.

**Q4. What is a vtable?**
> vtable (virtual table) is a lookup table created for classes with virtual functions. Each class has its own vtable. When a virtual function is called through a pointer, the vtable determines which function to call at runtime.

**Q5. What is a vptr?**
> vptr (virtual pointer) is a hidden pointer added to every object of a class with virtual functions. It points to the vtable of that class.

**Q6. Tricky: What is the output?**
```cpp
class Base {
public:
    void show() { cout << "Base "; }
    virtual void print() { cout << "Base "; }
};
class Derived : public Base {
public:
    void show() { cout << "Derived "; }
    void print() override { cout << "Derived "; }
};
int main() {
    Base* b = new Derived();
    b->show();   // ?
    b->print();  // ?
}
```
> Output: `Base Derived`
> `show()` is NOT virtual, so Base::show() is called (static binding).
> `print()` IS virtual, so Derived::print() is called (dynamic binding).

**Q7. Can we override non-virtual functions?**
> We can redefine them in derived class (method hiding), but it won't be polymorphic. Using a base class pointer will call the base version.

**Q8. What is the `override` keyword in C++11?**
> `override` tells the compiler "this function is overriding a base class virtual function." If the base doesn't have such virtual function, compiler gives error — helps catch bugs.

**Q9. What is the `final` keyword?**
> `final` prevents a virtual function from being overridden further, or prevents a class from being inherited.
```cpp
class Base {
    virtual void foo() final;
};
class Derived : public Base {
    void foo() override; // ERROR!
};
```

**Q10. Can constructors be virtual? Why/Why not?**
> No. Constructors cannot be virtual because virtual mechanism requires vtable, which is set up during construction. The object doesn't fully exist yet when constructor runs.

---

## 9. Method Overloading vs Overriding

### Quick Comparison

| Feature | Overloading | Overriding |
|---|---|---|
| Where | Same class | Base vs Derived class |
| Signature | Different parameters | Same signature |
| Type | Compile-time polymorphism | Runtime polymorphism |
| `virtual` needed | No | Yes (in base) |
| Return type | Can differ | Must be same (or covariant) |

### Overloading Example

```cpp
class Math {
public:
    int square(int x) { return x * x; }
    double square(double x) { return x * x; }  // overloaded
    long square(long x) { return x * x; }       // overloaded
};
```

### Overriding Example

```cpp
class Shape {
public:
    virtual double area() { return 0; }  // base
};
class Circle : public Shape {
    double r;
public:
    Circle(double r) : r(r) {}
    double area() override { return 3.14 * r * r; }  // overrides
};
```

### Common Mistakes

```cpp
// MISTAKE: Thinking this is overriding
class Base {
public:
    void show(int x) { cout << "Base: " << x; }
};
class Derived : public Base {
public:
    void show(double x) { cout << "Derived: " << x; }
    // This is OVERLOADING in derived, not overriding!
    // Base::show(int) is HIDDEN, not overridden
};
```

---

### Interview Questions — Overloading vs Overriding

**Q1. What is function overloading?**
> Having multiple functions with the same name but different parameter lists (type/number/order) in the same class.

**Q2. What is function overriding?**
> A derived class provides its own implementation of a virtual function defined in the base class. Same name, same signature.

**Q3. Can we overload virtual functions?**
> Yes. We can overload AND override. A virtual function can be overloaded in the derived class (different parameters) and also overridden (same parameters).

**Q4. What is covariant return type?**
> An overriding function can return a pointer/reference to a derived class instead of base class.
```cpp
class Base { virtual Base* clone(); };
class Derived : public Base { Derived* clone() override; }; // covariant
```

**Q5. Tricky: Is this overloading or overriding?**
```cpp
class A {
public:
    virtual void show(int x) {}
};
class B : public A {
public:
    void show(double x) {}  // overloads, HIDES A::show(int)
};
```
> Neither override nor simple overload — it's **method hiding**! B hides A's show(int).

---

## 10. Interface vs Abstract Class

### Simple English
- **Abstract Class** = Partially defined class. Has SOME implementation + SOME pure virtual functions.
- **Interface** = Fully abstract. ALL functions are pure virtual. No data. Pure contract.

### Hinglish Explanation
- Abstract class = **Incomplete blueprint**. Kuch kaam hua, kuch tumhe karna hai.
- Interface = **Agreement/Contract**. "Jo bhi implement karega, ye functions zaroor honge" — no implementation, sirf promise.

### In C++: Interface = Pure Abstract Class

```cpp
// Interface (Pure Abstract Class)
class IShape {
public:
    virtual double area() = 0;
    virtual double perimeter() = 0;
    virtual void draw() = 0;
    virtual ~IShape() = default;
};

// Abstract Class (partial implementation)
class Shape {
protected:
    string color;
public:
    Shape(string c) : color(c) {}
    virtual double area() = 0;         // pure virtual
    void setColor(string c) { color = c; }  // implemented
    string getColor() { return color; }     // implemented
    virtual ~Shape() {}
};

// Concrete Class
class Circle : public Shape {
    double radius;
public:
    Circle(double r, string c) : Shape(c), radius(r) {}
    double area() override { return 3.14 * radius * radius; }
};
```

### Comparison Table

| Feature | Abstract Class | Interface (Pure Abstract) |
|---|---|---|
| Pure virtual functions | At least one | ALL functions |
| Concrete methods | YES | NO |
| Data members | YES | NO (ideally) |
| Multiple inheritance | Limited | Yes (can implement many) |
| Constructor | YES | NO |
| Use case | "is-a" relationship | "can-do" capability |

### Real-World Example

```
Interface: IPrintable
+ print()

Interface: ISaveable
+ save()
+ load()

Document class implements both:
class Document : public IPrintable, public ISaveable {
    void print() { ... }
    void save() { ... }
    void load() { ... }
}
```

---

### Interview Questions — Interface vs Abstract Class

**Q1. What is an abstract class?**
> A class with at least one pure virtual function. Cannot be instantiated. Used as a base class template.

**Q2. What is an interface?**
> A pure abstract class where ALL methods are pure virtual and there are no data members. Defines a contract.

**Q3. When to use abstract class vs interface?**
> Use abstract class when you have common implementation to share. Use interface when you just want to define a contract (especially for multiple inheritance).

**Q4. Can abstract class have a constructor?**
> Yes. Though you can't instantiate it directly, its constructor is called when a derived class object is created.

**Q5. Can we have a pointer to abstract class?**
> Yes. `Shape* s = new Circle(5, "red");` — this is the key to polymorphism.

**Q6. Tricky: What if derived class has only SOME pure virtual implementations?**
> The derived class is still abstract. All pure virtual functions must be implemented to make it concrete.

**Q7. Multiple inheritance with interfaces — is it safe?**
> Yes, because interfaces have no data and no implementation, there is no ambiguity (no diamond problem with data).

---

## 11. SOLID Principles

### What is SOLID?

SOLID = 5 design principles for writing clean, maintainable OOP code.

| Letter | Principle | Simple Meaning |
|---|---|---|
| S | Single Responsibility | Ek class ka ek kaam |
| O | Open/Closed | Open for extension, closed for modification |
| L | Liskov Substitution | Derived class base class ki jagah use ho sake |
| I | Interface Segregation | Chhote chhote interfaces banao |
| D | Dependency Inversion | Abstraction pe depend karo, concrete pe nahi |

### S — Single Responsibility Principle

```cpp
// BAD: One class doing too many things
class Employee {
    void calculateSalary() {}
    void saveToDatabase() {}   // Shouldn't be here!
    void generateReport() {}   // Shouldn't be here!
};

// GOOD: Each class has ONE responsibility
class Employee {
    void calculateSalary() {}
};
class EmployeeRepository {
    void saveToDatabase(Employee e) {}
};
class EmployeeReporter {
    void generateReport(Employee e) {}
};
```

### O — Open/Closed Principle

```cpp
// BAD: Modifying existing class every time
class DiscountCalculator {
    double getDiscount(string customerType) {
        if (customerType == "Regular") return 0.05;
        if (customerType == "Premium") return 0.10;
        // Need to modify every time new type added!
    }
};

// GOOD: Open for extension
class IDiscount {
public:
    virtual double getDiscount() = 0;
};
class RegularDiscount : public IDiscount {
public:
    double getDiscount() override { return 0.05; }
};
class PremiumDiscount : public IDiscount {
public:
    double getDiscount() override { return 0.10; }
};
// New type? Add new class, don't modify existing!
```

### L — Liskov Substitution Principle

```cpp
// If Bird has fly(), and Penguin is a Bird but can't fly —
// this violates LSP!

// GOOD design:
class Bird {
public:
    virtual void eat() = 0;
};
class FlyingBird : public Bird {
public:
    virtual void fly() = 0;
};
class Sparrow : public FlyingBird {
    void eat() override {}
    void fly() override {}
};
class Penguin : public Bird {
    void eat() override {}
    // no fly() — correct!
};
```

### I — Interface Segregation Principle

```cpp
// BAD: Fat interface forces unnecessary implementation
class IWorker {
public:
    virtual void work() = 0;
    virtual void eat() = 0;    // Robot doesn't eat!
};

// GOOD: Segregated interfaces
class IWorkable { virtual void work() = 0; };
class IEatable  { virtual void eat() = 0; };

class Human : public IWorkable, public IEatable {
    void work() override {}
    void eat() override {}
};
class Robot : public IWorkable {
    void work() override {}
    // No need to implement eat()
};
```

### D — Dependency Inversion Principle

```cpp
// BAD: High-level depends on low-level
class MySQLDatabase {
public:
    void save(string data) { /* MySQL specific */ }
};
class App {
    MySQLDatabase db;  // tightly coupled!
public:
    void saveData(string data) { db.save(data); }
};

// GOOD: Depend on abstraction
class IDatabase {
public:
    virtual void save(string data) = 0;
};
class MySQLDatabase : public IDatabase {
public:
    void save(string data) override { /* MySQL */ }
};
class MongoDatabase : public IDatabase {
public:
    void save(string data) override { /* MongoDB */ }
};
class App {
    IDatabase* db;  // depends on abstraction
public:
    App(IDatabase* d) : db(d) {}
    void saveData(string data) { db->save(data); }
};
// Can switch database without changing App!
```

---

## 12. Real-World OOP Design

### Example: Online Food Ordering System

```
Classes:
- User (customer details)
- Restaurant (restaurant info, menu)
- MenuItem (food item)
- Order (order details)
- Cart (items before ordering)
- Payment (payment processing)
- DeliveryAgent (delivery tracking)
```

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class MenuItem {
public:
    string name;
    double price;
    string category;

    MenuItem(string n, double p, string c) : name(n), price(p), category(c) {}

    void display() {
        cout << name << " - Rs." << price << " (" << category << ")" << endl;
    }
};

class Cart {
private:
    vector<MenuItem> items;

public:
    void addItem(MenuItem item) {
        items.push_back(item);
        cout << item.name << " added to cart" << endl;
    }

    double getTotal() {
        double total = 0;
        for (auto& item : items) total += item.price;
        return total;
    }

    void showCart() {
        cout << "\n=== Cart ===" << endl;
        for (auto& item : items) item.display();
        cout << "Total: Rs." << getTotal() << endl;
    }
};

class IPayment {
public:
    virtual bool processPayment(double amount) = 0;
    virtual string getPaymentMethod() = 0;
    virtual ~IPayment() {}
};

class CashPayment : public IPayment {
public:
    bool processPayment(double amount) override {
        cout << "Cash payment of Rs." << amount << " collected" << endl;
        return true;
    }
    string getPaymentMethod() override { return "Cash"; }
};

class OnlinePayment : public IPayment {
private:
    string upiId;
public:
    OnlinePayment(string id) : upiId(id) {}
    bool processPayment(double amount) override {
        cout << "Online payment of Rs." << amount << " via UPI: " << upiId << endl;
        return true;
    }
    string getPaymentMethod() override { return "UPI"; }
};

class Order {
private:
    static int orderCounter;
    int orderId;
    Cart cart;
    string status;
    IPayment* payment;

public:
    Order(Cart c, IPayment* p) : cart(c), payment(p), status("Pending") {
        orderId = ++orderCounter;
    }

    bool placeOrder() {
        double total = cart.getTotal();
        cart.showCart();
        if (payment->processPayment(total)) {
            status = "Confirmed";
            cout << "Order #" << orderId << " placed successfully!" << endl;
            return true;
        }
        return false;
    }

    string getStatus() { return status; }
};

int Order::orderCounter = 0;

int main() {
    MenuItem m1("Paneer Butter Masala", 250, "Main Course");
    MenuItem m2("Garlic Naan", 50, "Bread");
    MenuItem m3("Gulab Jamun", 80, "Dessert");

    Cart cart;
    cart.addItem(m1);
    cart.addItem(m2);
    cart.addItem(m3);

    IPayment* payment = new OnlinePayment("rahul@upi");
    Order order(cart, payment);
    order.placeOrder();

    cout << "Order Status: " << order.getStatus() << endl;

    delete payment;
    return 0;
}
```

---

## 13. Mini Tests

### Mini Test 1 (After Topics 1-4)

**Q1.** What are the 4 pillars of OOP?

**Q2.** What is the size of an empty class in C++?

**Q3.** What is the default access modifier in a class?

**Q4.** Difference between encapsulation and abstraction?

**Q5.** Output:
```cpp
class A {
    int x = 5;
public:
    int y = 10;
};
int main() {
    A obj;
    // cout << obj.x;  // line 1
    cout << obj.y;     // line 2
}
```

**Answers:**
1. Encapsulation, Abstraction, Inheritance, Polymorphism
2. 1 byte
3. private
4. Encapsulation = hiding data; Abstraction = hiding implementation complexity
5. Line 1 = Compile Error. Line 2 = Output: 10

---

### Mini Test 2 (After Topics 5-7)

**Q1.** What is a pure virtual function?

**Q2.** When are destructors called in reverse order?

**Q3.** What is the diamond problem?

**Q4.** Output:
```cpp
class A { public: A(){cout<<"A ";} ~A(){cout<<"~A ";} };
class B : public A { public: B(){cout<<"B ";} ~B(){cout<<"~B ";} };
int main() { B obj; }
```

**Q5.** What is shallow vs deep copy?

**Answers:**
1. Function declared with `= 0`, must be implemented by derived class
2. When objects go out of scope (local objects), destructors called in LIFO order
3. When a class inherits from two classes that share a common base — base included twice
4. `A B ~B ~A`
5. Shallow = copies pointer address (shared data). Deep = copies actual data (independent).

---

### Mini Test 3 (After Topics 8-10)

**Q1.** Difference between overloading and overriding?

**Q2.** Can constructors be virtual?

**Q3.** What is vtable?

**Q4.** Output:
```cpp
class Base {
public:
    void show() { cout << "Base "; }
    virtual void display() { cout << "Base "; }
};
class Derived : public Base {
public:
    void show() { cout << "Derived "; }
    void display() override { cout << "Derived "; }
};
int main() {
    Base* b = new Derived();
    b->show();
    b->display();
    delete b;
}
```

**Q5.** What is abstract class vs interface?

**Answers:**
1. Overloading = same class, different params. Overriding = different class, same signature + virtual.
2. No — vtable not ready when constructor runs.
3. vtable is a lookup table for virtual functions, used for runtime polymorphism.
4. `Base Derived`
5. Abstract = some implementation + pure virtual. Interface = ALL pure virtual.

---

## 14. Full Revision Notes

```
OOP QUICK REFERENCE
===================

PILLARS:
  Encapsulation  = Bundle data + methods, hide data (private)
  Abstraction    = Hide complexity (abstract class/interface)
  Inheritance    = Reuse parent class properties
  Polymorphism   = Same name, different behavior

ACCESS MODIFIERS:
  private    = Only inside class
  protected  = Inside class + derived classes
  public     = Everywhere

CONSTRUCTOR TYPES:
  Default         = No params: MyClass() {}
  Parameterized   = With params: MyClass(int x) {}
  Copy            = MyClass(const MyClass& obj) {}
  Destructor      = ~MyClass() {} (cleanup)

INHERITANCE TYPES:
  Single      : A -> B
  Multiple    : A, B -> C
  Multilevel  : A -> B -> C
  Hierarchical: A -> B, A -> C
  Diamond solved by: virtual inheritance

POLYMORPHISM:
  Compile-time: Function/Operator Overloading (static binding)
  Runtime: Virtual Functions (dynamic binding via vtable)

VIRTUAL:
  virtual void foo() {}      = can be overridden
  virtual void foo() = 0;    = MUST be overridden (pure virtual)
  virtual ~Base() {}         = needed for proper cleanup

ABSTRACT CLASS:
  - Has >= 1 pure virtual function
  - Cannot instantiate directly
  - Can have data + some implementation

INTERFACE (C++):
  - ALL pure virtual functions
  - No data members
  - Pure contract

OVERLOADING vs OVERRIDING:
  Overloading  = Same class, different params (compile-time)
  Overriding   = Base/Derived, same signature + virtual (runtime)

SOLID:
  S = Single Responsibility: one class, one job
  O = Open/Closed: extend without modifying
  L = Liskov: derived = replaceable for base
  I = Interface Segregation: many small interfaces
  D = Dependency Inversion: depend on abstraction

KEY OPERATORS:
  this -> current object pointer
  ::   -> scope resolution
  ->   -> pointer member access
  .    -> object member access

MEMORY:
  new  = heap allocation (manual delete needed)
  delete = free heap memory
  Smart pointers (C++11): unique_ptr, shared_ptr (auto cleanup)
```

---

## 15. Top 50 Interview Questions

### Basic (1–15)

1. What is OOP? Name its 4 pillars.
2. What is a class? What is an object?
3. What is the difference between class and struct in C++?
4. What is encapsulation? Give a real-world example.
5. What is abstraction? How is it different from encapsulation?
6. What is a constructor? How many types?
7. What is a destructor? When is it called?
8. What are access modifiers? Name them.
9. What is the default access specifier in class vs struct?
10. What is the size of an empty class?
11. What is `this` pointer?
12. What is a copy constructor? When is it called?
13. What is the difference between shallow copy and deep copy?
14. What is an initializer list?
15. What is a static data member?

### Intermediate (16–30)

16. What is inheritance? Types?
17. What is the diamond problem? How to solve it?
18. What is virtual inheritance?
19. What is polymorphism? Types?
20. What is a virtual function?
21. What is a pure virtual function?
22. What is an abstract class?
23. What is vtable and vptr?
24. What is function overloading?
25. What is function overriding?
26. What is operator overloading?
27. Can constructor be virtual? Why not?
28. Why do we need virtual destructor?
29. What is the `override` keyword?
30. What is the `final` keyword?

### Advanced (31–45)

31. What is the Liskov Substitution Principle?
32. What is the difference between static and dynamic binding?
33. What is a friend function?
34. What is a friend class?
35. Can we overload `=` operator?
36. What is covariant return type?
37. What is method hiding?
38. What is SOLID? Explain each letter briefly.
39. What is Dependency Injection?
40. What is composition vs inheritance?
41. When to prefer composition over inheritance?
42. What is the Rule of Three in C++?
43. What is the Rule of Five in C++11?
44. What is move constructor?
45. What is smart pointer? Types?

### Output/Tricky (46–50)

46. Base pointer to derived object — which function called for virtual vs non-virtual?
47. Order of constructor/destructor in multiple inheritance?
48. What happens when derived class doesn't implement ALL pure virtual functions?
49. Can abstract class have a constructor?
50. What is object slicing?

---

### Answers to Key Tricky Questions

**Q28: Why virtual destructor?**
```cpp
class Base { ~Base() {} };         // Non-virtual
class Derived : public Base { ... };
Base* b = new Derived();
delete b;  // Only Base destructor called! Derived leaked!

// FIX:
class Base { virtual ~Base() {} }; // Virtual destructor
delete b;  // Both Derived then Base destructors called. Correct!
```

**Q42: Rule of Three**
> If a class needs a custom: (1) destructor, (2) copy constructor, or (3) copy assignment operator — it likely needs all three. (Because manual memory management is involved.)

**Q50: Object Slicing**
```cpp
class Base { int x; };
class Derived : public Base { int y; }; // extra member

Derived d;
Base b = d;  // SLICED! 'y' is lost, only Base part copied
// Use pointers/references to avoid slicing
```

---

## 16. Coding Interview Problems

### Problem 1: Design a Stack using OOP

```cpp
#include <iostream>
using namespace std;

class Stack {
private:
    int* data;
    int top;
    int capacity;

public:
    Stack(int cap) : capacity(cap), top(-1) {
        data = new int[capacity];
    }

    ~Stack() {
        delete[] data;
    }

    void push(int val) {
        if (top == capacity - 1) {
            cout << "Stack overflow!" << endl;
            return;
        }
        data[++top] = val;
    }

    int pop() {
        if (top == -1) {
            cout << "Stack underflow!" << endl;
            return -1;
        }
        return data[top--];
    }

    int peek() {
        if (top == -1) return -1;
        return data[top];
    }

    bool isEmpty() { return top == -1; }
    bool isFull() { return top == capacity - 1; }
    int size() { return top + 1; }
};

int main() {
    Stack s(5);
    s.push(10);
    s.push(20);
    s.push(30);
    cout << "Top: " << s.peek() << endl;    // 30
    cout << "Popped: " << s.pop() << endl;  // 30
    cout << "Size: " << s.size() << endl;   // 2
}
```

### Problem 2: Shape Hierarchy (Classic OOP)

```cpp
#include <iostream>
#include <cmath>
using namespace std;

class Shape {
protected:
    string color;
public:
    Shape(string c = "black") : color(c) {}
    virtual double area() = 0;
    virtual double perimeter() = 0;
    virtual void display() {
        cout << "Color: " << color
             << ", Area: " << area()
             << ", Perimeter: " << perimeter() << endl;
    }
    virtual ~Shape() {}
};

class Circle : public Shape {
    double radius;
public:
    Circle(double r, string c = "black") : Shape(c), radius(r) {}
    double area() override { return M_PI * radius * radius; }
    double perimeter() override { return 2 * M_PI * radius; }
};

class Rectangle : public Shape {
    double l, w;
public:
    Rectangle(double l, double w, string c = "black") : Shape(c), l(l), w(w) {}
    double area() override { return l * w; }
    double perimeter() override { return 2 * (l + w); }
};

class Triangle : public Shape {
    double a, b, c;
public:
    Triangle(double a, double b, double c) : a(a), b(b), c(c) {}
    double perimeter() override { return a + b + c; }
    double area() override {
        double s = perimeter() / 2;
        return sqrt(s * (s-a) * (s-b) * (s-c));
    }
};

int main() {
    Shape* shapes[] = {
        new Circle(5, "red"),
        new Rectangle(4, 6, "blue"),
        new Triangle(3, 4, 5)
    };

    for (auto* s : shapes) {
        s->display();
        delete s;
    }
}
```

### Problem 3: Singleton Pattern (Interview Favorite)

```cpp
class DatabaseConnection {
private:
    static DatabaseConnection* instance;
    string connectionString;

    DatabaseConnection(string cs) : connectionString(cs) {
        cout << "Database connected!" << endl;
    }

public:
    static DatabaseConnection* getInstance(string cs = "") {
        if (instance == nullptr) {
            instance = new DatabaseConnection(cs);
        }
        return instance;
    }

    void query(string sql) {
        cout << "Executing: " << sql << endl;
    }

    DatabaseConnection(const DatabaseConnection&) = delete;
    DatabaseConnection& operator=(const DatabaseConnection&) = delete;
};

DatabaseConnection* DatabaseConnection::instance = nullptr;

int main() {
    DatabaseConnection* db1 = DatabaseConnection::getInstance("localhost/mydb");
    DatabaseConnection* db2 = DatabaseConnection::getInstance();

    cout << (db1 == db2 ? "Same instance!" : "Different!") << endl; // Same!

    db1->query("SELECT * FROM users");
}
```

### Problem 4: Observer Pattern (Real-World)

```cpp
#include <iostream>
#include <vector>
using namespace std;

class Observer {
public:
    virtual void update(string event) = 0;
    virtual ~Observer() {}
};

class Subject {
    vector<Observer*> observers;
public:
    void subscribe(Observer* obs) {
        observers.push_back(obs);
    }
    void notify(string event) {
        for (auto obs : observers) obs->update(event);
    }
};

class StockMarket : public Subject {
    string stockName;
    double price;
public:
    StockMarket(string name, double p) : stockName(name), price(p) {}
    void setPrice(double newPrice) {
        price = newPrice;
        cout << stockName << " price changed to " << price << endl;
        notify(stockName + ":" + to_string(price));
    }
};

class Investor : public Observer {
    string name;
public:
    Investor(string n) : name(n) {}
    void update(string event) override {
        cout << name << " notified: " << event << endl;
    }
};

int main() {
    StockMarket reliance("Reliance", 2500);
    Investor i1("Rahul"), i2("Priya");

    reliance.subscribe(&i1);
    reliance.subscribe(&i2);

    reliance.setPrice(2600);
    reliance.setPrice(2450);
}
```

### Problem 5: Common Output Tricky Questions

```cpp
// Q1: Virtual destructor
class A {
public:
    ~A() { cout << "~A "; }
};
class B : public A {
public:
    ~B() { cout << "~B "; }
};
int main() {
    A* obj = new B();
    delete obj;
}
// Without virtual: Output: ~A   (BUG! B's destructor not called)
// With virtual ~A(): Output: ~B ~A (CORRECT)

// Q2: Static member
class Counter {
public:
    static int count;
    Counter() { ++count; }
    ~Counter() { --count; }
};
int Counter::count = 0;
int main() {
    Counter c1, c2, c3;
    cout << Counter::count; // 3
    { Counter c4; cout << Counter::count; } // 4, then destructor -> 3
    cout << Counter::count; // 3
}
// Output: 3 4 3

// Q3: Copy constructor
class Test {
public:
    Test() { cout << "Default "; }
    Test(const Test& t) { cout << "Copy "; }
};
void func(Test t) { }  // passed by value
int main() {
    Test t1;
    func(t1);
}
// Output: Default Copy
```

---

## Final Tips for Interviews

```
BEFORE INTERVIEW:
  1. Practice writing classes from scratch without IDE
  2. Know output of vtable, constructor order, copy constructor
  3. Be clear on: virtual vs non-virtual, overload vs override
  4. Practice SOLID with examples

DURING INTERVIEW:
  1. Think aloud — explain your design
  2. Mention trade-offs (composition vs inheritance)
  3. Ask clarifying questions
  4. Write clean, readable code

COMMON MISTAKES TO AVOID:
  1. Forgetting virtual destructor in base class
  2. Shallow copy when deep copy needed
  3. Not initializing static members outside class
  4. Confusing overloading with overriding
  5. Not handling the diamond problem
  6. Creating abstract class objects directly

GOLDEN RULES:
  - Encapsulate always (data private, methods public)
  - Prefer composition over inheritance when possible
  - Program to interface, not implementation
  - Keep classes small and focused (SRP)
  - Always virtual destructor for polymorphic base classes
```

---

*This guide covers OOP in C++ comprehensively — from zero to interview-ready.*
*Har concept ko code mein likhke practice karo. Tab hi interview mein confidence aayega.*

---

**Author's Note:** The best way to learn OOP is to design something real. Try designing a:
- Library Management System
- Parking Lot System
- Chess Game
- Hospital Management System

These are classic OOP design interview questions!
