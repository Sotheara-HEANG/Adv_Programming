# TP01 — Python Foundations and Clean Coding

## Objective

The objective of TP01 is to review fundamental Python programming concepts and learn how to write code that is clean, readable, well-organized, and easy to maintain.

By the end of this TP, I should understand the following topics:

- Variables
- Functions
- Classes
- Objects
- Methods
- Constants
- Modules
- Packages
- Clean code principles
- PEP 8 coding style
- Basic debugging
- Exception handling

---

# Part 1 — Basic Python Concepts

## 1. Variable

A **variable** is a name used to store a value that can be accessed and used in a program.

**Example:**

```python
student_name = "Student A"
age = 20
score = 85
```

**Key idea:**

```text
Variable → stores data
```

---

## 2. Function

A **function** is a reusable block of code designed to perform a specific task.

**Example:**

```python
def add(a, b):
    return a + b


result = add(5, 3)
print(result)
```

**Output:**

```text
8
```

**Key idea:**

```text
Function → reusable code that performs a task
```

---

## 3. Class

A **class** is a blueprint for creating objects. It defines the structure and behavior that objects created from it can have.

**Example:**

```python
class Student:
    pass
```

An **object** can be created from this class:

```python
student1 = Student()
```

**Key ideas:**

```text
Class → blueprint
Object → instance created from the class
```

---

## 4. Method

A **method** is a function defined inside a class. It typically describes an action that an object can perform.

**Example:**

```python
class Student:
    def study(self):
        print("The student is studying.")


student1 = Student()
student1.study()
```

**Key idea:**

```text
Method → function inside a class
```

---

## 5. Constant

A **constant** is a value intended to remain unchanged during program execution.

**Example:**

```python
PI = 3.14159
MAX_SCORE = 100
```

By convention, Python uses uppercase names for constants. This is a naming convention rather than a language-enforced restriction.

**Key idea:**

```text
Constant → fixed value
```

---

## 6. Module

A **module** is a Python file containing code that can be reused in other Python files.

**Example file:**

```text
calculator.py
```

**Inside `calculator.py`:**

```python
def add(a, b):
    return a + b
```

**Using the module from another file:**

```python
import calculator

print(calculator.add(5, 2))
```

**Key idea:**

```text
Module → one Python file
```

---

## 7. Package

A **package** organizes related Python modules into a directory, making larger projects easier to structure and manage.

**Example:**

```text
data_tools/
│
├── __init__.py
├── cleaning.py
├── analysis.py
└── visualization.py
```

**Key idea:**

```text
Package → collection of related modules
```

---

## Basic Concept Summary

| Concept | Meaning |
|:--|:--|
| Variable | Stores data |
| Function | Performs a reusable task |
| Class | Blueprint for creating objects |
| Object | Instance of a class |
| Method | Function inside a class |
| Constant | Value intended not to change |
| Module | Python file containing reusable code |
| Package | Folder containing related modules |

---

# Part 2 — Clean Code

**Clean code** is code that is easy to read, understand, modify, debug, and maintain. Two important qualities of clean code are readability and maintainability.

## 1. Readability

Meaningful variable names make code easier to understand.

**Less readable example:**

```python
a = 10
b = 5
c = a * b
```

**More readable example:**

```python
price = 10
quantity = 5
total_price = price * quantity
```

The second example is better because the variable names clearly communicate what each value represents and what the calculation does.

---

## 2. Maintainability

Reusable functions reduce repetition and make code easier to update.

**Repeated code:**

```python
total1 = 10 * 2
total2 = 20 * 3
```

**Improved version using a function:**

```python
def calculate_total(price, quantity):
    return price * quantity


total1 = calculate_total(10, 2)
total2 = calculate_total(20, 3)
```

If the calculation needs to change, it can be updated in one function rather than in multiple places.

---

# Part 3 — PEP 8 Coding Style

**PEP 8** is the official Python style guide. It provides conventions that help programmers write consistent, readable code.

**Important conventions:**

| Item | Recommended style |
|:--|:--|
| Variables | `snake_case` |
| Functions | `snake_case` |
| Classes | `PascalCase` |
| Constants | `UPPERCASE` |
| Indentation | 4 spaces |

**Example:**

```python
student_name = "Student A"
MAX_SCORE = 100


def calculate_average():
    pass


class StudentProfile:
    pass
```

---

# Part 4 — Exception Handling

**Exception handling** allows a program to respond to errors in a controlled way instead of terminating unexpectedly.

**Example:**

```python
try:
    number = int(input("Enter a number: "))
    print(100 / number)

except ValueError:
    print("Please enter a valid integer.")

except ZeroDivisionError:
    print("The number cannot be zero.")
```

In this example, `ValueError` handles invalid integer input, while `ZeroDivisionError` handles division by zero.

**Important keywords:**

```text
try
except
else
finally
raise
```

- `try` contains code that may raise an exception.
- `except` handles a matching exception.
- `else` runs when no exception is raised in the `try` block.
- `finally` runs during cleanup, whether or not an exception occurs.
- `raise` explicitly raises an exception.

---

# Part 5 — Debugging

**Debugging** is the process of finding, understanding, and fixing problems in a program.

**A simple debugging process:**

```text
Read the error
      ↓
Find where it happened
      ↓
Understand the problem
      ↓
Check the values
      ↓
Fix the code
      ↓
Run again
```

**Common Python errors include:**

- `SyntaxError`
- `TypeError`
- `ValueError`
- `NameError`
- `IndexError`
- `KeyError`
- `FileNotFoundError`

---

# What I Learned

After completing TP01, I understand that good programming is not only about making code work. It is also about writing code that other people, including my future self, can read and maintain.

I should be able to:

- Use meaningful variable names.
- Organize repeated code into functions.
- Use classes and objects when appropriate.
- Separate code into modules and packages.
- Follow PEP 8 coding style.
- Handle errors using `try-except`.
- Debug problems step by step.
- Write code that is easier to understand and maintain.

---

# TP01 Summary

```text
Python Foundations
        ↓
Variables
        ↓
Functions
        ↓
Classes & Objects
        ↓
Methods
        ↓
Constants
        ↓
Modules & Packages
        ↓
Clean Code
        ↓
PEP 8
        ↓
Exception Handling
        ↓
Debugging
```

> **Main takeaway:** The goal of TP01 is to build a strong foundation in Python and develop good habits for writing clean, readable, and maintainable code.
