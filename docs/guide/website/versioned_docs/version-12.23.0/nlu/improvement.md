---
id: version-12.23.0-improvement
title: Improving Your NLU
original_id: improvement
---

## Bot Improvement Module
Botpress provides a way for you to check if your QnA module's AI is performing well. You can do this by using the Bot Improvement module. This module lets the chatbot user rate responses from the chatbot using a simple thumbs-up/thumbs-down scale. These appear as two icons at the bottom left of the message bubble containing an answer from the QnA Module.

![Thumb Icons](../assets/b-i-chat.png)

### Module Interface
The bot improvement interface contains all the QnA items, which the user rated with a thumbs-down. These appear in the `Feedback Items` section. For context, a preview of the conversation is displayed when you select a feedback item.

![Graphical User Interface](../assets/b-i-interface.png)
### Pending
These are negative feedback items that an assigned collaborator has not yet inspected. Each feedback item has the following information attached.

#### Details
These are the message details, limited to the `event id` and message `timestamp`.

#### Detected Intent
A detected intent is the QnA entry that your chatbot elected as the best-fit answer to the users' questions. The intent type can either be a **Q&A** or a **Workflow**

### Solved
These are negative feedback items that an assigned collaborator has not yet inspected. 

### Conversation Preview
For context, the bot improvement module gives a preview of the conversation. It highlights the message responded to by the chatbot, allowing a collaborator to quickly identify why a user decided to rate the response with a thumbs-down.

## Misunderstood Module

Botpress ships with a **Misunderstood** module that helps you deal with user questions that the NLU engine did not match to any existing QnA's.

This module will allow you to:
1. View all phrases from the user which your chatbot didn't understand.
2. Assign them to a QnA or workflow.
![The Misunderstood Interface](../assets/misundertood-interface.png)
### Requirements
Activate the module in `.../data/global/botpress.config.json` or from the admin interface.

### Misunderstood Tab
The `Misunderstood` tab contains all user questions to which the chatbot could not find a response from the QnA module's question set. Of course, the answer may already be in this body of solutions. Still, if the calculated confidence level is too low for election, a user's question will end up being "misunderstood" and will appear in this interface.

The misunderstood tab has the following filters:

- **New** is where misunderstood questions that a collaborator has not yet inspected are.
- **Pending**  tab contains questions that have been classified as needing further investigation or which fall in the domain of another collaborator
- **Done** is where misunderstood questions that have been inspected and classified are found.
- **Ignored** tab contains phrases that were later handled by another process in the event engine. An example is keywords

### QnA Thumbs Down Tab
This tab is a replica of the Misunderstood tab. The only difference is that this tab captures answers from the QnA module, which the chatbot user rated with a thumbs-down. Your chatbot user rates this answer using two icons that appear at the bottom left of the message bubble containing a response from the QnA Module.

### Rectifying Misunderstood / Thumbs Down
The Misunderstood module provides tools to help you train your chatbot to understand questions flagged as unknown. You can do this in the _New Misunderstood_ pane, which is in the middle of the interface. In addition, there are three buttons labeled **Ignore**, **Skip**, and **Ammend**.

![The Misunderstood Interface](../assets/mis-interface-new-item.png)

#### Ignore
If you choose this option, a misunderstood phrase will be ignored, which is helpful for keywords that have been captured in the misunderstood module or for users who enter profane language or gibberish.

#### Skip
These are phrases that you may want to investigate further before deciding how you can handle them. Examples are phrases that may require expert knowledge to craft answers. Another collaborator can then go through these phrases, providing solutions and redirects.

#### Amend
The amend button lets you assign the misunderstood phrase to either an **Intent** or a **QnA**. There is a search capability for both of these. If you make a mistake when assigning, you can always use the undo button and assign again.

![The Misunderstood Interface](../assets/mis-solve-ammend.png)

> Currently, you cannot create a new QnA or a new intent from the Misunderstood module. Therefore, when the need arises, go to the module interface for either _QnA_ or _Natural Language Understanding_ and create then return to the Misunderstood module and classify.

## NLU Dataset Guideline

### General Guidelines
#### Number of utterances
The NLU engine uses Utterances to train your chatbot. Generally speaking, the more utterances, the better because AI models perform better predictions when they have a vast training body. The recommendation is:
- Per Intent: 10 to 20, plus 5 to 10 for every slot
- Per QNA: 10 to 20

#### Avoid duplication
Duplication when creating NLU datasets will result in redundant/useless data which the engine does not process. Below are forms of repetition you should always avoid.

**Within the same Intent/QNA** and **Across Intents/QNAs**

No training phrase should ever be duplicated. When you repeat training phrases, your chatbot will fail to train, and you will not be able to import that dataset to another chatbot.

**Singular/plural duplication**

Example: “Show bookmark” and “Show bookmarks”
We suggest that you use the singular and plural form in different phrases
Example: “Show bookmark” and “I want to see my bookmarks”

**Close duplicates**

When you try to add a variation, it is essential to add a different phrase instead of just adding a pronoun.
Example: “Show favorites” and “Show my favorites”

#### Avoid spelling and grammar mistakes
The NLU engine performs spellcheck on _the phrase written by a user during a conversation, not on your training dataset_. For matches to be close, your spelling and grammar should be impeccable. Take time to run spellcheck on your NLU training dataset.

#### Mix Utterances
- Mix natural language utterances with keyword-only utterances a well as natural language + fluff utterances
Example: “Reset password” and “I want to reset my password,” and “Can you please help me to reset my password for my emails”

- Mix utterances by using synonyms
Example: “hobby”, “leisure activities”, “free time”

- Mix abbreviations with written-out concepts in utterances
Example: “I have a problem with a PO” and “Purchase order issue”

- Case sensitivity: Text is converted to lowercase for intent matching

- Add different sentence structures
Example:
      See notifications               
      How can I view my reminders
      Look at my notifications list           
      Reminder summary
      I want to see my scheduled notifications. 
      Give a list of all my notifications 
      View all reminders              
      Where can I find reminders

#### Stick to one concept per Intent/QNA 

### When to use an Intent vs. QNA

#### Intent
When extracting information from the user input (e.g., dates and times), use an intent.
When a workflow needs to start (e.g., raise a ticket for a machine failure).

#### QNA
For straight forward Question and Answers

### Influence of Punctuation
The NLU engine ignores punctuation (except hyphens) when classifying text. 
Punctuation is considered when working with entities and slots.
Hyphenated words are joined as a single token.
