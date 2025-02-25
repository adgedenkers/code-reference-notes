# Python Lambda Functions with Examples

Python’s lambda functions, also known as anonymous functions, are a compact way to write small, single-use functions.

They’re useful for quick operations, especially when combined with other [Python](https://hackr.io/blog/what-is-python) features like map(), filter(), and sort().

## Basic Syntax

A lambda function is defined using the lambda keyword:

```python
lambda arguments: expression
```

Let's break down the rest of this syntax:

- **arguments:** A comma-separated list of parameters.  
- **expression:** A single expression that is evaluated and returned by the function.

## Common Examples

**1. Simple Lambda Function**

This is a simple lambda function to add 10 to a number. Note we have assigned the function to a variable, and passed in a parameter much like we would with a normal function:

```python
add_10 = lambda x: x + 10
print(add_10(5))
```

Output:

```python
15
```

**2. Lambda with Multiple Arguments**

This is a simple lambda function to multiply two numbers:

```python
multiply = lambda x, y: x * y
print(multiply(3, 4))
```

Output:

```python
12
```

## Common Use Cases

**1. Using Lambda with map()**

One of the most common ways to use a lambda function is with the map() function, as this applies a lambda to each item in an iterable:

```python
numbers = [1, 2, 3, 4]
squared = map(lambda x: x**2, numbers)
print(list(squared))
```

Output:

```python
[1, 4, 9, 16]
```

**2. Using Lambda with filter()**

This is another very common use case for the lambda function, as by combining with the filter() function, we can select items from an iterable based on a condition:

```python
numbers = [1, 2, 3, 4, 5, 6]
even_numbers = filter(lambda x: x % 2 == 0, numbers)
print(list(even_numbers))
```

Output:

```python
[2, 4, 6]
```

**3. Using Lambda with sort()**

Lambda functions are also hugely useful for defining unique keys for the sort() function. In this example, a list of tuples is sorted by the second element:

```python
pairs = [(1, 3), (2, 2), (4, 1)]
pairs.sort(key=lambda x: x[1])
print(pairs)
```

Output:

```python
[(4, 1), (2, 2), (1, 3)]
```

**4. Inline Functionality**

And of course, lambdas are great for quick, throwaway operations:

```python
print((lambda x, y: x + y)(5, 7))
```

Output:

```python
12
```

## Limitations of Lambda Functions

- **Single Expression:** Lambdas can only contain one expression, which limits their complexity versus a named function.  
- **Readability:** Overusing lambdas can make code harder to read, so use them wisely and appropriately.  
- **No Name:** Since lambdas are anonymous, debugging can be tricky.

## Key Takeaways

- My advice is to use lambda functions for simple, one-off tasks in your [Python projects](https://hackr.io/blog/python-projects).  
- Some of the best ideas are to combine them with map(), filter(), and sort() for powerful data transformations.  
- I think you should avoid using lambdas for complex logic—stick to named functions for better readability.

## Practice Exercise

Here's a simple challenge you can attempt, try writing a lambda function that takes a list of numbers and returns only those divisible by 3:

```python
numbers = [3, 5, 9, 12, 15, 20]
divisible_by_3 = filter(lambda x: x % 3 == 0, numbers)
print(list(divisible_by_3))
```

## Wrapping Up

Lambda functions are a handy tool for concise and efficient operations. By understanding their syntax and use cases, you can simplify your code and make it more Pythonic. Happy coding!