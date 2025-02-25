# # Build a Python Secure File Eraser App with PyQt (Step-by-Step)

Want to permanently delete sensitive files beyond recovery? In this project, you'll build a **Secure File Eraser** using Python and PyQt, ensuring that deleted files cannot be restored by forensic tools.

What You Will Learn:

- Using Python's `shutil` and `os` modules to securely delete files
- Implementing multiple overwrite passes for better security
- Creating a PyQt-based GUI for user-friendly file selection
- Adding confirmation dialogs to prevent accidental deletions
- Styling the UI for a modern and professional look

## Step 1: Setting Up the Project

Before we start coding, let’s set up our [Python project](https://hackr.io/blog/python-projects):

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, `secure_file_eraser.py`.

Note, if you don't have PyQt installed, you'll need to run the following install command:

```python
pip install pyqt5
```

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started to produce something like I've shown below:

![A Python-based Secure File Eraser with a PyQt graphical interface that permanently deletes sensitive files beyond recovery using multiple overwriting passes, ensuring data privacy and security.](https://i.imgur.com/afQlICX.gif)

## Step 2: Understanding the Project

Our **Secure File Eraser** will:

- Allow users to **select files** via a PyQt-based file picker.
- Overwrite the file with **random data multiple times** before deletion.
- Provide a **confirmation prompt** before proceeding with deletion.
- Offer a **simple and intuitive UI**.

And don't worry if you're not sure how to create a GUI with PyQt, I've got you covered. Plus, I've also created a detailed guide on [how to use PyQt](https://hackr.io/blog/how-to-use-python-pyqt) if you want some extra help.

## Step 3: Importing Required Modules

We need the following Python modules:

```python
import sys
import os
import random
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton, QFileDialog, QMessageBox
```

**Why These Modules?**

- `sys` → Required for running the PyQt application.
- `os` → Helps with file handling and deletion.
- `random` → [Python random](https://hackr.io/blog/python-random-function) generates random bytes for secure file overwriting.
- `PyQt5.QtWidgets` → Provides UI elements like buttons, labels, and file dialogs.

## Step 4: Sketching Out the Class and Methods

We’ll structure our PyQt5 GUI application using a **class-based approach** via [OOP in Python](https://hackr.io/blog/python-object-oriented-programming).

```python
class SecureFileEraser(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()
    
    def init_ui(self):
        """Initializes the UI layout and widgets."""
        pass
    
    def select_file(self):
        """Opens a file dialog for selecting the file to delete."""
        pass
    
    def securely_delete(self, file_path):
        """Overwrites and deletes the selected file securely."""
        pass
```

**How It Works:**

1. **Encapsulation of Functionality:**
    
    - The `SecureFileEraser` class encapsulates all functionality, making the application modular and easy to maintain.
        
    - This structure allows for better scalability if additional features are required in the future.
        
2. **Class Constructor (**`**__init__**` **Method):**
    
    - Calls `super().__init__()` to inherit properties from `QWidget`, enabling GUI capabilities.
        
    - Calls `self.init_ui()` to set up the graphical user interface components.
        
3. `**init_ui()**` **Method:**
    
    - This method will handle the layout, button placements, and widget styling.
        
    - It will ensure a user-friendly experience with clear instructions on how to use the tool.
        
4. `**select_file()**` **Method:**
    
    - Opens a file dialog that allows the user to select a file they want to delete.
        
    - Stores the selected file’s path in a class variable so it can be referenced later.
        
5. `**securely_delete()**` **Method:**
    
    - Once the file is selected and deletion is confirmed, this method will handle the secure overwriting process.
        
    - The file will be overwritten multiple times with random data before being deleted to prevent recovery.
        

By structuring the program this way, we ensure a **well-organized, scalable, and maintainable** application while keeping the logic modular and easy to modify.

## Step 5: Implementing the UI with PyQt5

```python
def init_ui(self):
        """Initializes the UI layout and widgets with styling."""
        self.setWindowTitle("Secure File Eraser")
        self.setGeometry(100, 100, 400, 300)
        
        # Apply custom font and palette
        app_font = QFont("Arial", 12)
        self.setFont(app_font)
        
        layout = QVBoxLayout()
        
        self.label = QLabel("Select a file to securely delete.")
        self.label.setStyleSheet("color: #FF0000; font-size: 14px; font-weight: bold;")
        layout.addWidget(self.label)
        
        self.select_button = QPushButton("Select File")
        self.select_button.setStyleSheet("""
            QPushButton {
                background-color: #FF0000;
                color: white;
                border-radius: 5px;
                padding: 10px;
                font-size: 14px;
                font-weight: bold;
            }
            QPushButton:hover {
                background-color: #CC0000;
            }
        """
        )
        self.select_button.clicked.connect(self.select_file)
        layout.addWidget(self.select_button)
        
        self.delete_button = QPushButton("Permanently Delete")
        self.delete_button.setStyleSheet("""
            QPushButton {
                background-color: #660000;
                color: white;
                border-radius: 5px;
                padding: 10px;
                font-size: 14px;
                font-weight: bold;
            }
            QPushButton:hover {
                background-color: #330000;
            }
        """
        )
        self.delete_button.clicked.connect(lambda: self.securely_delete(self.file_path) if hasattr(self, 'file_path') else None)
        layout.addWidget(self.delete_button)
        
        self.setLayout(layout)
        
        # Apply overall window styling
        self.setStyleSheet("""
            QWidget {
                background-color: #000000;
                color: white;
            }
        """
        )
```

**How It Works:**

- **Encapsulation of UI Setup:**
    
    - The `init_ui()` method centralizes all UI setup, ensuring a clean structure.
        
    - This makes styling and modifications easier in the future.
        
- **UI Components:**
    
    - A label provides instructions to the user.
        
    - A `Select File` button allows the user to choose a file for deletion.
        
    - A `Permanently Delete` button performs the secure deletion after selection.
        
- **Theming and Styling:**
    
    - A **red and black color scheme** is applied for a modern, high-contrast look.
        
    - Buttons have hover effects to improve user interaction.
        
    - The entire window background is black for better contrast and readability.
        

By applying this structure and styling, the **Secure File Eraser** now has a professional and user-friendly interface, ensuring a seamless user experience.

## Step 6: Implementing File Selection

```python
def select_file(self):
    """Opens a file dialog for selecting the file to delete."""
    file_dialog = QFileDialog()
    file_path, _ = file_dialog.getOpenFileName(self, "Select File to Delete")
    
    if file_path:
        self.file_path = file_path
        self.label.setText(f"Selected: {os.path.basename(file_path)}")
```

**How It Works:**

- **Encapsulation of File Selection:**
    
    - The `select_file()` method centralizes the logic for choosing a file.
        
    - This approach makes it easy to integrate additional features like file validation.
        
- **Opening the File Dialog:**
    
    - Uses `QFileDialog.getOpenFileName()` to open a system-native file selection window.
        
    - Allows users to browse and pick any file for secure deletion.
        
- **Storing the Selected File Path:**
    
    - The selected file's path is stored in `self.file_path`, ensuring the program remembers the chosen file.
        
    - This variable will be referenced later when the user proceeds with secure deletion.
        
- **Updating the UI Dynamically:**
    
    - After selecting a file, the program updates the label to display the file name via a [Python f-string](https://hackr.io/blog/python-f-strings).
        
    - This provides immediate feedback to the user, confirming the file selection.
        

By implementing file selection this way, we ensure a smooth and intuitive user experience while maintaining flexibility for future improvements.

## Step 7: Implementing Secure File Deletion

```python
def securely_delete(self, file_path):
    """Overwrites and deletes the selected file securely."""
    confirm = QMessageBox.question(
        self,
        "Confirm Deletion",
        f"Are you sure you want to permanently delete {os.path.basename(file_path)}?",
        QMessageBox.Yes | QMessageBox.No,
        QMessageBox.No
    )
    
    if confirm == QMessageBox.Yes:
        try:
            with open(file_path, "wb") as file:
                for _ in range(3):  # Overwrite file with random data 3 times
                    file.write(os.urandom(os.path.getsize(file_path)))
            
            os.remove(file_path)  # Delete the file
            self.label.setText("File securely deleted!")
        except Exception as e:
            self.label.setText("Error: Unable to delete file.")
```

**How It Works:**

- **Encapsulation of File Deletion:**
    
    - The `securely_delete()` method encapsulates the entire deletion process, ensuring a single point of control.
        
    - This modularity makes it easier to modify the deletion process, such as adding additional security measures.
        
- **Confirmation Dialog for Safety:**
    
    - Before deletion, a `QMessageBox.question()` prompt asks users to confirm the action.
        
    - This prevents accidental file deletions by ensuring the user explicitly agrees.
        
- **Secure Overwriting Process:**
    
    - Uses a `with open(file_path, "wb")` block to overwrite the file’s contents.
        
    - We use [Python range](https://hackr.io/blog/python-range-function) to ensure the file is overwritten **three times** using `os.urandom()`, ensuring old data is effectively destroyed.
        
    - This prevents forensic recovery tools from reconstructing the original contents.
        
- **Deleting the File:**
    
    - After overwriting, the file is permanently removed with `os.remove(file_path)`.
        
    - This ensures the file is not just hidden or moved to the trash but is completely erased.
        
    - We also implement a [try-except block](https://hackr.io/blog/python-try-except) to handle any errors with deletion.
- **Updating the UI Dynamically:**
    
    - If the deletion is successful, the label updates to indicate **"File securely deleted!"**
        
    - If an error occurs (e.g., permission issues), the UI displays an appropriate error message instead of crashing.
        

By implementing secure file deletion this way, we ensure the process is **user-friendly, effective, and irreversible**, making it nearly impossible for deleted files to be recovered.

## Step 8: Running the PyQt5 App

```python
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = SecureFileEraser()
    window.show()
    sys.exit(app.exec_())
```

**How It Works:**

- **Initializing the Application:**
    
    - The `QApplication` object manages the application’s event loop and ensures the UI is responsive.
        
    - `sys.argv` allows for proper handling of command-line arguments, though they are not used in this project.
        
- **Creating an Instance of the Secure File Eraser:**
    
    - `window = SecureFileEraser()` initializes the GUI and loads the main interface.
        
    - This ensures that the UI elements and logic are properly set up before the window is displayed.
        
- **Displaying the Application Window:**
    
    - The `window.show()` method makes the PyQt5 application visible to the user.
        
    - Without this call, the UI would not appear on the screen.
        
- **Starting the Event Loop:**
    
    - `sys.exit(app.exec_())` starts the PyQt5 event loop, listening for user interactions such as button clicks and window resizing.
        
    - Ensures the application remains open until the user manually closes it.
        

By structuring the application this way, we ensure that the **Secure File Eraser** runs smoothly, allowing users to easily interact with the GUI and securely delete files with confidence.

## Final Code: Secure File Eraser

```python
import sys
import os
import random
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton, QFileDialog, QMessageBox
from PyQt5.QtGui import QFont


class SecureFileEraser(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()

    def init_ui(self):
        """Initializes the UI layout and widgets with styling."""
        self.setWindowTitle("Secure File Eraser")
        self.setGeometry(100, 100, 400, 300)
        
        # Apply custom font and palette
        app_font = QFont("Arial", 12)
        self.setFont(app_font)
        
        layout = QVBoxLayout()
        
        self.label = QLabel("Select a file to securely delete.")
        self.label.setStyleSheet("color: #FF0000; font-size: 14px; font-weight: bold;")
        layout.addWidget(self.label)
        
        self.select_button = QPushButton("Select File")
        self.select_button.setStyleSheet("""
            QPushButton {
                background-color: #FF0000;
                color: white;
                border-radius: 5px;
                padding: 10px;
                font-size: 14px;
                font-weight: bold;
            }
            QPushButton:hover {
                background-color: #CC0000;
            }
        """
        )
        self.select_button.clicked.connect(self.select_file)
        layout.addWidget(self.select_button)
        
        self.delete_button = QPushButton("Permanently Delete")
        self.delete_button.setStyleSheet("""
            QPushButton {
                background-color: #660000;
                color: white;
                border-radius: 5px;
                padding: 10px;
                font-size: 14px;
                font-weight: bold;
            }
            QPushButton:hover {
                background-color: #330000;
            }
        """
        )
        self.delete_button.clicked.connect(lambda: self.securely_delete(self.file_path) if hasattr(self, 'file_path') else None)
        layout.addWidget(self.delete_button)
        
        self.setLayout(layout)
        
        # Apply overall window styling
        self.setStyleSheet("""
            QWidget {
                background-color: #000000;
                color: white;
            }
        """
        )
    
    def select_file(self):
        """Opens a file dialog for selecting the file to delete."""
        file_dialog = QFileDialog()
        file_path, _ = file_dialog.getOpenFileName(self, "Select File to Delete")
        
        if file_path:
            self.file_path = file_path
            self.label.setText(f"Selected: {os.path.basename(file_path)}")
    
    def securely_delete(self, file_path):
        """Overwrites and deletes the selected file securely."""
        confirm = QMessageBox.question(
            self,
            "Confirm Deletion",
            f"Are you sure you want to permanently delete {os.path.basename(file_path)}?",
            QMessageBox.Yes | QMessageBox.No,
            QMessageBox.No
        )
        
        if confirm == QMessageBox.Yes:
            try:
                with open(file_path, "wb") as file:
                    for _ in range(3):  # Overwrite file with random data 3 times
                        file.write(os.urandom(os.path.getsize(file_path)))
                
                os.remove(file_path)  # Delete the file
                self.label.setText("File securely deleted!")
            except Exception as e:
                self.label.setText("Error: Unable to delete file.")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = SecureFileEraser()
    window.show()
    sys.exit(app.exec_())
```

## Wrapping Up

Congratulations! You’ve built a **Secure File Eraser** with PyQt5 that ensures files are permanently deleted beyond recovery.

**What You Learned:**

- Using PyQt5 to create a GUI-based tool  
- Implementing secure file deletion with overwriting  
- Structuring Python programs with OOP principles  
- Adding confirmation dialogs to enhance user experience  
- Handling file selection via `QFileDialog`

**Next Steps:**

- Allow users to select multiple files for batch deletion.
- Implement an option to choose the number of overwrite passes.
- Add a **drag-and-drop** file selection feature.

The best way to learn is by **building projects**—so keep experimenting!