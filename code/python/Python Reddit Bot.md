# **Reddit Bot**

This Python project creates an automated Reddit bot with some new modules, namely _praw_ and _enchant_ (see the pip install commands).

This is a fairly simple concept as the program checks every comment in a selected subreddit and then replies to any comments that contain a predefined ‘trigger phrase’. To do this, we use the _praw_ module to interact with Reddit, and _enchant_ to generate similar words to the comment, allowing us to make an appropriate reply.

This idea is really useful if you’re looking for Python projects to learn how to answer questions in your own subreddit. You’d just need to expand this code to include automated responses for predefined questions (you’ve probably already noticed this being used by others on Reddit!). 

**Important note:** You’ll need to check out [these instructions](https://github.com/reddit-archive/reddit/wiki/OAuth2-Quick-Start-Example#first-steps) to get hold of a client_id, client_secret, username, password, and user_agent. You’ll need this information to make comments to Reddit via the API interface.

**Source Code:**

```python
'''
Reddit Reply Bot
-------------------------------------------------------------
pip install praw pyenchant
'''


import praw
import enchant


def reddit_bot(sub, trigger_phrase):
   reddit = praw.Reddit(
       client_id='your_client_id',
       client_secret='your_client_secret',
       username='your_username',
       password='your_pw',
       user_agent='your_user_agent'
   )

   subreddit = reddit.subreddit(sub)
   dict_suggest = enchant.Dict('en_US')

   for comment in subreddit.stream.comments():
       if trigger_phrase in comment.body.lower():
           word = comment.body.replace(trigger_phrase, '')
           reply_text = ''
           similar_words = dict_suggest.suggest(word)
           for similar in similar_words:
               reply_text += (similar + ' ')
           comment.reply(reply_text)


if __name__ == '__main__':
   reddit_bot(sub='Python', trigger_phrase='useful bot')
```