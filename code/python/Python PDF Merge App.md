# How To Create A PDF Merger App with Python with GUI

Have you ever needed to combine multiple PDF files into one but didn’t want to rely on online tools?

In this tutorial, I’ll show you how to build a Python program that merges PDF files seamlessly using Object-Oriented Programming (OOP). By the end, you’ll have a simple application with a graphical user interface (GUI) that allows users to select PDF files, merge them, and save the result with just a few clicks.

This project is beginner-friendly and demonstrates how to use Python libraries for PDF manipulation and GUI development. Let’s get started.

## How to Build a PDF Merger App

A PDF merger app is a practical [Python project](https://hackr.io/blog/python-projects) to enhance your Python skills. Here’s what you’ll learn:

- Using the _PyPDF2_ library to manipulate PDF files.
- Leveraging _tkinter_ to create a graphical user interface.
- Understanding Object-Oriented Programming (OOP) by structuring the app as a class.

The thing I like the most about this project is that it's not only educational but genuinely useful. It's also a great addition to your portfolio, as it shows you can build something that people really need.

Let’s dive in and build an app like the one I've shown below:

![A Python-based PDF merger application with a graphical user interface built using PyQt, featuring buttons to add and remove PDFs, merge files, and save the newly created PDF](https://i.imgur.com/alWk03P.gif)

## Project Prerequisites

Before we begin coding, let’s review the skills and tools needed for this tutorial.

Don’t worry if you’re not a [Python](https://hackr.io/blog/what-is-python) expert just yet! Having a few basics under your belt will make this journey smoother and more enjoyable.

**Basic Python Knowledge**

You should be familiar with:

- Variables, functions, and loops.
- Basic file handling and exception handling.

**Required Libraries**

We’ll use the following Python libraries:

- _PyPDF2_ for merging PDF files.
- _tkinter_ for creating the GUI.

If you don't already have _PyPDF2_ installed, just run this command:

```python
pip install PyPDF2
```

**A Curious and Experimental Mind**

To my mind, experimenting and making small adjustments are the keys to learning. So be ready to explore and debug as you go along!

## Step 1: Set Up Your Project

Before I jump into coding, let’s set up the project:

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, _pdf_merger.py_.

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started.

## Step 2: Import Required Libraries

Let’s start by importing the libraries we’ll use:

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PyPDF2 import PdfMerger
```

- We'll [use tkinter](https://hackr.io/blog/how-to-use-python-tkinter) to create the graphical interface for the app.
- _PdfMerger_ from _PyPDF2_ will allow us to combine multiple PDF files into one single PDF file.

## Step 2: Using Object-Oriented Programming in the App

We’ll structure the app using a Python class. I like this approach as it organizes related data and methods together, making the code reusable and easier to maintain.

The _PDFMergerApp_ class will:

- Initialize the app window and its components.  
- Define methods for adding, removing, and merging PDF files.  
- Handle user interactions with the graphical interface.

Here’s the structure of the class:

```python
class PDFMergerApp:
    def __init__(self, root):
        # Initialize the app window and variables

    def create_widgets(self):
        # Set up the GUI components

    def add_files(self):
        # Add PDF files to the list

    def remove_selected(self):
        # Remove selected PDF files from the list

    def merge_pdfs(self):
        # Merge the selected PDF files
```

## Step 3: Build the GUI with OOP

**Initialize the Main Window**

Here’s how the ___init___ method sets up the main application window:

```python
class PDFMergerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("PDF Merger")
        self.root.geometry("600x400")
        self.root.configure(bg="#f0f4f7")

        self.pdf_files = []

        self.create_widgets()
```

- _self.root_ sets the main window properties.  
- _self.pdf_files_ stores the list of selected PDF files.  
- _self.create_widgets()_ initializes the GUI components.

**Create the Widgets**

We'll then use the _create_widgets_ method to set up labels, buttons, and a listbox. That said, I'd highly encourage you to experiment with this stage and try out various designs and layouts to expand your _tkinter_ skills.

```python
def create_widgets(self):
    # Title Label
    title_label = tk.Label(
        self.root, text="PDF Merger", font=("Arial", 20), bg="#f0f4f7", fg="#333"
    )
    title_label.pack(pady=10)

    # Listbox to display selected PDF files
    self.listbox = tk.Listbox(
        self.root, selectmode=tk.MULTIPLE, width=50, height=10, font=("Arial", 12)
    )
    self.listbox.pack(pady=10)

    # Buttons Frame
    button_frame = tk.Frame(self.root, bg="#f0f4f7")
    button_frame.pack(pady=10)

    # Add File Button
    add_file_button = tk.Button(
        button_frame,
        text="Add Files",
        command=self.add_files,
        bg="#007bff",
        fg="white",
        font=("Arial", 12),
        padx=10,
        pady=5,
    )
    add_file_button.pack(side="left", padx=5)

    # Remove File Button
    remove_file_button = tk.Button(
        button_frame,
        text="Remove Selected",
        command=self.remove_selected,
        bg="#dc3545",
        fg="white",
        font=("Arial", 12),
        padx=10,
        pady=5,
    )
    remove_file_button.pack(side="left", padx=5)

    # Merge PDFs Button
    merge_button = tk.Button(
        self.root,
        text="Merge PDFs",
        command=self.merge_pdfs,
        bg="#28a745",
        fg="white",
        font=("Arial", 14),
        padx=20,
        pady=10,
    )
    merge_button.pack(pady=20)
```

- The _create_widgets_ method organizes GUI elements for better readability and modularity.  
- Buttons call specific methods like _self.add_files_ and _self.merge_pdfs_.

## Step 4: Add Functionality with OOP

**Add Files**

The _add_files_ method lets users select and display PDF files:

```python
def add_files(self):
    files = filedialog.askopenfilenames(
        filetypes=[("PDF Files", "*.pdf")], title="Select PDF Files"
    )
    for file in files:
        if file not in self.pdf_files:
            self.pdf_files.append(file)
            self.listbox.insert(tk.END, file)
```

**Remove Selected Files**

The _remove_selected_ method removes selected files:

```python
def remove_selected(self):
    selected_indices = self.listbox.curselection()
    for index in reversed(selected_indices):
        self.pdf_files.pop(index)
        self.listbox.delete(index)
```

**Merge PDFs**

The _merge_pdfs_ method combines the selected PDFs, and it also uses a [try-except](https://hackr.io/blog/python-try-except) block to gracefully handle errors:

```python
def merge_pdfs(self):
    if not self.pdf_files:
        messagebox.showwarning("No Files", "Please add at least two PDF files to merge.")
        return

    save_path = filedialog.asksaveasfilename(
        defaultextension=".pdf",
        filetypes=[("PDF Files", "*.pdf")],
        title="Save Merged PDF As",
    )
    if not save_path:
        return

    try:
        merger = PdfMerger()
        for pdf_file in self.pdf_files:
            merger.append(pdf_file)

        merger.write(save_path)
        merger.close()

        messagebox.showinfo("Success", f"Merged PDF saved to:\n{save_path}")
    except Exception as e:
        messagebox.showerror("Error", f"An error occurred while merging PDFs:\n{e}")
```

## Step 5: Run the Application

Finally, initialize and run the app:

```python
if __name__ == "__main__":
    root = tk.Tk()
    app = PDFMergerApp(root)
    root.mainloop()
```

With this block, we create the _Tkinter_ root window, instantiate an instance of the _PDFMergerAPp_ class, and then start the _Tkinter_ event loop with _root.mainloop()_.

## Full Program Source Code

Here’s the complete code for the Python PDF Merger App:

```python
'''
Hackr.io Python Tutorial: PDF Merger
'''
import tkinter as tk
from tkinter import filedialog, messagebox
from PyPDF2 import PdfMerger


class PDFMergerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("PDF Merger")
        self.root.geometry("600x400")
        self.root.configure(bg="#f0f4f7")

        self.pdf_files = []

        self.create_widgets()

    def create_widgets(self):
        # Title Label
        title_label = tk.Label(
            self.root, text="PDF Merger", font=("Arial", 20), bg="#f0f4f7", fg="#333"
        )
        title_label.pack(pady=10)

        # Listbox to display selected PDF files
        self.listbox = tk.Listbox(
            self.root, selectmode=tk.MULTIPLE, width=50, height=10, font=("Arial", 12)
        )
        self.listbox.pack(pady=10)

        # Buttons Frame
        button_frame = tk.Frame(self.root, bg="#f0f4f7")
        button_frame.pack(pady=10)

        # Add File Button
        add_file_button = tk.Button(
            button_frame,
            text="Add Files",
            command=self.add_files,
            bg="#007bff",
            fg="white",
            font=("Arial", 12),
            padx=10,
            pady=5,
        )
        add_file_button.pack(side="left", padx=5)

        # Remove File Button
        remove_file_button = tk.Button(
            button_frame,
            text="Remove Selected",
            command=self.remove_selected,
            bg="#dc3545",
            fg="white",
            font=("Arial", 12),
            padx=10,
            pady=5,
        )
        remove_file_button.pack(side="left", padx=5)

        # Merge PDFs Button
        merge_button = tk.Button(
            self.root,
            text="Merge PDFs",
            command=self.merge_pdfs,
            bg="#28a745",
            fg="white",
            font=("Arial", 14),
            padx=20,
            pady=10,
        )
        merge_button.pack(pady=20)

    def add_files(self):
        files = filedialog.askopenfilenames(
            filetypes=[("PDF Files", "*.pdf")], title="Select PDF Files"
        )
        for file in files:
            if file not in self.pdf_files:
                self.pdf_files.append(file)
                self.listbox.insert(tk.END, file)

    def remove_selected(self):
        selected_indices = self.listbox.curselection()
        for index in reversed(selected_indices):
            self.pdf_files.pop(index)
            self.listbox.delete(index)

    def merge_pdfs(self):
        if not self.pdf_files:
            messagebox.showwarning(
                "No Files", "Please add at least two PDF files to merge.")
            return

        save_path = filedialog.asksaveasfilename(
            defaultextension=".pdf",
            filetypes=[("PDF Files", "*.pdf")],
            title="Save Merged PDF As",
        )
        if not save_path:
            return

        try:
            merger = PdfMerger()
            for pdf_file in self.pdf_files:
                merger.append(pdf_file)

            merger.write(save_path)
            merger.close()

            messagebox.showinfo(
                "Success", f"Merged PDF saved to:\n{save_path}")
        except Exception as e:
            messagebox.showerror(
                "Error", f"An error occurred while merging PDFs:\n{e}")


# Main Application
if __name__ == "__main__":
    root = tk.Tk()
    app = PDFMergerApp(root)
    root.mainloop()
```

## Wrapping Up

Congratulations! You’ve just built a Python PDF merger app using OOP, that's quite something! Hopefully, this project has shown you how Python can combine libraries like _PyPDF2_ and _tkinter_ to create practical and genuinely useful applications.

Feel free to expand this project by:

- Allowing users to rearrange the order of selected files.
- Adding a preview feature to display the content of selected PDFs.
- Supporting other file types, such as images or text files.

Happy coding!