# Object Oriented Programming Python | Docs With Examples

Object-Oriented Programming (OOP) is a programming paradigm that organizes code into objects, which bundle data and behavior. Python supports OOP with classes and objects.

## What is a Class?

A **class** is a blueprint for creating objects in [Python](https://hackr.io/blog/what-is-python). It defines attributes (data) and methods (functions) that operate on that data.

```python
class Car:
    def __init__(self, brand, model):
        self.brand = brand  # Attribute
        self.model = model  # Attribute
    
    def display_info(self):
        print(f"Car: {self.brand} {self.model}")
```

## Creating an Object

An **object** is an instance of a class.

```python
my_car = Car("Toyota", "Corolla")
my_car.display_info()
```

**Output:**

```python
Car: Toyota Corolla
```

## The `__init__` Method (Constructor)

The `__init__` method initializes the attributes when an object is created - this is a _dunder_ method.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

### Instance vs. Class Attributes

- **Instance attributes** belong to specific objects.
- **Class attributes** are shared across all instances.

```python
class Dog:
    species = "Canine"  # Class attribute
    
    def __init__(self, name):
        self.name = name  # Instance attribute
```

```python
dog1 = Dog("Buddy")
dog2 = Dog("Max")
print(dog1.species)  # Canine
print(dog2.name)  # Max
```

## Inheritance (Reusing Other Classes)

Inheritance allows one class (the child class) to inherit attributes and methods from another class (the parent class). This avoids repetition and allows for code reuse.

```python
class ElectricCar(Car):
    def __init__(self, brand, model, battery_size):
        super().__init__(brand, model)  # Inherit attributes from Car
        self.battery_size = battery_size  # New attribute for ElectricCar
    
    def display_battery(self):
        print(f"Battery size: {self.battery_size} kWh")
```

```python
ev = ElectricCar("Tesla", "Model S", 100)
ev.display_info()
ev.display_battery()
```

**Explanation:**

- `ElectricCar` inherits the attributes (`brand`, `model`) and method (`display_info`) from `Car`.
    
- It also adds a new method `display_battery()`.
    
- This reduces code duplication, making programs more maintainable.
    

## Encapsulation (Data Hiding)

Encapsulation is the practice of restricting access to certain parts of an object. This helps protect data and prevents unintended modifications.

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Private attribute
    
    def get_balance(self):
        return self.__balance
```

```python
account = BankAccount(1000)
print(account.get_balance())  # 1000
```

**Explanation:**

- `__balance` is a private attribute (denoted by `__` before the name), meaning it cannot be accessed directly.
    
- Instead, we use the method `get_balance()` to safely retrieve the balance.
    
- This ensures better control over data access and modification.
    

## Polymorphism

Polymorphism allows different classes to define methods with the same name but different implementations. This enables flexibility and makes code more reusable.

```python
class Cat:
    def speak(self):
        return "Meow"

class Dog:
    def speak(self):
        return "Woof"

animals = [Cat(), Dog()]
for animal in animals:
    print(animal.speak())
```

**Output:**

```python
Meow
Woof
```

**Explanation:**

- Both `Cat` and `Dog` have a `speak()` method, but they behave differently.
    
- This allows us to use a common interface (calling `speak()`) regardless of the object type.
    
- Polymorphism helps make code more flexible and adaptable to different use cases.
    

## Key Takeaways

- OOP organizes code into **classes and objects** in your [Python projects](https://hackr.io/blog/python-projects).
    
- **Inheritance** allows one class to reuse the attributes and methods of another class.
    
- **Encapsulation** protects object data and controls how it's accessed.
    
- **Polymorphism** enables different classes to share method names with different implementations.
    

## Practice Exercise

Here's a simple challenge, open up your [Python editor](https://app.hackr.io/editors/python) and try to write a class `Rectangle` with methods to calculate area and perimeter:

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
```

## Wrapping Up

Object-Oriented Programming helps structure code efficiently. Understanding classes, objects, inheritance, encapsulation, and polymorphism allows you to write scalable and maintainable Python programs. By mastering these concepts, you can build more robust and reusable software. Happy coding!