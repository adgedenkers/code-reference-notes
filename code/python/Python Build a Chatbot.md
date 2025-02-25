# Python **Chatbot**

This Python project uses the _chatterbot_ module (see pip install instructions below) to train an automated chatbot to answer any question you ask! I know; we’re now using AI!

You’ll see that the program is one of the relatively small Python projects in this list, but feel free to explore the ChatterBot documentation along with the broader field of AI chatbots if you’d like to learn more or expand the code’s features.

If this has whet your appetite for building AI bots, check out our [24-Hour Python Chatbot course](https://hackr.io/blog/24-hour-python-chatbot-course) to learn how to build an AI-powered chatbot with the same tools that power OpenAI’s ChatGPT.

And if you're hungry for even more AI goodness, check out OpenAI,'s latest feature that lets you [build your own GPT](https://hackr.io/blog/how-to-create-your-own-gpt).

**Important note:** ChatterBot is no longer being actively maintained. This means you need to make a small change to the tagging.py file located in the ‘Lib/site-packages/chatterbot’ directory of your Python installation folder.

Don’t worry; it’s straightforward to do, and we’ve included the exact source code you need to use, as shown below.

**Source Code:**

```python
'''
Chat Bot
-------------------------------------------------------------
1) pip install ChatterBot chatterbot-corpus spacy
2) python3 -m spacy download en_core_web_sm
   Or... choose the language you prefer
3) Navigate to your Python3 directory
4) Modify Lib/site-packages/chatterbot/tagging.py
  to properly load 'en_core_web_sm'
'''


from chatterbot import ChatBot
from chatterbot.trainers import ChatterBotCorpusTrainer


def create_chat_bot():
   chatbot = ChatBot('Chattering Bot')
   trainer = ChatterBotCorpusTrainer(chatbot)
   trainer.train('chatterbot.corpus.english')

   while True:
       try:
           bot_input = chatbot.get_response(input())
           print(bot_input)

       except (KeyboardInterrupt, EOFError, SystemExit):
           break


if __name__ == '__main__':
   create_chat_bot()
```

**Modify tagging.py:**

Find the first code snippet, which is part of the __init__ method for the PosLemmaTagger class. Replace this with the _if/else_ statement.

**Note:** this example is for the English library we used in our example, but feel free to switch this out to another language if you’d prefer.

```python
# Replace this:
self.nlp = spacy.load(self.language.ISO_639_1.lower())

# With this:
if self.language.ISO_639_1.lower() == 'en':
   self.nlp = spacy.load('en_core_web_sm')
else:
   self.nlp = spacy.load(self.language.ISO_639_1.lower())
```