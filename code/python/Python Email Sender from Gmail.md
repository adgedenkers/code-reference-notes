# How to Send Emails in Python with Gmail (SMTP Tutorial!)

If you've ever needed to send emails from a Python script — whether for notifications, automated reports, or alerts — you’ll be glad to know that Gmail SMTP makes it simple. With a few lines of code, you can integrate email functionality into your Python projects.

In this tutorial, I’ll walk you through setting up Gmail SMTP access securely and creating a reusable email function that you can easily integrate into any project.

## What is SMTP and Why Use Gmail?

SMTP (Simple Mail Transfer Protocol) is the industry standard for sending emails. Gmail’s SMTP service allows developers to programmatically send emails using their Google accounts.

### **Why use Gmail SMTP?**

- **Reliable & Free**: You can send emails without needing third-party services.
- **Secure**: Supports encryption and authentication.
- **Easy to Use**: Works with Python’s built-in `smtplib` module.

## Step 1: Enable Gmail SMTP Access

Before writing any code, we need to configure your Gmail account for SMTP access, but this is very simple to do:

- Log in with your Google account, and head to your Google account page at [https://myaccount.google.com](https://myaccount.google.com/)
- Click on the 'Security' option in the left-hand menu
- Click on '2-Step verification'
- Enable '2-Step verification' with whichever method you prefer (unless it's already enabled)
- Stay within the '2-Step verification' page and scroll down to 'App passwords'
- Click on ‘App Passwords’
- Enter a name for your app - this could be 'Email Client'
- Click on ‘Create’ then save this password

We now have a brand new password for use within our Python apps.

**Important note:** This app password is not the same as the one you use to login to your Gmail account, it is only for programmatic use within your [Python projects](https://hackr.io/blog/python-projects).

## Step 2: Install Dependencies

Great, now, let's set to work on installing our required packages for creating a secure email script in [Python](https://hackr.io/blog/what-is-python):

```python
pip install python-dotenv
```

If you're not sure why we need this, well, the idea here is to store our email credentials securely using environment variables. This is much better than hardcoding them into a script from a security standpoint.

## Step 3: Store Credentials Securely

Instead of hardcoding your email credentials in Python, it's a lot more secure to store them in a `.env` file. Don't be intimidated by this, it's very easy.

### **Create a `.env` File**

Inside your project folder, create a file named `.env` and add the following environment variables. Of course, be sure to add your Gmail address and the app password we generated previously:

```python
EMAIL_ADDRESS=your_email@gmail.com
EMAIL_PASSWORD=your_app_password
```

This prevents accidental credential exposure when sharing code.

## Step 4: Writing the Email Sending Script

Now, let’s write a Python script to send emails using Gmail SMTP.

### **Step 1: Import Required Modules**

```python
import smtplib
import os
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from dotenv import load_dotenv
```

We import:

- `smtplib` for email sending.
- `os` to retrieve environment variables.
- `MIMEText` & `MIMEMultipart` for email formatting.
- `load_dotenv` to load credentials from `.env`.

### **Step 2: Load Credentials and Set SMTP Configuration**

```python
# Load environment variables
load_dotenv()

# Email Configuration
SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587
EMAIL_ADDRESS = os.getenv("EMAIL_ADDRESS")
EMAIL_PASSWORD = os.getenv("EMAIL_PASSWORD")
```

We use `load_dotenv()` to securely retrieve email credentials and then configure Gmail’s SMTP settings.

### **Step 3: Create a Reusable Email Function**

Now, let's write a simple function that we can use to send email within our Python apps via Gmail.

```python
def send_email(recipient_email, subject, body):
    """
    Sends an email using Gmail SMTP.
    
    Args:
        recipient_email (str): The recipient's email address.
        subject (str): The subject of the email.
        body (str): The email body content.
    """
    try:
        msg = MIMEMultipart()
        msg['From'] = EMAIL_ADDRESS
        msg['To'] = recipient_email
        msg['Subject'] = subject

        msg.attach(MIMEText(body, 'plain'))

        with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
            server.starttls()
            server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
            server.sendmail(EMAIL_ADDRESS, recipient_email, msg.as_string())
        
        print("Email sent successfully!")
    except Exception as e:
        print(f"Error sending email: {e}")
```

This function:

1. Creates an email object with `MIMEMultipart()`.
2. Sets sender, recipient, and subject.
3. Attaches the email body.
4. Logs into Gmail SMTP and sends the email.

### **Step 4: Testing the Script**

Now we have our email sender function, let's test this to verify that we can send and receive emails via our Python code.

```python
if __name__ == "__main__":
    recipient = "recipient_email@gmail.com"
    subject = "Test Email from Python"
    body = "This is a test email sent using Python and Gmail SMTP."
    send_email(recipient, subject, body)
```

Simply run the script to send a test email, and voila!

## Step 5: Making It Reusable for Any Project

For better modularity, it's also a good idea to save the function in a separate file called _email_utils.py_. Then, you can import and use it in any script like this:

```python
from email_utils import send_email

send_email("recipient@example.com", "Subject Here", "Email body content")
```

This allows easy email sending from different scripts without rewriting code.

## Full Program Source Code

```python
import smtplib
import os
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

# Email Configuration
SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587
EMAIL_ADDRESS = os.getenv("EMAIL_ADDRESS")
EMAIL_PASSWORD = os.getenv("EMAIL_PASSWORD")


def send_email(recipient_email, subject, body):
    """
    Sends an email using Gmail SMTP.
    
    Args:
        recipient_email (str): The recipient's email address.
        subject (str): The subject of the email.
        body (str): The email body content.
    """
    try:
        msg = MIMEMultipart()
        msg['From'] = EMAIL_ADDRESS
        msg['To'] = recipient_email
        msg['Subject'] = subject

        msg.attach(MIMEText(body, 'plain'))

        # Connect to the server and send email
        with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
            server.starttls()
            server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
            server.sendmail(EMAIL_ADDRESS, recipient_email, msg.as_string())
        
        print("Email sent successfully!")
    except Exception as e:
        print(f"Error sending email: {e}")


# Example usage
if __name__ == "__main__":
    recipient = "recipient_email@gmail.com"
    subject = "Test Email from Python"
    body = "This is a test email sent using Python and Gmail SMTP."
    send_email(recipient, subject, body)
```

## Wrapping Up

Congratulations! You’ve successfully set up Gmail SMTP in Python. Now, you can:

- Send emails programmatically.

- Store credentials securely.

- Reuse the function in different projects.

Try modifying the script to send attachments, HTML emails, or schedule automated reports. If you found this guide helpful, feel free to share it. Happy coding!