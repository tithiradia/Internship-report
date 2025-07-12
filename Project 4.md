# 🔍 Project 4: Local Business Email Scraper

## 📅 Description
This project is an automated system designed to scrape, extract, and log email addresses of local businesses 
based on location-specific Google search queries. The workflow is built using two linked n8n workflows—a 
trigger workflow that sends search queries and a scraper workflow that performs live scraping and 
extraction. 

The system takes custom queries like "andheri east finance site:.in" or "koramangala finance services 
site:.in" and dynamically generates Google Maps search URLs. These pages are scraped for all embedded 
URLs, which are filtered to exclude non-business domains (e.g., Google, Gstatic). From these pages, email 
addresses are extracted using regex, deduplicated, and finally stored in Google Sheets. 
The system also features a loop and delay mechanism to prevent being blocked due to too many requests, 
and to allow for bulk region-wise scraping using pre-filled query lists.


## 🔁 Working Flow
### 1. Query Trigger (Start) 
• The Trigger Workflow manually or automatically starts with a list of search queries (e.g., finance 
services in Andheri, site:.in). 

• These queries are sent into a batching loop, which passes each one sequentially into the scraper system. 

### 2. Scraper Workflow Execution 
• The Scrap Demo Workflow receives the query and dynamically builds a Google Search or Google Maps 
URL for that query. For example: 

https://www.google.com/maps/search/{{query}}

• The resulting HTML data from the search results page is captured via HTTP Request. 

### 3. Extract URLs from Search Result Page 
• A JavaScript code node scans the raw HTML response to extract all valid URLs using a regex URL 
pattern.

• These URLs are filtered using another Filter node to exclude generic or irrelevant domains like:

    o google.com
    
    o gstatic.com 
    
    o schema.org 
    
    o example.com 
    
### 4. Batch Loop + Page Scraping 
• The remaining URLs (likely to be real business or service websites) are looped using the Split In 
Batches node. 

• Each URL is fetched individually using another HTTP Request node, with error handling enabled to 
avoid workflow crashes. 

### 5. Email Extraction via Regex 
• A second JavaScript node processes each page’s HTML content using a regex pattern to extract valid 
business email addresses.

• The regex excludes image file extensions to avoid false positives from embedded image links (e.g., 
.png, .jpg). 

### 6. Email Aggregation, Deduplication, and Splitting 
• Extracted emails are aggregated from each URL, then split into separate rows using the Split Out node. 

• Duplicates are removed using the Remove Duplicates node, ensuring the Google Sheet only receives 
unique email addresses. 

### 7. Save to Google Sheets 
• Final cleaned email records are appended to a Google Sheet titled “scrapping emails”.

• Each row logs a single verified email address that originated from the search query. 


### 📄 Scope & Utility
| Feature            | Value                                          |
|--------------------|------------------------------------------------|
| Local Targeting    | Region-specific search queries                 |
| Lead Generation    | Quick CRM-ready email lists                    |
| Flexible Workflow  | Can integrate with Airtable, Notion, etc.      |


### 🛠️ Tools
- n8n
- JavaScript + Regex
- Google Sheets API
