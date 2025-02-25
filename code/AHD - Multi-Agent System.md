
# Adge's Multi-Agent System Design

## Agent Types

### Super-Agent in Charge (SAC)

This agent is like the brains of the operation. It routes the flow of data from trigger to action and even action, to action, to another action. It is responsible for splitting the flow into multiple actions. 

### Trigger Agents

These Agents take action when the state of what each is monitoring, changes. A new item is added, an item is removed, changed, etc.. 

- monitor a local or cloud folder
- monitor your email
- monitor your calendar
- monitor API data (weather/etc.)

### Action Agents

These Agents take action when directed to do so by the SAC.

- Save this file to Dropbox
- Send this API request
- Process this CSV file data

