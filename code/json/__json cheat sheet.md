---
title: JSON Cheat Sheet
author: Adge Denkers
github: "@adgedenkers"
---

# Cheatsheet 📖

A quick reference for understanding JSON syntax and structure.

### **1. Basic JSON Structure**

JSON consists of **key-value pairs** enclosed in `{}` (curly braces).

```json
{
  "key": "value"
}
```

---

### **2. JSON Data Types**

JSON supports:

- **String** (`"Hello World"`)
- **Number** (`42`, `3.14`, `-10`)
- **Boolean** (`true`, `false`)
- **Null** (`null`)
- **Array** (`[ ]`)
- **Object** (`{ }`)

**Example:**

```json
{
  "name": "Alice",
  "age": 25,
  "isStudent": false,
  "hobbies": ["Reading", "Cycling", "Gaming"],
  "address": {
    "street": "123 Main St",
    "city": "Springfield",
    "zip": 12345
  }
}
```

---

### **3. JSON Arrays (Lists)**

Arrays are enclosed in `[ ]` and can contain multiple values.

**Example:**

```json
{
  "fruits": ["Apple", "Banana", "Cherry"]
}
```

**Array of Objects:**

```json
{
  "employees": [
    { "name": "John", "role": "Manager" },
    { "name": "Sarah", "role": "Developer" },
    { "name": "Mike", "role": "Designer" }
  ]
}
```

---

### **4. Nested JSON**

Objects can be **nested** inside other objects.

**Example:**

```json
{
  "company": "TechCorp",
  "location": {
    "city": "New York",
    "country": "USA"
  },
  "departments": [
    {
      "name": "Engineering",
      "employees": 50
    },
    {
      "name": "Marketing",
      "employees": 30
    }
  ]
}
```

---

### **5. Empty & Null Values**

Use `{}` for an empty object and `[]` for an empty array.

**Example:**

```json
{
  "data": {},
  "items": []
}
```

Use `null` for missing values.

```json
{
  "username": "JohnDoe",
  "email": null
}
```

---

### **6. JSON Best Practices ✅**

✅ Always use **double quotes** for keys and string values.  
✅ Use **consistent indentation** (2 or 4 spaces).  
✅ Avoid **trailing commas** (last item in an object or array should not have a comma).  
✅ Validate JSON using online tools like [jsonlint.com](https://jsonlint.com/).

---

Would you like this in a downloadable **Markdown or PDF format** for easier reference? 😊