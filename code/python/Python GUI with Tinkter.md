# How to Build Your First GUI with Tkinter & Python

If you’ve ever wanted to create desktop applications with Python, Tkinter is one of the easiest ways to get started. As Python’s standard GUI library, Tkinter makes it simple to design and build interactive graphical interfaces.

In this tutorial, I’ll walk through the basics of Tkinter, creating a simple app with labels, text fields, and buttons, while explaining the foundational concepts you need to know.

By the end of this guide, you’ll have your first functional Tkinter app and a solid understanding of how to expand these skills for more complex projects.

## What is Tkinter?

Tkinter is Python’s built-in library for creating graphical user interfaces (GUIs). It allows developers to build windows, dialogs, buttons, text fields, and much more. Since it comes pre-installed with [Python](https://hackr.io/blog/what-is-python), there’s no need for extra installation—you can jump right in.

Here are the three key concepts of Tkinter:

1. **The Main Window**: The primary application window where all widgets are placed.  
2. **Widgets**: Elements such as labels, buttons, and text fields that allow user interaction.  
3. **Event Loop**: A mechanism that keeps the application running and listens for user inputs (e.g., button clicks).

## Setting Up Your First Tkinter App

Want to turn some relatively simple code from your [Python project](https://hackr.io/blog/python-projects) into an app like I've shown below? Well, you can, and it's not as hard as you might think.

![Turn Python code into a GUI with tkinter](https://cdn.hackr.io/uploads/posts/attachments/1738091429nvaMhol0HL.webp)

That said, I think the best way to learn how to do this is to dive in and code a basic _Tkinter_ application with me.

So, open your [Python editor](https://app.hackr.io/editors/python) or IDE and then simply follow the steps below to create a Python project with a graphical user interface.

**Step 1: Creating a Python File & Importing Tkinter**

First, create a new Python file and name it something like _tkinter_tutorial.py_. Then, at the top of your file, import _Tkinter_ using the following code:

```python
import tkinter as tk
from tkinter import messagebox
```

We use _import tkinter as tk_ to make the code shorter and more readable. The _messagebox_ module is imported separately to display pop-up dialogs.

**Step 2: Creating the Main Window**

Next, we need to initialize the main application window, and this is a fairly repeatable step that you'll need for all _tkinter_ apps.

```python
root = tk.Tk()
root.title("Basic Tkinter App")
root.geometry("400x300")
root.configure(bg="#f4f4f4")
```

There are a few aspects to unpack here, so let's figure out what each line does:  
- **tk.Tk():** Creates the main application window.  
- **title():** Sets the title of the window.  
- **geometry():** Defines the size of the window in pixels (width x height).  
- **configure():** Allows customization of the window, such as setting the background color.

**Step 3: Adding Widgets**

Widgets are the building blocks of your GUI. Let's get our feet wet by adding a label, a text field, and buttons to our app.

**Adding a Label**

A label is used to display text in the application:

```python
label = tk.Label(
    root,
    text="Welcome to Tkinter!",
    font=("Arial", 16, "bold"),
    bg="#f4f4f4",
    fg="#333"
)
label.pack(pady=20)
```

Let’s break down what’s happening here:

- **tk.Label():** Creates a label widget. The root parameter indicates that this label is part of the main application window.
- **text:** The text to display in the label.
- **font:** Specifies the font type, size, and style (e.g., bold).
- **bg:** Sets the background color of the label.
- **fg:** Sets the text (foreground) color.
- **pack():** Places the widget in the window. The pady parameter adds vertical padding around the label to create spacing.

_**Why Do We Pass root?**_

You might be wondering why we pass the root object as an argument to widgets.

Well, in Tkinter, widgets need to know which window or container they belong to. Passing root ensures that each widget is associated with the main application window. Without this parameter, the widget wouldn’t be displayed in the app.

**Adding a Text Field**

An entry widget allows users to input text like any standard text field you've encountered:

```python
text_field = tk.Entry(root, font=("Arial", 14), width=25)
text_field.pack(pady=10)
```

Let's break down what we've done here:

- **tk.Entry():** Creates a single-line text input field. The root parameter ensures it’s part of the main application window.
- **font:** Specifies the font type and size for the text entered in the field.
- **width:** Determines the number of characters visible in the field.
- **pack():** Places the widget in the window and adds vertical padding.

**Adding Buttons**

Buttons let users trigger actions in the app. We've all used them before, so here’s how to add a submit button:

```python
def submit_action():
    user_input = text_field.get()
    if user_input.strip():
        messagebox.showinfo("Input Received", f"You entered: {user_input}")
    else:
        messagebox.showwarning("No Input", "Please enter something!")

submit_button = tk.Button(
    root,
    text="Submit",
    font=("Arial", 12),
    bg="#007bff",
    fg="white",
    command=submit_action
)
submit_button.pack(pady=10)
```

Let’s break it down:

- **tk.Button():** Creates a button widget. The root parameter places it in the main application window.
- **text:** The label displayed on the button.
- **font:** Sets the font type and size.
- **bg:** Sets the button’s background color.
- **fg:** Sets the button’s text (foreground) color.
- **command:** Links the button to the _submit_action_ function. When clicked, this function retrieves text from the entry field using _.get()_, checks if it’s empty, and shows a corresponding message.
- **pack():** Places the button in the window with vertical padding.

Finally, let’s also add a quit button:

```python
quit_button = tk.Button(
    root,
    text="Quit",
    font=("Arial", 12),
    bg="#dc3545",
    fg="white",
    command=root.quit
)
quit_button.pack(pady=10)
```

Here’s what each parameter does:

- **text:** Specifies the label on the button.
- **command:** Links the button to root.quit, which ends the application when clicked.
- **bg and fg:** Customizes the button’s background and text colors.
- **pack():** Places the button in the window with vertical padding.

**Step 4: Running the App**

To run the app, you need to include the following block of code at the end of your script:

```python
root.mainloop()
```

This starts Tkinter’s _event loop_, which keeps the app running and responsive. Then, simply run your Python script to reveal an app window like I've shown below.

![Successful creation of a Python GUI with tkinter](https://cdn.hackr.io/uploads/posts/attachments/1738091997H2pop73E2g.webp)

## Using Object-Oriented Programming with Tkinter

For larger applications, organizing your code with OOP is a best practice. If you're new to Python and unfamiliar with OOP, you may want to come back to this later. Otherwise, here's how to refactor our app using a class:

```python
class BasicApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Basic Tkinter App")
        self.root.geometry("400x300")
        self.root.configure(bg="#f4f4f4")
        self.create_widgets()

    def create_widgets(self):
        self.label = tk.Label(
            self.root, text="Welcome to Tkinter!", font=("Arial", 16, "bold"), bg="#f4f4f4", fg="#333"
        )
        self.label.pack(pady=20)

        self.text_field = tk.Entry(self.root, font=("Arial", 14), width=25)
        self.text_field.pack(pady=10)

        self.submit_button = tk.Button(
            self.root,
            text="Submit",
            font=("Arial", 12),
            bg="#007bff",
            fg="white",
            command=self.submit_action
        )
        self.submit_button.pack(pady=10)

        self.quit_button = tk.Button(
            self.root,
            text="Quit",
            font=("Arial", 12),
            bg="#dc3545",
            fg="white",
            command=self.root.quit
        )
        self.quit_button.pack(pady=10)

    def submit_action(self):
        user_input = self.text_field.get()
        if user_input.strip():
            messagebox.showinfo("Input Received", f"You entered: {user_input}")
        else:
            messagebox.showwarning("No Input", "Please enter something!")

if __name__ == "__main__":
    root = tk.Tk()
    app = BasicApp(root)
    root.mainloop()
```

How the OOP Version Works

- **Class Structure:** The application is encapsulated in the _BasicApp_ class. This makes the code more modular and easier to manage.
- **Passing root:** When creating an instance of _BasicApp_, the root object (our main window) is passed to the class. This allows all widgets and configurations to refer back to the same window.
- **Initialization (__init__):** The ___init___ method sets up the application by configuring the main window and calling the _create_widgets_ method.
- **Method Organization:** Widget creation and event handling are organized into separate methods within the class. For example, _create_widgets_ defines the layout, while _submit_action_ handles button clicks.

I hope you can see how neat and tidy this approach is, but just in case you need more convincing, here are the key advantages of OOP:

1. **Organization:** Group related functions and data together.  
2. **Reusability:** Easily extend the class for more features.  
3. **Readability:** Cleaner and more modular code.

## Wrapping Up

Congratulations! You’ve built your first Tkinter app and learned the foundational concepts of GUI programming in Python. With this knowledge, you can create more complex applications, such as calculators, to-do lists, or even image editors.

Feel free to experiment with the code, add more widgets, or improve the styling. Happy coding!