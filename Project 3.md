# 📇 Project 3: Visiting Card Automation Bot

## 📅 Description
The Visiting Card Automation Bot is a smart AI-powered assistant that digitizes and organizes visiting card 
information sent through Telegram. This unified system supports both single-card uploads and bulk-card 
images, making it ideal for everyday networking and post-event contact management. Users simply send a 
photo of one or many cards, and the bot intelligently extracts structured data including name, designation, 
email, phone number, company, and address using OpenAI GPT-4o Vision. 

In single-card mode, the bot processes one card at a time, checks for duplicates using phone or email, and 
adds new entries to Google Sheets. A WhatsApp link is automatically generated for each contact, enabling 
quick follow-ups directly from the sheet. 

In multi-card mode, the system detects and extracts multiple card blocks from a single image. It then applies 
loop logic in n8n to split the array of contacts and log each one individually. Additional features include 
tagging sales categories, assigning user quality scores, and even providing catalog recommendations based on 
the card’s company or domain—for example, suggesting automation tools for SaaS company contacts. 
This entire process is seamless, automated, and user-friendly—allowing sales teams, recruiters, or event   
participants to build a contact database rapidly and efficiently without tedious manual entry.

## 🔁 Working Flow
### 1. Telegram Input 
• User sends a photo (either a single card or a group of cards) to the Telegram bot. 

• The image is captured and passed into the n8n workflow.

### 2. Data Extraction via GPT-4o Vision 
• The image is analyzed using OpenAI GPT-4o’s vision model. 

• In single mode, one structured set of contact details is extracted.

• In bulk mode, an array of contact objects is returned—each representing one business card in the image. 
Fields extracted per contact:

    • Name 
    • Job Title 
    • Phone Number 
    • Email Address 
    • Company 
    • Address 

### 3. Loop & Split Logic (for Multi-Card) 
• If multiple cards are detected:

    o The contact array is looped over using n8n’s Split In Batches node. 
    
    o Each contact is treated as an individual record for storage and checking. 
    
### 4. Duplicate Check 
• Each new contact is checked against existing records in Google Sheets. 

• The system prevents duplicates using phone numbers or email as keys.

### 5. Storage in Google Sheets 
• Each contact (whether from a single or multi-card upload) is saved in a centralized Google Sheet. 

• The sheet includes timestamps, source user info, and contact fields. 

## 📄 Scope & Utility
| Feature                      | Benefit                                                          |
|------------------------------|------------------------------------------------------------------|
| Single & Multi-card Parsing  | Works for everyday and event use cases                           |
| AI-Based OCR + Logic         | Handles different fonts, languages, and layouts                  |
| Fast Follow-up               | WhatsApp links and lead scores enable faster decision-making     |
| CRM-Ready Data               | Structured, cleaned, deduplicated records in Sheets              |
| Scalable & Adaptable         |Can be extended to sync with CRMs like HubSpot, Zoho, Salesforce  |

### 🛠️ Tools
- GPT-4o Vision
- Telegram Bot API
- n8n
- Google Sheets API
