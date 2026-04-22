# Topic 2: Object-Oriented Programming in Python

## 1. Overview

Object-Oriented Programming (OOP) is a paradigm that organizes code into **objects** — instances of **classes** that bundle data (attributes) and behavior (methods) together. Python fully supports OOP with classes, inheritance, polymorphism, encapsulation, and abstraction.

---

## 2. Key Concepts

### 2.1 Classes and Objects

A **class** is a blueprint; an **object** is an instance of that class.

```python
class Student:
    pass

s1 = Student()   # s1 is an object of class Student
s2 = Student()   # s2 is another object
print(type(s1))  # <class '__main__.Student'>
```

### 2.2 The `__init__` Constructor

The `__init__` method is the **constructor** — it initializes object attributes when an object is created.

```python
class Student:
    def __init__(self, name, roll):
        self.name = name    # instance attribute
        self.roll = roll

s = Student("Alice", 101)
print(s.name)  # Alice
print(s.roll)  # 101
```

- `self` refers to the current instance of the class.
- `self` must be the first parameter in every instance method.

### 2.3 Instance Methods

```python
class Student:
    def __init__(self, name, roll):
        self.name = name
        self.roll = roll
        self.marks = []

    def add_marks(self, mark):
        self.marks.append(mark)

    def display_info(self):
        print(f"Name: {self.name}, Roll: {self.roll}")
        print(f"Marks: {self.marks}")
        if self.marks:
            print(f"Average: {sum(self.marks)/len(self.marks):.2f}")

# Usage
s = Student("Alice", 101)
s.add_marks(85)
s.add_marks(90)
s.add_marks(78)
s.display_info()
# Name: Alice, Roll: 101
# Marks: [85, 90, 78]
# Average: 84.33
```

### 2.4 Encapsulation

**Encapsulation** means protecting data inside the class and controlling access to it.

- **Public** attributes: Accessible anywhere — `self.name`
- **Protected** attributes (convention): Prefix `_` — `self._name`
- **Private** attributes: Prefix `__` — `self.__name` (name mangled to `_ClassName__name`)

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner          # public
        self.__balance = balance    # private

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount

    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
        else:
            print("Insufficient funds!")

    def get_balance(self):
        return self.__balance

acc = BankAccount("Alice", 1000)
acc.deposit(500)
print(acc.get_balance())  # 1500
# print(acc.__balance)    # AttributeError!
print(acc._BankAccount__balance)  # 1500 (name mangling — not recommended)
```

### 2.5 Inheritance

A **child class** inherits attributes and methods from a **parent class**.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return f"{self.name} makes a sound."

class Dog(Animal):
    def speak(self):
        return f"{self.name} says Woof!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says Meow!"

d = Dog("Rex")
c = Cat("Whiskers")
print(d.speak())  # Rex says Woof!
print(c.speak())  # Whiskers says Meow!
```

**Types of Inheritance:**
| Type | Description |
|------|-------------|
| Single | One child, one parent |
| Multiple | One child, multiple parents |
| Multilevel | Child → Parent → Grandparent |
| Hierarchical | Multiple children, one parent |

**Multiple Inheritance:**
```python
class A:
    def method(self):
        print("A")

class B:
    def method(self):
        print("B")

class C(A, B):
    pass

obj = C()
obj.method()  # A  (follows MRO: Method Resolution Order — left to right)
print(C.__mro__)
```

**`super()` — calling parent class methods:**
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Student(Person):
    def __init__(self, name, age, roll):
        super().__init__(name, age)  # call parent constructor
        self.roll = roll

s = Student("Alice", 20, 101)
print(s.name, s.age, s.roll)  # Alice 20 101
```

### 2.6 Polymorphism

**Polymorphism** = "many forms". Same interface, different behavior.

```python
class Shape:
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    def area(self):
        return 3.14159 * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    def area(self):
        return self.width * self.height

# Polymorphic behavior
shapes = [Circle(5), Rectangle(4, 6)]
for s in shapes:
    print(f"Area: {s.area()}")
# Area: 78.53975
# Area: 24
```

**Operator Overloading:**
```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)  # Vector(4, 6)
```

### 2.7 Abstraction

**Abstraction** hides complex implementation details and shows only necessary features.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    @abstractmethod
    def perimeter(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2

    def perimeter(self):
        return 2 * 3.14159 * self.radius

# shape = Shape()  # TypeError: Can't instantiate abstract class
c = Circle(5)
print(c.area())       # 78.53975
print(c.perimeter())  # 31.4159
```

### 2.8 Special (Dunder) Methods

| Method | Purpose | Invoked by |
|--------|---------|------------|
| `__init__` | Constructor | `ClassName()` |
| `__str__` | Human-readable string | `print(obj)`, `str(obj)` |
| `__repr__` | Developer-friendly string | `repr(obj)` |
| `__len__` | Length | `len(obj)` |
| `__add__` | Addition | `obj1 + obj2` |
| `__eq__` | Equality | `obj1 == obj2` |
| `__lt__` | Less than | `obj1 < obj2` |
| `__getitem__` | Indexing | `obj[key]` |

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def __str__(self):
        return f"Student({self.name}, avg={sum(self.marks)/len(self.marks):.1f})"

    def __lt__(self, other):
        return sum(self.marks) < sum(other.marks)

s1 = Student("Alice", [85, 90])
s2 = Student("Bob", [70, 80])
print(s1)          # Student(Alice, avg=87.5)
print(s1 < s2)     # False
```

---

## 3. MCQs (15 Questions)

**Q1.** What is the purpose of `self` in Python class methods?
- A) It refers to the class itself
- B) It refers to the current instance of the class
- C) It is a keyword for static methods
- D) It is optional and can be removed

**Answer:** B) It refers to the current instance of the class

---

**Q2.** Which method is automatically called when an object is created?
- A) `__new__`
- B) `__create__`
- C) `__init__`
- D) `__start__`

**Answer:** C) `__init__`

---

**Q3.** What is the output?
```python
class A:
    x = 10
a = A()
a.x = 20
print(A.x, a.x)
```
- A) `20 20`
- B) `10 20`
- C) `10 10`
- D) Error

**Answer:** B) `10 20` — `a.x = 20` creates an instance attribute that shadows the class attribute.

---

**Q4.** Which of the following achieves encapsulation in Python?
- A) Using `__` prefix for attributes
- B) Using `public` keyword
- C) Using `protected` keyword
- D) Using `final` keyword

**Answer:** A) Using `__` prefix for attributes

---

**Q5.** What is the output?
```python
class Parent:
    def show(self):
        print("Parent")
class Child(Parent):
    def show(self):
        print("Child")
obj = Child()
obj.show()
```
- A) Parent
- B) Child
- C) ParentChild
- D) Error

**Answer:** B) Child — Method overriding.

---

**Q6.** Which keyword is used to inherit from a parent class?
- A) `extends`
- B) `inherits`
- C) Class name in parentheses: `class Child(Parent)`
- D) `implements`

**Answer:** C) Class name in parentheses: `class Child(Parent)`

---

**Q7.** What does `super()` do?
- A) Creates a new superclass
- B) Calls a method from the parent class
- C) Makes a class abstract
- D) Deletes the parent object

**Answer:** B) Calls a method from the parent class

---

**Q8.** Which module provides abstract base classes in Python?
- A) `abstract`
- B) `abc`
- C) `interface`
- D) `base`

**Answer:** B) `abc`

---

**Q9.** Can you instantiate an abstract class directly?
- A) Yes
- B) No — it raises `TypeError`
- C) Only if it has no abstract methods
- D) B and C both

**Answer:** D) B and C both — A class with all abstract methods implemented is no longer abstract.

---

**Q10.** What is the MRO in Python?
- A) Memory Reference Order
- B) Method Resolution Order
- C) Module Registration Order
- D) Method Return Order

**Answer:** B) Method Resolution Order — determines which method to call in multiple inheritance.

---

**Q11.** What is the output of `type(type)`?
- A) `<class 'type'>`
- B) `<class 'object'>`
- C) `type`
- D) Error

**Answer:** A) `<class 'type'>`

---

**Q12.** Which dunder method is invoked by `print(obj)`?
- A) `__repr__`
- B) `__print__`
- C) `__str__`
- D) `__display__`

**Answer:** C) `__str__` (falls back to `__repr__` if `__str__` is not defined)

---

**Q13.** What is **polymorphism**?
- A) A class having multiple constructors
- B) Same method name behaving differently in different classes
- C) A class inheriting from multiple parents
- D) Hiding data from external access

**Answer:** B) Same method name behaving differently in different classes

---

**Q14.** What is the output?
```python
class A:
    def __init__(self):
        self.__x = 5
a = A()
print(a._A__x)
```
- A) Error
- B) 5
- C) None
- D) `__x`

**Answer:** B) 5 — Python name mangles `__x` to `_A__x`.

---

**Q15.** Which of the following is NOT a pillar of OOP?
- A) Encapsulation
- B) Inheritance
- C) Compilation
- D) Polymorphism

**Answer:** C) Compilation — The four pillars are Encapsulation, Inheritance, Polymorphism, and Abstraction.
