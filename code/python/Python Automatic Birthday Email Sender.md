# **Automatic Birthday Mail Sending**

This Python project uses the standard _smtplib_, _EmailMessage,_ and _datetime_ modules, in addition to _pandas_ and _openpyxl_ (these need to be pip installed, as shown below) to send automated birthday emails.

This program reads from an Excel sheet that contains all of your friends’ details (see Excel sheet format in source code below). It then sends them an email if today is their big day before making a note in your spreadsheet to say they’ve received their email.

We’ve used the _smtplib_ and _EmailMessage_ modules to create an SSL connection to our email account and message. We’ve then used a _pandas_ dataframe to store spreadsheet-style data within the Python program (an essential skill for data scientists). Finally, we used date formatting with the _datetime_ module’s _.strftime()_ function.

So, lots of new skills to get to grips with!

**Important note:** See my project on how to [set up Gmail for Python](https://hackr.io/blog/how-to-send-emails-with-python-using-gmail) before tackling this project.

**Source Code:**

```python
'''
Birthday Email Sender
-------------------------------------------------------------
pip install pandas openpyxl
excel file cols:
Name, Email, Birthday (MM/DD/YYYY), Last Sent (YYYY)
'''


import pandas as pd
from datetime import datetime
import smtplib
from email.message import EmailMessage


def send_email(recipient, subject, msg):
   GMAIL_ID = 'your_email_here'
   GMAIL_PWD = 'your_password_here'

   email = EmailMessage()
   email['Subject'] = subject
   email['From'] = GMAIL_ID
   email['To'] = recipient
   email.set_content(msg)

   with smtplib.SMTP_SSL('smtp.gmail.com', 465) as gmail_obj:
       gmail_obj.ehlo()
       gmail_obj.login(GMAIL_ID, GMAIL_PWD)
       gmail_obj.send_message(email)
   print('Email sent to ' + str(recipient) + ' with Subject: \''
         + str(subject) + '\' and Message: \'' + str(msg) + '\'')


def send_bday_emails(bday_file):
   bdays_df = pd.read_excel(bday_file)
   today = datetime.now().strftime('%m-%d')
   year_now = datetime.now().strftime('%Y')
   sent_index = []

   for idx, item in bdays_df.iterrows():
       bday = item['Birthday'].to_pydatetime().strftime('%m-%d')
       if (today == bday) and year_now not in str(item['Last Sent']):
           msg = 'Happy Birthday ' + str(item['Name'] + '!!')
           send_email(item['Email'], 'Happy Birthday', msg)
           sent_index.append(idx)

   for idx in sent_index:
       bdays_df.loc[bdays_df.index[idx], 'Last Sent'] = str(year_now)

   bdays_df.to_excel(bday_file, index=False)


if __name__ == '__main__':
   send_bday_emails(bday_file='your_bdays_list.xlsx')
```