# Build a Python Speed Typing Test with Tkinter (Step-by-Step)

In this tutorial, we will build a **Speed typing test** using Python and Tkinter. This project is ideal for growing your typing speed while learning how to create an interactive GUI application.

By the end of this tutorial, you will have a functional speed typing test that:

- Displays a random sentence for the user to type.
- Tracks the time taken to complete the sentence.
- Calculates Words Per Minute (WPM).
- Allows the user to try again with a new sentence.

## Step 1: Setting Up the Project

Before we start coding, let’s set up our [Python project](https://hackr.io/blog/python-projects):

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, `speed_typing.py`.

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started and create something like I've shown below:

![GUI for speed typing test built with Tkinter in Python.](https://i.imgur.com/NcVQTqh.gif)

## Step 2: Importing Required Modules

First, we need to import the necessary modules:

```python
import tkinter as tk
from timeit import default_timer as timer
import random
```

### Why These Modules?

- **tkinter**: We will [use Tkinter](https://hackr.io/blog/how-to-use-python-tkinter) to create the graphical user interface.
- **timeit**: Helps measure the time taken to complete the typing test.
- **random**: selects a random sentence for each test.

## Step 3: Designing the Class Structure

We will use an [Object-Oriented Approach (OOP)](https://hackr.io/blog/python-object-oriented-programming) to structure the application. Below is the basic skeleton of our class:

```python
class SpeedTypingTest:
    def __init__(self, root):
        """Initialize the main game window and variables."""
        pass
    
    def setup_ui(self):
        """Set up the UI components such as labels, buttons, and text entry field."""
        pass
    
    def check_result(self):
        """Compare the user's input with the expected sentence and calculate the WPM."""
        pass
    
    def reset_test(self):
        """Reset the test with a new sentence and restart the timer."""
        pass
```

**Breakdown:**

- **`__init__`**: Initializes the application and sets up the UI.
- **`setup_ui`**: Creates the graphical elements.
- **`check_result`**: Compares the typed sentence, calculates **Words Per Minute (WPM)**, and displays results.
- **`reset_test`**: Resets the test and selects a new sentence.

## Step 4: Implementing the `__init__` Method

The `__init__` method initializes the Tkinter window, sets up game parameters, and calls the function that constructs the user interface.

```python
class SpeedTypingTest:
    def __init__(self, root):
        self.root = root
        self.root.title("Speed Typing Test")
        self.root.geometry("600x350")
        
        self.sentences = [
            "The quick brown fox jumps over the lazy dog.",
            "Speed typing improves accuracy and efficiency.",
            "Python is a powerful programming language."
        ]
        
        self.start_time = None
        self.setup_ui()
```

**Breakdown:**

- **Creates the Tkinter main window**:
    
    - `self.root.title("Speed Typing Test")` sets the window title.
        
    - `self.root.geometry("600x350")` defines the size of the window.
        
- **Defines a list of sentences**:
    
    - `self.sentences` contains multiple predefined sentences.
        
    - A random sentence will be chosen for each test.
        
- **Initializes** `**start_time**` **variable**:
    
    - This will be used to track the time taken to type the sentence.
        
- **Calls** `**setup_ui()**`:
    
    - This method will construct the user interface components.
        

This setup ensures that the application starts with the correct window properties and necessary variables initialized, creating a solid foundation for the speed typing test.

## Step 5: Implementing the `setup_ui` Method

The `setup_ui` method creates the user interface components such as labels, buttons, and the input field.

```python
def setup_ui(self):
    """Set up the UI components."""
    self.sentence = random.choice(self.sentences)
    
    self.label_sentence = tk.Label(self.root, text=self.sentence, font=("Arial", 14), wraplength=500)
    self.label_sentence.pack(pady=20)
    
    self.label_prompt = tk.Label(self.root, text="Type the above sentence:", font=("Arial", 12))
    self.label_prompt.pack()
    
    self.entry = tk.Entry(self.root, width=50)
    self.entry.pack(pady=10)
    self.entry.bind("<Return>", lambda event: self.check_result())  # Allow Enter key to submit
    
    self.button_done = tk.Button(self.root, text="Done", command=self.check_result, width=12, bg="lightblue")
    self.button_done.pack(pady=5)
    
    self.button_retry = tk.Button(self.root, text="Try Again", command=self.reset_test, width=12, bg="lightgrey")
    self.button_retry.pack(pady=5)
    
    self.result_label = tk.Label(self.root, text="", font=("Arial", 12))
    self.result_label.pack(pady=20)
    
    self.start_time = timer()  # Start the timer when UI loads
```

**Breakdown:**

- **Selects a random sentence**:
    
    - `self.sentence = random.choice(self.sentences)` ensures that the user gets a new challenge each time by relying on the [Python random](https://hackr.io/blog/python-random-function) module's built-in methods.
        
- **Displays the sentence to be typed**:
    
    - `self.label_sentence` is a `Label` widget that shows the sentence with a word wrap at 500 pixels.
        
- **Creates an input field**:
    
    - `self.entry` is an `Entry` widget where the user types their sentence.
        
    - It listens for the **Enter key (**`**<Return>**`**)** to submit automatically.
        
- **Adds buttons for user actions**:
    
    - `"Done"` button submits the typed text for checking.
        
    - `"Try Again"` button resets the test with a new sentence.
        
- **Displays results dynamically**:
    
    - `self.result_label` updates with either success (WPM and time) or an error message if the input is incorrect.
        
- **Starts the timer**:
    
    - `self.start_time = timer()` ensures the typing duration is tracked from the moment the UI appears.
        

This setup ensures that the user interface is intuitive, interactive, and responsive, making the speed typing test a smooth experience.

## Step 6: Implementing the `check_result` Method

The `check_result` method is responsible for verifying the user's input, calculating the time taken, and computing the Words Per Minute (WPM) score.

```python
def check_result(self):
    """Check if the typed sentence matches the displayed one and show the result."""
    typed_text = self.entry.get()
    if typed_text == self.sentence:
        end_time = timer()
        time_taken = round(end_time - self.start_time, 2)
        words = len(self.sentence.split())
        wpm = round((words / time_taken) * 60, 2)  # Words per minute calculation
        self.result_label.config(text=f"Time: {time_taken} sec | WPM: {wpm}", fg="green")
    else:
        self.result_label.config(text="Incorrect typing. Try again!", fg="red")
```

**Breakdown:**

- **Retrieves the user’s input**:
    
    - `typed_text = self.entry.get()` extracts the text entered in the input field.
        
- **Compares the typed sentence with the displayed one**:
    
    - If they match, the test is considered successful.
        
    - Otherwise, an error message is displayed.
        
- **Calculates the time taken to complete the test**:
    
    - `end_time = timer()` records the completion time.
        
    - `time_taken = round(end_time - self.start_time, 2)` computes the time difference in seconds.
        
- **Computes the Words Per Minute (WPM) score**:
    
    - `words = len(self.sentence.split())` counts the number of words in the given sentence.
        
    - `wpm = round((words / time_taken) * 60, 2)` converts the time into WPM by scaling it up to a full minute.
        
- **Displays the result dynamically**:
    
    - `self.result_label.config(text=f"Time: {time_taken} sec | WPM: {wpm}", fg="green")` updates the result label with performance metrics via a [Python f-string](https://hackr.io/blog/python-f-strings).
        
    - If incorrect, `self.result_label.config(text="Incorrect typing. Try again!", fg="red")` notifies the user to retry.
        

This method ensures that the typing speed test provides **instant feedback** to the user, enabling them to track their typing efficiency and accuracy.

## Step 7: Implementing the `reset_test` Method

The `reset_test` method resets the typing test by selecting a new sentence, clearing the input field, and restarting the timer.

```python
def reset_test(self):
    """Reset the test with a new sentence and restart the timer."""
    self.sentence = random.choice(self.sentences)
    self.label_sentence.config(text=self.sentence)
    self.entry.delete(0, tk.END)  # Clear input field
    self.result_label.config(text="")
    self.start_time = timer()  # Restart timer
```

**Breakdown:**

- **Selects a new sentence from the list**:
    
    - `self.sentence = random.choice(self.sentences)` ensures the user gets a new challenge each time.
        
- **Updates the displayed sentence**:
    
    - `self.label_sentence.config(text=self.sentence)` refreshes the UI with the new sentence.
        
- **Clears the input field for a fresh start**:
    
    - `self.entry.delete(0, tk.END)` removes any previously typed text.
        
- **Resets the result label**:
    
    - `self.result_label.config(text="")` ensures old results are not shown for the new test.
        
- **Restarts the timer**:
    
    - `self.start_time = timer()` begins tracking the time for the new attempt.
        

This ensures that the typing test restarts seamlessly, giving the user a new sentence and resetting all necessary fields.

## Step 8: Running the Application

Once all the methods have been implemented, we need to create an instance of our `SpeedTypingTest` class and run the application. This is done using the standard Python convention for running a script.

```python
if __name__ == "__main__":
    root = tk.Tk()
    app = SpeedTypingTest(root)
    root.mainloop()
```

**Breakdown:**

- **Ensures the script runs independently**:
    
    - The `if __name__ == "__main__"` statement ensures that the script only runs when executed directly and not when imported as a module.
        
- **Initializes the Tkinter root window**:
    
    - `root = tk.Tk()` creates the main application window.
        
- **Creates an instance of the** `**SpeedTypingTest**` **class**:
    
    - `app = SpeedTypingTest(root)` initializes the typing test UI and logic.
        
- **Runs the Tkinter event loop**:
    
    - `root.mainloop()` starts the event loop that listens for user interactions and updates the UI accordingly.
        

This final step ensures the application runs smoothly and allows the user to interact with the Speed Typing Test efficiently.

## Final Code: Speed Typing Test

```python
import tkinter as tk
from timeit import default_timer as timer
import random

class SpeedTypingTest:
    def __init__(self, root):
        self.root = root
        self.root.title("Speed Typing Test")
        self.root.geometry("600x350")
        
        self.sentences = [
            "The quick brown fox jumps over the lazy dog.",
            "Speed typing improves accuracy and efficiency.",
            "Python is a powerful programming language."
        ]
        
        self.start_time = None
        self.setup_ui()
    
    def setup_ui(self):
        """Set up the UI components."""
        self.sentence = random.choice(self.sentences)
        
        self.label_sentence = tk.Label(self.root, text=self.sentence, font=("Arial", 14), wraplength=500)
        self.label_sentence.pack(pady=20)
        
        self.label_prompt = tk.Label(self.root, text="Type the above sentence:", font=("Arial", 12))
        self.label_prompt.pack()
        
        self.entry = tk.Entry(self.root, width=50)
        self.entry.pack(pady=10)
        self.entry.bind("<Return>", lambda event: self.check_result())  # Allow Enter key to submit
        
        self.button_done = tk.Button(self.root, text="Done", command=self.check_result, width=12, bg="lightblue")
        self.button_done.pack(pady=5)
        
        self.button_retry = tk.Button(self.root, text="Try Again", command=self.reset_test, width=12, bg="lightgrey")
        self.button_retry.pack(pady=5)
        
        self.result_label = tk.Label(self.root, text="", font=("Arial", 12))
        self.result_label.pack(pady=20)
        
        self.start_time = timer()  # Start the timer when UI loads

    def check_result(self):
        """Check if the typed sentence matches the displayed one and show the result."""
        typed_text = self.entry.get()
        if typed_text == self.sentence:
            end_time = timer()
            time_taken = round(end_time - self.start_time, 2)
            words = len(self.sentence.split())
            wpm = round((words / time_taken) * 60, 2)  # Words per minute calculation
            self.result_label.config(text=f"Time: {time_taken} sec | WPM: {wpm}", fg="green")
        else:
            self.result_label.config(text="Incorrect typing. Try again!", fg="red")

    def reset_test(self):
        """Reset the test with a new sentence and restart the timer."""
        self.sentence = random.choice(self.sentences)
        self.label_sentence.config(text=self.sentence)
        self.entry.delete(0, tk.END)  # Clear input field
        self.result_label.config(text="")
        self.start_time = timer()  # Restart timer

if __name__ == "__main__":
    root = tk.Tk()
    app = SpeedTypingTest(root)
    root.mainloop()
```

## Wrapping Up

Congratulations! You’ve built a Speed Typing Test using Python and Tkinter. This project introduced you to key GUI development concepts such as handling user input, tracking time, computing WPM, and dynamically updating the interface.

**Next Steps:**

- **Expand the Sentence List:** Add more complex sentences to make the test more diverse.
    
- **Improve UI Design:** Enhance the layout with better fonts, colors, and styles.
    
- **Track High Scores:** Implement a system that saves and displays the fastest typing times.
    
- **Add Difficulty Levels:** Introduce beginner, intermediate, and advanced difficulty settings.
    
- **Implement Real-time Feedback:** Highlight incorrect words as the user types instead of waiting until the end.
    

By adding these features, you can create a more engaging and challenging typing test. Keep experimenting, refining, and happy coding!