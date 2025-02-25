
# How To Create A Python Error Notification App with Gmail

Have you ever wanted to stay updated about critical events or errors without constantly checking logs?

In this tutorial, I’ll show you how to build a Python program that monitors log files in real time and notifies you via email when specific events occur. By the end of this guide, you’ll have a practical tool that watches for issues and sends you alerts immediately.

This project is beginner-friendly and demonstrates how to use Python libraries for file monitoring and email automation. Let’s get started!

## Why Build an Error Monitoring System?

A log monitoring system is a great [Python project](https://hackr.io/blog/python-projects) to improve your skills while solving real-world problems. Here’s what you’ll learn:

- Using the _watchdog_ library to monitor file changes.  
- Leveraging _smtplib_ to send email notifications.  
- Customizing event handling by overriding methods in an OOP class.

The best part? This project is practical for system administrators, developers, or anyone who needs to monitor logs for errors or critical events in real-time.

This is also a great way to boost your portfolio with a project that has practical relevance in real Python roles.

Let’s dive in and build an app that does exactly what I've shown below:

![A Python log monitoring system that automates real-time file monitoring and notifications using Watchdog for detecting file changes and smtplib for sending email alerts, helping developers and system admins track critical events or errors instantly](https://i.imgur.com/bpZqwjD.gif)

## Project Prerequisites

Before we begin coding, let’s review the skills and tools needed for this tutorial.

Don’t worry if you’re not a [Python](https://hackr.io/blog/what-is-python) expert just yet! Having a few basics under your belt will make this journey smoother and more enjoyable.

**Basic Python Knowledge**

You should be familiar with:

- Variables, functions, and loops.  
- Basic file handling and exception handling.

**Required Libraries**

We’ll use the following Python libraries:

- **watchdog** for real-time file monitoring.

- **smtplib** for sending emails.

- **dotenv** for loading email credentials from a .env file.

If you don't already have _watchdog_ or _dotenv_ installed, just run this command:

```python
pip install watchdog python-dotenv
```

**A Curious and Experimental Mind**

To my mind, experimenting and making small adjustments is one of the keys to learning coding. So, be ready to explore and debug as you go along!

## Step 1: Set Up Your Project

Before we start coding, let’s set up the project:

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, _log_monitor.py_.

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started.

## Step 2: Import Required Libraries

Let’s begin by importing the libraries we’ll use in this project:

```python
import time
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from dotenv import load_dotenv
import os
```

Explanation:

- _watchdog_ provides tools to monitor file changes.

- _smtplib_ and _email_ libraries help send email notifications.

- _os_ and _dotenv_ libraries help to load email credentials from a _.env_ file.

## Step 3: Create the Email Notification System

Next, we’ll define a function to send email alerts when specific events occur, and we'll use a [try-except](https://hackr.io/blog/python-try-except) block for graceful error handling:

```python
# Load environment variables
load_dotenv()

# Email Configuration
SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587
EMAIL_ADDRESS = os.getenv("EMAIL_ADDRESS")
EMAIL_PASSWORD = os.getenv("EMAIL_PASSWORD")
RECIPIENT_EMAIL = "recipient_email@gmail.com"

def send_email(subject, body):
    try:
        msg = MIMEMultipart()
        msg['From'] = EMAIL_ADDRESS
        msg['To'] = RECIPIENT_EMAIL
        msg['Subject'] = subject
        msg.attach(MIMEText(body, 'plain'))

        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
        server.send_message(msg)
        server.quit()
        print(f"Email sent: {subject}")
    except Exception as e:
        print(f"Failed to send email: {e}")
```

For this app, I've opted to use Gmail as the email client, but of course, feel free to use whichever you prefer.

That said, if you want to follow along with me and use Gmail, you need to complete some additional steps to enable your Python app to access your Gmail account.

But don't worry, as I've created a walkthrough project to show you exactly how to [set up Gmail with your Python project](https://hackr.io/blog/how-to-send-emails-with-python-using-gmail).

## Step 4: Monitor the Log File

We’ll create a class that inherits from _FileSystemEventHandler_ to monitor file changes and detect specific events.

```python
class LogFileHandler(FileSystemEventHandler):
    def __init__(self, keywords, file_path):
        self.keywords = keywords
        self.file_path = file_path

    def on_modified(self, event):
        if event.src_path == self.file_path:
            with open(self.file_path, 'r') as file:
                lines = file.readlines()
                for line in lines[-10:]:  # Check only the last 10 lines
                    for keyword in self.keywords:
                        if keyword in line:
                            subject = f"Alert: {keyword} detected"
                            body = f"Keyword '{keyword}' detected in log:\n\n{line}"
                            send_email(subject, body)
```

**How This Works:**  
- The _LogFileHandler_ class inherits from _FileSystemEventHandler_.  
- We override the _on_modified_ method to specify actions when the monitored file changes.  
- The program checks the last 10 lines of the log file for specific keywords.

## Step 5: Run the Monitoring Program

Finally, we’ll set up the main function to initialize the observer and start monitoring:

```python
if __name__ == "__main__":
    path_to_watch = "mylog.log"  # Replace with your log file
    keywords_to_watch = ["ERROR", "CRITICAL", "500 Internal Server Error"]

    event_handler = LogFileHandler(keywords_to_watch, path_to_watch)
    observer = Observer()
    observer.schedule(event_handler, path=path_to_watch, recursive=False)

    print("Monitoring started...")
    observer.start()
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()
```

Key Points:  
- The _observer_ watches the log file for modifications.  
- When the log file changes, the _on_modified_ method in _LogFileHandler_ is triggered.

## Full Program Source Code

Here’s the complete source code for the Log Monitoring and Notification System:

```python
'''
Hackr.io Python Tutorial: Log Notification System
'''
import time
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv()

# Email Configuration
SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587
EMAIL_ADDRESS = os.getenv("EMAIL_ADDRESS")
EMAIL_PASSWORD = os.getenv("EMAIL_PASSWORD")
RECIPIENT_EMAIL = "recipient_email@gmail.com"

# Function to send email


def send_email(subject, body):
    try:
        msg = MIMEMultipart()
        msg['From'] = EMAIL_ADDRESS
        msg['To'] = RECIPIENT_EMAIL
        msg['Subject'] = subject
        msg.attach(MIMEText(body, 'plain'))

        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
        server.send_message(msg)
        server.quit()
        print(f"Email sent: {subject}")
    except Exception as e:
        print(f"Failed to send email: {e}")

# Define Event Handler


class LogFileHandler(FileSystemEventHandler):
    def __init__(self, keywords, file_path):
        self.keywords = keywords
        self.file_path = file_path

    def on_modified(self, event):
        if event.src_path == self.file_path:
            with open(self.file_path, 'r') as file:
                lines = file.readlines()
                for line in lines[-10:]:  # Check only the last 10 lines for efficiency
                    for keyword in self.keywords:
                        if keyword in line:
                            subject = f"Alert: {keyword} detected"
                            body = f"Keyword '{keyword}' detected in log:\n\n{line}"
                            send_email(subject, body)


# Main Function
if __name__ == "__main__":
    path_to_watch = "logs/mylog.log"  # Replace with your log file
    keywords_to_watch = ["ERROR", "CRITICAL", "500 Internal Server Error"]

    event_handler = LogFileHandler(keywords_to_watch, path_to_watch)
    observer = Observer()
    observer.schedule(event_handler, path=path_to_watch, recursive=False)

    print("Monitoring started...")
    observer.start()
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()
```

## Wrapping Up

Congratulations! You’ve just built a Python log monitoring and notification system. This program demonstrates how Python can automate real-world tasks using libraries like _watchdog_ and _smtplib_.

Feel free to extend this project by:

- Monitoring multiple files.  
- Adding support for SMS or Slack notifications.  
- Logging detected events for future analysis.

Happy coding!