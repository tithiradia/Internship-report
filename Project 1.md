# 🚀 Project 1: Social Media Feedback Bot

## 📅 Project Description

An interactive Telegram bot built to collect structured user feedback using natural conversation. It captures details such as name, feedback text, and issue type, stores them in Google Sheets, and performs sentiment analysis using GPT-4o.

## ⚙️Working Mechanism (DETAILED FLOW) 
### 1. Telegram Trigger 
• The Telegram Bot is set up in n8n using the Telegram Trigger node. 

• It listens for incoming messages from users on Telegram.

• As soon as a user sends a message to the bot, the workflow is triggered. 

### 2. AI Agent using GPT-4o 
• Once the message is received, it is passed to the AI Agent node (GPT-4o via OpenAI API or n8n’s AI 
integration). 

• GPT-4o guides the user through a step-by-step conversation: 

    o Step 1: “Hi! May I know your name?” 

    o Step 2: “Thanks, [Name]! Can you share your feedback about our product/service?”

    o Step 3: “Is this feedback related to any issue? If yes, can you specify the issue type (e.g., bug, 
    delay, service quality)?” 

• Each user reply is stored in variables as part of the flow. 

### 3. Duplicate Check with IF Node 
• Before saving the response, an IF node checks for duplicates in Google Sheets. 

• It queries the Google Sheet using the Google Sheets API to check: 

    o Has this user already submitted feedback? 

    o Is the same feedback text already present? 

• This prevents duplicate entries from being stored. 

### 4. Append to Google Sheets 
• If the feedback is not a duplicate, the data is:

    o Appended to a new row in Google Sheets. 

    o Fields like Name, Feedback Text, Issue Type, and Timestamp are stored. 

### 5. Sentiment Analysis with GPT-4o 
• The feedback text is passed again to GPT-4o for sentiment analysis. 

• The AI determines whether the feedback is:

    o  Positive 

    o  Negative

    o  Neutral (optional) 

• A new column Sentiment is added in Google Sheets with the corresponding tag. 

### 6. Feedback Acknowledgement 
• After storing the feedback and analyzing sentiment, the bot replies:

    o “Thanks for your feedback, [Name]! We appreciate your time.” 

    o (Optional) A custom message based on sentiment (e.g., apologizing for negative feedback).

### 📄 Scope & Utility
| Aspect       | Detail                                                   |
|--------------|-----------------------------------------------------------|
| Scalability  | Can be extended to WhatsApp, Instagram, or website chat  |
| Use Cases    | Feedback collection, surveys, customer support           |
| Automation   | Replaces manual entry with intelligent workflows         |

### 🛠️ Tools
- n8n
- Telegram Bot API
- Google Sheets API
- GPT-4o
