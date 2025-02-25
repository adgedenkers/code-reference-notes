# Python List Comprehensions | Docs With Examples

Python list comprehensions are a concise and elegant way to create lists.

They allow you to generate, filter, and modify lists with minimal syntax, making your [Python](https://hackr.io/blog/what-is-python) code more readable, efficient, and Pythonic.

## Basic Syntax

The syntax for a Python list comprehension is relatively straightforward, and as you'd expect with Python, it reads like a human sentence. The key is to start with the for loop, then read the expression last:

```python
[expression for item in iterable if condition]
```

Let's break down the various aspects of this:

- **expression:** The value or operation to include in the new list.  
- **item:** The variable representing each element in the iterable.  
- **iterable:** The collection being iterated over (e.g., list, range, string).  
- **condition** _(optional)_: A filter that determines whether to include an item.

## Common Examples

**1. Basic List Comprehension**

A classic use case is to create a list of squared numbers:

```python
squares = [x**2 for x in range(5)]
print(squares)
```

Output:

```python
[0, 1, 4, 9, 16]
```

**2. Adding a Condition**

You can also use a list comprehension to filter even numbers, and it would be just as easy to filter for odd numbers:

```python
evens = [x for x in range(10) if x % 2 == 0]
print(evens)
```

Output:

```python
[0, 2, 4, 6, 8]
```

**3. Transforming Elements**

Another useful implementation is to convert a list of strings to uppercase:

```python
words = ["hello", "world", "python"]
uppercase_words = [word.upper() for word in words]
print(uppercase_words)
```

Output:

```python
['HELLO', 'WORLD', 'PYTHON']
```

**4. Nested List Comprehensions**

Although they're a little harder to read, you can use nested list comprehensions to create lists of lists (or 2D lists). For example, you could generate a multiplication table:

```python
table = [[x * y for y in range(1, 4)] for x in range(1, 4)]
print(table)
```

Output:

```python
[[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

**5. List Comprehensions with `if-else`**

We can also use an if-else statement to apply a transformation based on a condition.

Note: this conditional approach is deliberately part of the expression versus a conditional for the iteration. If we had placed the conditional at the end, this would have filtered the elements out and our list would be shorter:

```python
numbers = [-2, -1, 0, 1, 2]
transformed = [x**2 if x > 0 else 0 for x in numbers]
print(transformed)
```

Output:

```python
[0, 0, 0, 1, 4]
```

## Common Use Cases

**1. Filtering Data**

List comprehensions are ideal for filtering data into a new list, and in this case, we have used a conditional to filter for words longer than 3 characters:

```python
words = ["cat", "elephant", "dog", "giraffe"]
long_words = [word for word in words if len(word) > 3]
print(long_words)
```

Output:

```python
['elephant', 'giraffe']
```

**2. Flattening Nested Lists**

Higher dimensional lists can be common in Python, but we can use a list comprehension to flatten a list of lists:

```python
nested = [[1, 2], [3, 4], [5, 6]]
flattened = [item for sublist in nested for item in sublist]
print(flattened)
```

Output:

```python
[1, 2, 3, 4, 5, 6]
```

## Key Takeaways

- List comprehensions are concise and improve code readability in your [Python projects](https://hackr.io/blog/python-projects).  
- Use these with conditions and transformations to customize your output.  
- Avoid over-complicating list comprehensions to keep them readable - keep it pythonic and follow the zen of Python!

## Practice Exercise

Here's a simple exercise to test yourself, try writing a list comprehension that creates a list of numbers divisible by both 3 and 5 between 1 and 50:

```python
divisible = [x for x in range(1, 51) if x % 3 == 0 and x % 5 == 0]
print(divisible)
```

## Wrapping Up

List comprehensions are a powerful feature of Python, enabling you to create and manipulate lists efficiently. By mastering their syntax and use cases, you can make your code both concise and Pythonic. Happy coding!