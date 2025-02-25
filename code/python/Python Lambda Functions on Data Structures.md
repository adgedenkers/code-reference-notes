# **Python Lambda Functions for Lists, Dicts, and Pandas DataFrames**

Python’s lambda functions, also known as anonymous functions, offer a compact way to write small, single-use functions. They are particularly useful when applied to **lists**, **dictionaries**, and **Pandas DataFrames** for quick data transformations and filtering.

### **Basic Syntax**

A lambda function is defined using the `lambda` keyword:

```python
lambda arguments: expression
```

- **arguments**: A comma-separated list of parameters.
- **expression**: A single expression that is evaluated and returned by the function.

---

## **Using Lambda with Lists**

Lambda functions work well with list transformations, especially in combination with `map()`, `filter()`, and `sorted()`.

### **1. Mapping a Lambda Function to a List**

Apply a function to each element in a list using `map()`:

```python
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)
```

**Output:**

```
[1, 4, 9, 16, 25]
```

### **2. Filtering a List with Lambda**

Use `filter()` to select elements that meet a condition:

```python
numbers = [10, 15, 21, 30, 35]
multiples_of_5 = list(filter(lambda x: x % 5 == 0, numbers))
print(multiples_of_5)
```

**Output:**

```
[10, 15, 30, 35]
```

### **3. Sorting a List of Tuples**

Sort based on the second element using `sorted()`:

```python
tuples_list = [(1, 3), (2, 2), (4, 1)]
sorted_tuples = sorted(tuples_list, key=lambda x: x[1])
print(sorted_tuples)
```

**Output:**

```
[(4, 1), (2, 2), (1, 3)]
```

---

## **Using Lambda with Dictionaries**

Lambdas are useful when working with dictionaries for sorting and filtering operations.

### **1. Sorting a Dictionary by Values**

Sort a dictionary based on values:

```python
scores = {'Alice': 85, 'Bob': 92, 'Charlie': 78}
sorted_scores = dict(sorted(scores.items(), key=lambda item: item[1]))
print(sorted_scores)
```

**Output:**

```
{'Charlie': 78, 'Alice': 85, 'Bob': 92}
```

### **2. Filtering a Dictionary**

Select items where values meet a condition:

```python
filtered_scores = {k: v for k, v in scores.items() if v > 80}
print(filtered_scores)
```

**Output:**

```
{'Alice': 85, 'Bob': 92}
```

---

## **Using Lambda with Pandas DataFrames**

Lambdas are particularly powerful in **Pandas** for transforming and filtering data.

**Use this CSV file:**
![[lambda_csv_example.csv]]
### **1. Applying a Lambda Function to a Column**

Transform a DataFrame column using `apply()`:

```python
import pandas as pd

data = pd.DataFrame({'Name': ['Alice', 'Bob', 'Charlie'], 'Score': [85, 92, 78]})
data['Updated Score'] = data['Score'].apply(lambda x: x + 5)
print(data)
```

**Output:**

```
     Name  Score  Updated Score
0  Alice     85            90
1    Bob     92            97
2 Charlie     78            83
```

### **2. Conditional Column Updates**

Modify a column based on conditions:

```python
data['Pass/Fail'] = data['Score'].apply(lambda x: 'Pass' if x >= 80 else 'Fail')
print(data)
```

**Output:**

```
     Name  Score Pass/Fail
0  Alice     85     Pass
1    Bob     92     Pass
2 Charlie     78     Fail
```

### **3. Filtering a DataFrame with Lambda**

Select rows where `Score` is greater than 80:

```python
filtered_data = data[data['Score'].apply(lambda x: x > 80)]
print(filtered_data)
```

**Output:**

```
     Name  Score Pass/Fail
0  Alice     85     Pass
1    Bob     92     Pass
```

### **4. Creating a New Column Based on Multiple Columns**

Using `apply()` with multiple columns:

```python
data['Category'] = data.apply(lambda row: 'High' if row['Score'] > 90 else 'Medium' if row['Score'] > 80 else 'Low', axis=1)
print(data)
```

**Output:**

```
     Name  Score Pass/Fail Category
0  Alice     85     Pass   Medium
1    Bob     92     Pass    High
2 Charlie     78     Fail     Low
```

---

## **Key Takeaways**

- **Lambda functions** are a great way to write short, one-off functions.
- They work well with **lists** (map, filter, sorted).
- Can be applied efficiently to **dictionaries** (sorting, filtering).
- Are extremely powerful with **Pandas DataFrames** (apply, filtering, conditional logic).
- Best for **concise operations**—avoid using lambdas for complex logic.

### **Practice Challenge**

Try writing a lambda function that takes a DataFrame column and replaces values greater than 80 with "High" and others with "Low".

```python
data['High/Low'] = data['Score'].apply(lambda x: 'High' if x > 80 else 'Low')
print(data)
```

Happy coding!