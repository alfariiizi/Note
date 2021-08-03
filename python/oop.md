# OOP in Python

Taken note from: [David Amos, _Real Python_](https://realpython.com/python3-object-oriented-programming/)

## Classes vs Instances

Class
:   template/blueprint/cetakan nya.

Instance
:   object yang terbuat dari suatu class.

> Kalo di C++, Instance biasanya cukup itu disebut _object_.

## How to define class?

```python
class Dog:
    # species is Class Attributes
    species = "Canis familiaris"

    # __init__ is a Constructor
    def __init__(self, name, age):
        # self.name and self.age are Instance Attributes
        self.name = name
        self.age = age

    # description() and speak() are Instance Method
    def description(self):
        return f"{self.name} is {self.age} years old"

    def speak(self, sound):
        return f"{self.name} says {sound}"

    # __str__() is special method that make `print()` function will print this
    def __str__(self):
        # return description()
        # Code above is the same as code below
        return f"{self.name} is {self.age} years old"

a = Dog('Bulldog', 5)
b = Dog('Bulldog', 5)
print( a == b ) # output: False
# They represent different two distinct objects in memory.
```

> `.__init__()` and `.__str__()` are called **Dunder Methods**[^dunder-methods], because they *begin* and *end* with double underscores (`__`).

## Inheritance

```python
class Dog:
    species = "Canis familiaris"

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"{self.name} is {self.age} years old"

    def speak(self, sound):
        return f"{self.name} barks {sound}"

    def description(self):
        return f"{self.name} is {self.age} years old"

class JackRussellTerrier(Dog):
    def speak(self, sound="Arf"):
        return f"{self.name} says {sound}"
    
    def description(self):
        # super() make this (description()) method can access parent class method
        return super().description()

miles = JackRussellTerrier('Miles', 4)

print(isinstance(miles, Dog))   # output: True

```

[^dunder-methods]: For further read about Dunder Methods, see this site <https://www.section.io/engineering-education/dunder-methods-python/>
