# Python Decorators: A Quick Tutorial

#### **What Are Decorators?**

Decorators in Python are functions that modify the behavior of another function or method **without modifying its actual code**. They are commonly used for **logging, authentication, access control, caching, and more**.

A decorator is applied using the `@decorator_name` syntax.

---

## **1. Basic Decorator Example**

A simple decorator that prints something before and after a function runs:

```python
def my_decorator(func):
    def wrapper():
        print("Something before the function runs")
        func()
        print("Something after the function runs")
    return wrapper

@my_decorator
def say_hello():
    print("Hello, world!")

say_hello()
```

### **Output**

```
Something before the function runs
Hello, world!
Something after the function runs
```

---

## **2. Decorators With Arguments**

If the decorated function takes arguments, the wrapper function should also accept them.

```python
def repeat_decorator(func):
    def wrapper(*args, **kwargs):
        print("Repeating function call:")
        func(*args, **kwargs)
        func(*args, **kwargs)
    return wrapper

@repeat_decorator
def greet(name):
    print(f"Hello, {name}!")

greet("Adge")
```

### **Output**

```
Repeating function call:
Hello, Adge!
Hello, Adge!
```

---

## **3. Returning Values from Decorated Functions**

Make sure the wrapper returns the original function’s result.

```python
def double_result(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result * 2
    return wrapper

@double_result
def add(x, y):
    return x + y

print(add(3, 4))  # Output: 14
```

---

## **4. Using `functools.wraps` to Preserve Metadata**

Using `functools.wraps` ensures the decorated function retains its name and docstring.

```python
import functools

def log_function(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with {args} {kwargs}")
        return func(*args, **kwargs)
    return wrapper

@log_function
def multiply(a, b):
    """Multiplies two numbers."""
    return a * b

print(multiply(3, 5))
print(multiply.__name__)  # Output: multiply (instead of wrapper)
print(multiply.__doc__)   # Output: Multiplies two numbers.
```

---

## **5. Decorators with Arguments**

If a decorator itself needs parameters, wrap it in another function.

```python
import functools

def repeat(n):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)  # Runs the function 3 times
def say_hi():
    print("Hi!")

say_hi()
```

### **Output**

```
Hi!
Hi!
Hi!
```

---

## **6. Applying Multiple Decorators**

Decorators are applied from **top to bottom**.

```python
def uppercase(func):
    def wrapper():
        return func().upper()
    return wrapper

def exclaim(func):
    def wrapper():
        return func() + "!!!"
    return wrapper

@exclaim
@uppercase
def greet():
    return "hello"

print(greet())  # Output: HELLO!!!
```

---

## **7. Class-Based Decorators**

You can also use a class as a decorator by defining `__call__`:

```python
class CountCalls:
    def __init__(self, func):
        self.func = func
        self.count = 0

    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call {self.count} to {self.func.__name__}")
        return self.func(*args, **kwargs)

@CountCalls
def say_hello():
    print("Hello!")

say_hello()
say_hello()
```

### **Output**

```
Call 1 to say_hello
Hello!
Call 2 to say_hello
Hello!
```

---

## **Conclusion**

- Decorators **wrap functions** to modify their behavior.
- Use `@decorator_name` before a function to apply a decorator.
- Use `functools.wraps` to preserve metadata.
- You can **pass arguments** to decorators by wrapping them in another function.
- Decorators **stack from top to bottom**.
- You can implement decorators using **functions** or **classes**.

Want to dive deeper into **real-world use cases** like caching, authentication, or logging? Let me know! 🚀