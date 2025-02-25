# Build a Python Network Speed Test App with PyQt (Step-by-Step)

Want to **measure your internet speed** with a sleek GUI? In this project, you'll build a **Python-based network speed test tool** using **PyQt5** for a modern, interactive interface.

What You Will Learn:

- Using the `speedtest` library to measure internet speed  
- Creating a **responsive PyQt5 GUI**  
- Displaying real-time download, upload, and ping results  
- Styling the UI for a clean and modern look  
- Adding error handling for better user experience

## Step 1: Setting Up the Project

Before we start coding, let’s set up our [Python project](https://hackr.io/blog/python-projects):

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, `network_speed_test.py`.

Note, if you don't have the speedtest library installed, you'll need to run the following install command:

```python
pip install speedtest-cli
```

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started to produce something like I've shown below:

![A Python-based Network Speed Test Tool with a PyQt5 graphical interface that measures internet speed, including download and upload rates, and network latency, providing real-time results in an interactive desktop application.](https://i.imgur.com/Q4s7wFA.gif)

## Step 2: Understanding the Project

Our **Network Speed Test Tool** will:

- **Measure download and upload speeds** using the `speedtest` module.
- **Test network latency (ping)** to the nearest server.
- **Provide a clean UI** with results displayed clearly.
- **Offer a 'Start Test' button** for user control.

And don't worry if you're not sure how to create a GUI with PyQt, I've got you covered. Plus, I've also created a detailed guide on [how to use PyQt](https://hackr.io/blog/how-to-use-python-pyqt) if you want some extra help.

## Step 3: Importing Required Modules

We need the following Python modules:

```python
import sys
import urllib.request
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QLineEdit, QPushButton, QMessageBox
from PyQt5.QtGui import QFont
```

**Why These Modules?**

- **`sys`** → Required for running the PyQt5 application.
- **`speedtest`** → Measures internet download, upload speeds, and ping.
- **`PyQt5.QtWidgets`** → Builds the user interface.
- **`PyQt5.QtGui`** → Customizes fonts and appearance.

## Step 4: Sketching Out the Class and Methods

We'll structure our **PyQt5 GUI application** using a **class-based approach** via [OOP in Python](https://hackr.io/blog/python-object-oriented-programming).

```python
class SpeedTestApp(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()
    
    def init_ui(self):
        """Initializes the UI layout and widgets."""
        pass
    
    def run_speed_test(self):
        """Runs the network speed test and displays results."""
        pass
```

**How It Works:**

- **Encapsulation:**
    
    - The `SpeedTestApp` class encapsulates both the **GUI setup** and the **speed test functionality**, keeping the logic well-organized.
        
- **Class Constructor (**`**__init__**` **Method):**
    
    - Calls `super().__init__()` to inherit properties from `QWidget`.
        
    - Calls `self.init_ui()` to set up the graphical user interface.
        
- `**init_ui()**` **Method:**
    
    - Defines the structure of the UI by adding **labels, buttons, and result display areas**.
        
    - Connects the **'Start Test' button** to the `run_speed_test()` method.
        
- `**run_speed_test()**` **Method:**
    
    - Executes the network speed test when the button is clicked.
        
    - Updates the UI dynamically with **download, upload, and ping results**.
        

This class-based approach ensures **scalability, reusability, and maintainability**, making it easy to enhance or modify the application in the future.

## Step 5: Implementing the UI with PyQt5

```python
def init_ui(self):
    """Initializes the UI layout and widgets with styling."""
    self.setWindowTitle("Network Speed Test")
    self.setGeometry(100, 100, 400, 300)
    
    # Apply custom font and palette
    app_font = QFont("Arial", 12)
    self.setFont(app_font)
    
    layout = QVBoxLayout()
    
    self.label = QLabel("Click 'Start Test' to measure your network speed.")
    self.label.setStyleSheet("color: #660000; font-size: 14px; font-weight: bold;")
    layout.addWidget(self.label)
    
    self.start_button = QPushButton("Start Test", self)
    self.start_button.setStyleSheet("""
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
    """)
    self.start_button.clicked.connect(self.run_speed_test)
    layout.addWidget(self.start_button)
    
    self.results_label = QLabel("")
    self.results_label.setStyleSheet("color: #003366; font-size: 14px;")
    layout.addWidget(self.results_label)
    
    self.setLayout(layout)
    
    # Apply overall window styling
    self.setStyleSheet("""
        QWidget {
            background-color: #f2f2f2;
        }
    """)
```

**How It Works:**

- **Creates the application window**:
    
    - Sets the **title** to `"Network Speed Test"`.
        
    - Defines a **fixed window size** (`400x300` pixels) to ensure UI consistency.
        
- **Adds a label (**`**QLabel**`**) for user instructions**:
    
    - Displays `"Click 'Start Test' to measure your network speed."`.
        
    - Uses a **bold dark red font** to grab user attention.
        
- **Includes a 'Start Test' button (**`**QPushButton**`**)**:
    
    - Uses a **red background with white text** to make it stand out.
        
    - Implements a **hover effect** (darker red when hovered) to improve user interaction.
        
    - Calls the `run_speed_test()` method when clicked.
        
- **Displays results dynamically (**`**QLabel**`**)**:
    
    - Initially **empty** but updates with speed test results after execution.
        
    - Styled in **blue for contrast and readability**.
        
- **Applies overall UI styling (**`**setStyleSheet**`**)**:
    
    - Uses a **light gray background (**`**#f2f2f2**`**)** for a clean look.
        
    - Ensures that all elements maintain a **cohesive color scheme** (red, dark red, blue).
        

This **modern, structured layout** ensures that the app is **intuitive, visually appealing, and easy to use** for all users.

## Step 6: Implementing Speed Test Logic

```python
def run_speed_test(self):
    """Runs the network speed test and updates UI with results in real-time."""
    self.label.setText("Testing... Please wait.")
    QApplication.processEvents()

    try:
        st = speedtest.Speedtest()
        # RUN speedtest-cli --list to get a list of valid servers
        st.get_servers([1234])  # Replace with a valid Speedtest server ID

        # TEST DOWNLOAD SPEED
        self.label.setText("Testing Download Speed...")
        QApplication.processEvents()
        download_speed = st.download() / 1_000_000  # Convert to Mbps
        self.results_label.setText(f"Download Speed: {download_speed:.2f} Mbps\n")
        QApplication.processEvents()

        # TEST UPLOAD SPEED
        self.label.setText("Testing Upload Speed...")
        QApplication.processEvents()
        upload_speed = st.upload() / 1_000_000  # Convert to Mbps
        self.results_label.setText(
            f"Download Speed: {download_speed:.2f} Mbps\n"
            f"Upload Speed: {upload_speed:.2f} Mbps\n"
        )
        QApplication.processEvents()

        # TEST PING
        self.label.setText("Testing Ping...")
        QApplication.processEvents()
        ping = st.results.ping
        self.results_label.setText(
            f"Download Speed: {download_speed:.2f} Mbps\n"
            f"Upload Speed: {upload_speed:.2f} Mbps\n"
            f"Ping: {ping:.2f} ms"
        )
        self.label.setText("Testing Complete!")
        QApplication.processEvents()

    except Exception as e:
        self.results_label.setText("Error: Could not complete the test.")
```

**How It Works:**

- **Displays 'Testing…' message** to indicate that the test is starting.
    
    - `QApplication.processEvents()` is used after each update to ensure the UI remains responsive.
        
- **Uses** `**speedtest.Speedtest()**` **to initiate the test**:
    
    - **IMPORTANT:** Run `speedtest-cli --list` in the terminal to find available servers.
        
    - Calls `get_servers([1234])` to select a specific Speedtest server.
        
- **Runs each test sequentially and updates the UI dynamically**:
    
    1. **Download Speed Test:**
        
        - Updates the label to `"Testing Download Speed..."`.
            
        - Measures the download speed and updates the results label with a [Python f-string](https://hackr.io/blog/python-f-strings).
            
    2. **Upload Speed Test:**
        
        - Updates the label to `"Testing Upload Speed..."`.
            
        - Measures the upload speed and updates the results label again.
            
    3. **Ping Test:**
        
        - Updates the label to `"Testing Ping..."`.
            
        - Measures ping (latency) and updates the results label with all test data.
            
- **Ensures real-time feedback for the user**:
    
    - Instead of waiting for the entire test to finish, results are displayed **as they are received**.
        
    - This enhances user experience by showing progress dynamically.
        
- **Handles errors gracefully**:
    
    - If the test fails (e.g., no internet connection or server issue), our [try-except block](https://hackr.io/blog/python-try-except) returns an error message instead of crashing the application.
        

This approach provides a **smooth, real-time speed test experience** with a **responsive UI and immediate feedback**.

## Step 7: Running the PyQt5 App

```python
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = SpeedTestApp()
    window.show()
    sys.exit(app.exec_())
```

**How It Works:**

- **Initializes the PyQt5 application (**`**QApplication**`**)**:
    
    - The `QApplication` object **manages the event loop** and ensures smooth GUI performance.
        
    - `sys.argv` allows for proper handling of command-line arguments.
        
- **Creates an instance of the** `**SpeedTestApp**` **class**:
    
    - This initializes the **UI layout and button interactions**.
        
- **Displays the main application window (**`**show()**`**)**:
    
    - This ensures that the GUI appears when the script is executed.
        
- **Starts the event loop (**`**sys.exit(app.exec_())**`**)**:
    
    - The event loop waits for **user actions (button clicks, window interactions, etc.).**
        
    - `sys.exit()` ensures the application **closes cleanly when the user exits**.
        

By structuring the program this way, we ensure a **responsive and stable application**, allowing users to easily test their network speeds with a **simple and modern interface**.

## Final Code: Network Speed Test

```python
import sys
import speedtest
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QPushButton
from PyQt5.QtGui import QFont


class SpeedTestApp(QWidget):
    def __init__(self):
        """Initializes the speed test application with UI elements."""
        super().__init__()
        self.init_ui()

    def init_ui(self):
        """Sets up the application window, layout, and styled widgets."""
        self.setWindowTitle("Network Speed Test")
        self.setGeometry(100, 100, 400, 300)

        # Apply custom font
        app_font = QFont("Arial", 12)
        self.setFont(app_font)

        # Set layout
        layout = QVBoxLayout()

        # Instruction label
        self.label = QLabel("Click 'Start Test' to measure your network speed.")
        self.label.setStyleSheet("color: #660000; font-size: 14px; font-weight: bold;")
        layout.addWidget(self.label)

        # Start button
        self.start_button = QPushButton("Start Test", self)
        self.start_button.setStyleSheet("""
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
        """)
        self.start_button.clicked.connect(self.run_speed_test)
        layout.addWidget(self.start_button)

        # Results label
        self.results_label = QLabel("")
        self.results_label.setStyleSheet("color: #003366; font-size: 14px;")
        layout.addWidget(self.results_label)

        # Apply layout
        self.setLayout(layout)

        # Apply overall window styling
        self.setStyleSheet("""
            QWidget {
                background-color: #f2f2f2;
            }
        """)

    def run_speed_test(self):
        """Runs the network speed test and updates UI with results in real-time."""
        self.label.setText("Testing... Please wait.")
        QApplication.processEvents()

        try:
            st = speedtest.Speedtest()
            # RUN speedtest-cli --list to get a list of valid servers
            st.get_servers([1234])  # Replace with a valid Speedtest server ID

            # TEST DOWNLOAD SPEED
            self.label.setText("Testing Download Speed...")
            QApplication.processEvents()
            download_speed = st.download() / 1_000_000  # Convert to Mbps
            self.results_label.setText(f"Download Speed: {download_speed:.2f} Mbps\n")
            QApplication.processEvents()

            # TEST UPLOAD SPEED
            self.label.setText("Testing Upload Speed...")
            QApplication.processEvents()
            upload_speed = st.upload() / 1_000_000  # Convert to Mbps
            self.results_label.setText(
                f"Download Speed: {download_speed:.2f} Mbps\n"
                f"Upload Speed: {upload_speed:.2f} Mbps\n"
            )
            QApplication.processEvents()

            # TEST PING
            self.label.setText("Testing Ping...")
            QApplication.processEvents()
            ping = st.results.ping
            self.results_label.setText(
                f"Download Speed: {download_speed:.2f} Mbps\n"
                f"Upload Speed: {upload_speed:.2f} Mbps\n"
                f"Ping: {ping:.2f} ms"
            )
            self.label.setText("Testing Complete!")
            QApplication.processEvents()

        except Exception as e:
            self.results_label.setText("Error: Could not complete the test.")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = SpeedTestApp()
    window.show()
    sys.exit(app.exec_())
```

## Wrapping Up

Congratulations! You’ve built a **fully functional Network Speed Test Tool** in Python using PyQt5.

**What You Learned:**

- Using **PyQt5** to create a GUI-based tool  
- Running **download, upload, and ping tests** with `speedtest`  
- Structuring Python programs using **OOP principles**  
- Enhancing UI with **dynamic updates and styling**  
- Implementing **error handling** for a better user experience

**Next Steps:**

- **Add a real-time speed graph** using Matplotlib.  
- **Save test results to a file or database**.  
- **Allow users to choose test servers manually**.  
- **Build a web version using Flask or Streamlit**.

The best way to learn Python is by **building real projects**—so keep experimenting!