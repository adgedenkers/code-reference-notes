# # Build a Python File Encryption Tool with PyQt (Step-by-Step)

Want to protect your sensitive files from unauthorized access? In this project, you'll build a **File Encryption & Decryption Tool** using Python, PyQt, and the `cryptography` library to ensure your files remain secure.

What You Will Learn:

- Using the `cryptography` library to encrypt and decrypt files
- Implementing AES encryption for strong security
- Creating a PyQt-based GUI for user-friendly file selection
- Adding password-based encryption for extra protection
- Styling the UI for a modern and professional look

## Step 1: Setting Up the Project

Before we start coding, let’s set up our [Python project](https://hackr.io/blog/python-projects):

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, `file_encryptor.py`.

Note, if you don't have PyQt or cryptography installed, you'll need to run the following install command:

```python
pip install pyqt5 cryptography
```

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started to produce something like I've shown below:

![A Python-based file encryption and decryption tool with a PyQt5 graphical interface, utilizing AES encryption and password protection to securely protect sensitive files from unauthorized access](https://i.imgur.com/Nf2MLlo.gif)

## Step 2: Understanding the Project

Our **File Encryption & Decryption Tool** will:

- Allow users to **select files** via a PyQt-based file picker.
- Encrypt files using **AES encryption** for strong security.
- Require a **password** to decrypt files.
- Provide a **simple and intuitive UI**.

And don't worry if you're not sure how to create a GUI with PyQt, I've got you covered. Plus, I've also created a detailed guide on [how to use PyQt](https://hackr.io/blog/how-to-use-python-pyqt) if you want some extra help.

## Step 3: Importing Required Modules

We need the following Python modules:

```python
import sys
import os
import base64
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton, QFileDialog, QMessageBox, QLineEdit
from PyQt5.QtGui import QFont
from cryptography.fernet import Fernet
```

**Why These Modules?**

- `sys` → Required for running the PyQt application.
- `os` → Helps with file handling.
- `base64` → Encodes and decodes keys in a secure format.
- `PyQt5.QtWidgets` → Provides UI elements like buttons, labels, and file dialogs.
- `PyQt5.QtGui` → Allows for font customization and enhanced UI styling.
- `cryptography.fernet` → Implements AES encryption for secure file handling.

## Step 4: Sketching Out the Class and Methods

We’ll structure our PyQt5 GUI application using a **class-based approach** via [OOP in Python](https://hackr.io/blog/python-object-oriented-programming).

```python
class FileEncryptor(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()
    
    def init_ui(self):
        """Initializes the UI layout and widgets."""
        pass
    
    def select_file(self):
        """Opens a file dialog for selecting the file to encrypt or decrypt."""
        pass
    
    def encrypt_file(self, file_path, password):
        """Encrypts the selected file using AES encryption."""
        pass
    
    def decrypt_file(self, file_path, password):
        """Decrypts the selected file using AES decryption."""
        pass
```

**How It Works:**

- **Encapsulation of Functionality:**
    
    - The `FileEncryptor` class encapsulates the UI setup and encryption logic.
    - This makes it easy to expand features like key management or batch encryption.
- **Class Constructor (`__init__` Method):**
    
    - Calls `super().__init__()` to inherit from `QWidget`, enabling GUI functionality.
    - Calls `self.init_ui()` to initialize the graphical interface.
- **`init_ui()` Method:**
    
    - This method sets up the GUI layout, including file selection, password entry, and encryption buttons.
    - Ensures a smooth and user-friendly experience.
- **`select_file()` Method:**
    
    - Opens a file selection dialog to let users choose a file for encryption or decryption.
    - Stores the selected file path for further processing.
- **`encrypt_file()` & `decrypt_file()` Methods:**
    
    - Handles the actual encryption and decryption using AES.
    - Uses password-based encryption for added security.

By structuring the program this way, we ensure a **well-organized, scalable, and maintainable** application while keeping the logic modular and easy to modify.

## Step 5: Implementing the UI with PyQt5

```python
def init_ui(self):
        """Initializes the UI layout and widgets with styling."""
        self.setWindowTitle("File Encryption & Decryption Tool")
        self.setGeometry(100, 100, 450, 300)
        
        # Apply custom font and layout
        app_font = QFont("Arial", 12)
        self.setFont(app_font)
        layout = QVBoxLayout()
        
        self.label = QLabel("Select a file to encrypt or decrypt.")
        self.label.setStyleSheet("color: #FF0000; font-size: 14px; font-weight: bold;")
        layout.addWidget(self.label)
        
        self.password_input = QLineEdit(self)
        self.password_input.setPlaceholderText("Enter password")
        self.password_input.setEchoMode(QLineEdit.Password)
        self.password_input.setStyleSheet("""
            QLineEdit {
                border: 2px solid #FF0000;
                border-radius: 5px;
                padding: 5px;
                font-size: 14px;
            }
        """
        )
        layout.addWidget(self.password_input)
        
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
        
        self.encrypt_button = QPushButton("Encrypt File")
        self.encrypt_button.setStyleSheet("""
            QPushButton {
                background-color: #008000;
                color: white;
                border-radius: 5px;
                padding: 10px;
                font-size: 14px;
                font-weight: bold;
            }
            QPushButton:hover {
                background-color: #006600;
            }
        """
        )
        self.encrypt_button.clicked.connect(lambda: self.encrypt_file(self.file_path, self.password_input.text()) if hasattr(self, 'file_path') else None)
        layout.addWidget(self.encrypt_button)
        
        self.decrypt_button = QPushButton("Decrypt File")
        self.decrypt_button.setStyleSheet("""
            QPushButton {
                background-color: #000080;
                color: white;
                border-radius: 5px;
                padding: 10px;
                font-size: 14px;
                font-weight: bold;
            }
            QPushButton:hover {
                background-color: #000066;
            }
        """
        )
        self.decrypt_button.clicked.connect(lambda: self.decrypt_file(self.file_path, self.password_input.text()) if hasattr(self, 'file_path') else None)
        layout.addWidget(self.decrypt_button)
        
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
    
    - The `init_ui()` method initializes all UI components in one place for clarity and maintainability.
        
    - Ensures a clean separation of logic between UI and functionality.
        
- **UI Components:**
    
    - A label provides instructions to the user.
        
    - A `QLineEdit` field allows users to enter a password for encryption and decryption.
        
    - A `Select File` button enables users to choose a file.
        
    - `Encrypt File` and `Decrypt File` buttons trigger the respective processes.
        
- **Theming and Styling:**
    
    - A **red, green, and blue color scheme** enhances visual clarity.
        
    - Buttons have hover effects to improve user interaction.
        
    - The window background is black for a sleek, modern look.
        

By structuring and styling the UI this way, we ensure that the **File Encryption & Decryption Tool** is both **visually appealing and user-friendly**, making it simple for users to protect their sensitive data.

This approach ensures **clean, efficient, and dynamic event handling**, making the **File Encryption & Decryption Tool** more user-friendly and intuitive.

**Understanding Lambda Functions in Button Click Events**

In our UI, we use [Python lambda functions](https://hackr.io/blog/python-lambda-functions) to connect button clicks to encryption and decryption actions:

```python
self.encrypt_button.clicked.connect(lambda: self.encrypt_file(self.file_path, self.password_input.text()) if hasattr(self, 'file_path') else None)
self.decrypt_button.clicked.connect(lambda: self.decrypt_file(self.file_path, self.password_input.text()) if hasattr(self, 'file_path') else None)
```

**How It Works:**

- **Lambda Functions as Event Handlers:**
    
    - A lambda function is a short, anonymous function that executes when the button is clicked.
        
    - Instead of defining a separate method for handling clicks, we use a lambda to dynamically call `encrypt_file` or `decrypt_file` with the appropriate parameters.
        
- **Checking for a Selected File (**`**hasattr(self, 'file_path')**`**):**
    
    - The lambda first checks if `self.file_path` exists to ensure a file has been selected.
        
    - If `self.file_path` exists, it calls `encrypt_file()` or `decrypt_file()` with the selected file and the password entered by the user.
        
    - If no file is selected, it does nothing, preventing errors.
        
- **Passing User Input Dynamically:**
    
    - The `password_input.text()` method fetches the current password entered by the user at the moment of the button click.
        
    - This ensures that the correct password is used for encryption or decryption.
        

**Why Use Lambda Functions Here?**

- **Simplifies the Code:** Instead of defining multiple methods just for handling button clicks, we directly map the button event to its corresponding function call.
    
- **Ensures Parameters Are Passed Dynamically:** Unlike `connect(self.encrypt_file)`, which would require predefined arguments, the lambda allows passing `self.file_path` and `self.password_input.text()` at runtime.
    
- **Prevents Unnecessary Function Calls:** The function only executes when the button is clicked, ensuring efficient and controlled behavior.
    

## Step 6: Implementing File Selection

```python
def select_file(self):
    """Opens a file dialog for selecting the file to encrypt or decrypt."""
    file_dialog = QFileDialog()
    file_path, _ = file_dialog.getOpenFileName(self, "Select File to Encrypt/Decrypt")
    
    if file_path:
        self.file_path = file_path
        self.label.setText(f"Selected: {os.path.basename(file_path)}")
```

**How It Works:**

- **Encapsulation of File Selection:**
    
    - The `select_file()` method centralizes the logic for selecting a file.
        
    - This modular approach allows for easy future enhancements, such as multi-file selection or filtering specific file types.
        
- **Opening the File Dialog:**
    
    - Uses `QFileDialog.getOpenFileName()` to open a system-native file selection window.
        
    - This ensures compatibility with different operating systems and a smooth user experience.
        
- **Storing the Selected File Path:**
    
    - Once a file is selected, its path is stored in `self.file_path`.
        
    - This variable is referenced later during encryption or decryption operations.
        
- **Updating the UI Dynamically:**
    
    - After selecting a file, the `QLabel` updates to display the file name with a [Python f-string](https://hackr.io/blog/python-f-strings).
        
    - This provides immediate feedback to the user, confirming the file selection.
        

By structuring the file selection this way, we ensure an intuitive, user-friendly experience while maintaining a scalable and maintainable design.

## Step 7: Implementing File Encryption & Decryption

```python
def decrypt_file(self, file_path, password):
    """Decrypts the selected file using AES decryption."""
    key = base64.urlsafe_b64encode(password.ljust(32).encode('utf-8'))
    cipher = Fernet(key)
    
    try:
        with open(file_path, "rb") as file:
            encrypted_data = file.read()
        decrypted_data = cipher.decrypt(encrypted_data)
        
        new_file_path = file_path.replace(".enc", "")
        with open(new_file_path, "wb") as file:
            file.write(decrypted_data)
        
        self.label.setText("File successfully decrypted!")
    except Exception as e:
        self.label.setText("Error: Decryption failed.")
```

**How It Works:**

- **Encapsulation of Encryption & Decryption:**
    
    - The `encrypt_file()` and `decrypt_file()` methods centralize security operations.
        
    - This makes it easy to modify encryption strength, key handling, or add new security features.
        
- **Password-Based Key Generation:**
    
    - The user-provided password is padded to 32 bytes and encoded using `base64.urlsafe_b64encode()`.
        
    - This ensures that the key length matches AES encryption requirements.
        
- **File Encryption Process:**
    
    - Reads the file contents into memory.
        
    - Encrypts the contents using AES via `Fernet(cipher).encrypt()`.
        
    - Saves the encrypted data to a new file with a `.enc` extension.
        
- **File Decryption Process:**
    
    - Reads the encrypted file’s contents.
        
    - Decrypts the data using the same password-based AES key.
        
    - Saves the decrypted contents as a new file, removing the `.enc` extension.
        
- **Handling Errors & Updating the UI:**
    
    - If encryption or decryption fails (e.g., wrong password, corrupt file), the UI displays an error message via our [try-except block](https://hackr.io/blog/python-try-except).
        
    - Success messages are displayed after operations complete to provide feedback to the user.
        

By implementing encryption and decryption this way, we ensure **strong security, ease of use, and a user-friendly approach** to protecting sensitive data.

## Step 8: Running the PyQt5 App

```python
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = FileEncryptor()
    window.show()
    sys.exit(app.exec_())
```

**How It Works:**

- **Initializing the Application:**
    
    - The `QApplication` object is responsible for managing the GUI event loop and user interactions.
        
    - `sys.argv` allows proper handling of command-line arguments, though they are not needed in this project.
        
- **Creating an Instance of the File Encryptor:**
    
    - `window = FileEncryptor()` initializes the UI and loads all graphical elements.
        
    - This ensures that all widgets, buttons, and input fields are properly set up before being displayed.
        
- **Displaying the Application Window:**
    
    - `window.show()` makes the PyQt5 application visible to the user.
        
    - Without this step, the GUI would not appear.
        
- **Starting the Event Loop:**
    
    - `sys.exit(app.exec_())` starts the PyQt5 event loop, ensuring that the application continues running until the user manually closes it.
        
    - This event loop listens for interactions, such as button clicks and window events.
        

By structuring the application this way, we ensure that the **File Encryption & Decryption Tool** runs smoothly and efficiently, allowing users to interact with an intuitive and responsive GUI while securely managing their files.

## Final Code: File Encryption Tool

```python
import sys
import os
import base64
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton, QFileDialog, QMessageBox, QLineEdit
from PyQt5.QtGui import QFont
from cryptography.fernet import Fernet


class FileEncryptor(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()

    def init_ui(self):
        """Initializes the UI layout and widgets with styling."""
        self.setWindowTitle("File Encryption & Decryption Tool")
        self.setGeometry(100, 100, 450, 300)
        
        # Apply custom font and layout
        app_font = QFont("Arial", 12)
        self.setFont(app_font)
        layout = QVBoxLayout()
        
        self.label = QLabel("Select a file to encrypt or decrypt.")
        self.label.setStyleSheet("color: #FF0000; font-size: 14px; font-weight: bold;")
        layout.addWidget(self.label)
        
        self.password_input = QLineEdit(self)
        self.password_input.setPlaceholderText("Enter password")
        self.password_input.setEchoMode(QLineEdit.Password)
        self.password_input.setStyleSheet("""
            QLineEdit {
                border: 2px solid #FF0000;
                border-radius: 5px;
                padding: 5px;
                font-size: 14px;
            }
        """
        )
        layout.addWidget(self.password_input)
        
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
        
        self.encrypt_button = QPushButton("Encrypt File")
        self.encrypt_button.setStyleSheet("""
            QPushButton {
                background-color: #008000;
                color: white;
                border-radius: 5px;
                padding: 10px;
                font-size: 14px;
                font-weight: bold;
            }
            QPushButton:hover {
                background-color: #006600;
            }
        """
        )
        self.encrypt_button.clicked.connect(lambda: self.encrypt_file(self.file_path, self.password_input.text()) if hasattr(self, 'file_path') else None)
        layout.addWidget(self.encrypt_button)
        
        self.decrypt_button = QPushButton("Decrypt File")
        self.decrypt_button.setStyleSheet("""
            QPushButton {
                background-color: #000080;
                color: white;
                border-radius: 5px;
                padding: 10px;
                font-size: 14px;
                font-weight: bold;
            }
            QPushButton:hover {
                background-color: #000066;
            }
        """
        )
        self.decrypt_button.clicked.connect(lambda: self.decrypt_file(self.file_path, self.password_input.text()) if hasattr(self, 'file_path') else None)
        layout.addWidget(self.decrypt_button)
        
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
        """Opens a file dialog for selecting the file to encrypt or decrypt."""
        file_dialog = QFileDialog()
        file_path, _ = file_dialog.getOpenFileName(self, "Select File to Encrypt/Decrypt")
        
        if file_path:
            self.file_path = file_path
            self.label.setText(f"Selected: {os.path.basename(file_path)}")
    
    def encrypt_file(self, file_path, password):
        """Encrypts the selected file using AES encryption."""
        key = base64.urlsafe_b64encode(password.ljust(32).encode('utf-8'))
        cipher = Fernet(key)
        
        try:
            with open(file_path, "rb") as file:
                file_data = file.read()
            encrypted_data = cipher.encrypt(file_data)
            
            with open(file_path + ".enc", "wb") as file:
                file.write(encrypted_data)
            
            self.label.setText("File successfully encrypted!")
        except Exception as e:
            self.label.setText("Error: Encryption failed.")

    def decrypt_file(self, file_path, password):
        """Decrypts the selected file using AES decryption."""
        key = base64.urlsafe_b64encode(password.ljust(32).encode('utf-8'))
        cipher = Fernet(key)
        
        try:
            with open(file_path, "rb") as file:
                encrypted_data = file.read()
            decrypted_data = cipher.decrypt(encrypted_data)
            
            new_file_path = file_path.replace(".enc", "")
            with open(new_file_path, "wb") as file:
                file.write(decrypted_data)
            
            self.label.setText("File successfully decrypted!")
        except Exception as e:
            self.label.setText("Error: Decryption failed.")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = FileEncryptor()
    window.show()
    sys.exit(app.exec_())
```

## Wrapping Up

Nice work! You’ve built a Python **File Encryption & Decryption Tool** with PyQt5 that ensures files can be securely encrypted and decrypted with password protection.

**What You Learned:**

- Using PyQt5 to create a GUI-based tool
    
- Implementing AES encryption for secure file handling
    
- Structuring Python programs with OOP principles
    
- Handling password-based encryption securely
    
- Managing file selection with `QFileDialog`
    

**Next Steps:**

- Allow users to generate and store encryption keys separately.
    
- Implement support for encrypting multiple files in batch mode.
    
- Add an option to choose between different encryption algorithms.
    
- Introduce a secure password hashing mechanism to strengthen security.
    

The best way to learn is by **building projects**—so keep experimenting!
