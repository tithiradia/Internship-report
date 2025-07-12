# 🏛️ Project 6: GeM Tender Scraper with PDF Parsing

## 📅 Description
The GeM Tender Scraper is a specialized automation tool designed to fetch and filter ongoing tenders listed on 
India’s Government eMarketplace (GeM) platform. The script performs a paginated crawl of live tenders, 
parses metadata such as bid ID, bid number, category, quantity, ministry, and deadline, and filters them by 
district relevance using an Excel sheet of Indian districts. 

It goes one step further by downloading associated tender PDFs, using logic to generate download links based 
on bid type. These PDFs are processed using Tabula-Py to extract detailed information such as the delivery 
location or item specifications mentioned within the documents. By scanning the PDF contents for any match 
with the predefined district names, the system identifies and flags tenders relevant to specific local regions, 
providing massive value to local businesses that need to track only geographically relevant opportunities. 
The scraper handles retries, timeouts, and errors, and it stores the clean results in a local .json file. This makes 
it highly suitable for use by local SMEs, government liaisons, or consulting firms who need to regularly monitor 
GeM for local business leads. 

## 🔁 Working Flow
### 1. Load District Data 
• Reads a cleaned Excel file: indian_districts_cleaned_final.xlsx. 

• Extracts all district names into a list, used later for city-level filtering of PDF data. 

### 2. API Session Setup 
• The script requires manual input of session tokens (csrf_token and ci_session) copied from an 
authenticated GeM session.

• These tokens are used as cookies and form headers to send authorized POST requests to the GeM API.

### 3. Fetch Tender Listings (Paginated) 
• Sends POST requests to: https://bidplus.gem.gov.in/all-bids-data. 

• Custom payload filters tenders with: 

    o Status: ongoing_bids 
    
    o Type: all
    
    o Sort order: Bid-Start-Date-Oldest 
    
• Bids are fetched page-by-page in a while loop until no more new tenders are found. 

• Each page is parsed using the extract_bid_data() function.

### 4. Extract Metadata from JSON 
Each bid is parsed to extract: 

Includes bid_id, bid_number, start_date, end_date, category, quantity, ministry, department, bid_url, and 
download_url. These cover core metadata like bid identity, timeline, product/service type, issuing authority, 
and relevant access links. 

The script ignores expired tenders by comparing the tender’s end date to the current UTC timestamp.

### 5. PDF Link Generation 
• Two types of download links are dynamically constructed based on bid pattern:

    o If /R/ is present in bid_number, use https://bidplus.gem.gov.in/showradocumentPdf/{bid_id} 
    
    o Else use https://bidplus.gem.gov.in/showbidDocument/{bid_id} 
    
### 6. Retry Logic & Error Handling 
• Each page fetch includes retry handling (up to 3 retries). 

• On continuous failure, the script skips the current page and moves on.

• Once all bids are processed or no new bids are found, results are saved to a local file: 
historical_bids_clean.json. 

### 📄 Scope & Utility
| Use Case                       | Value                                                     |
|--------------------------------|-----------------------------------------------------------|
| Local Businesses               | Helps identify tenders geographically relevant to them    |
| Consulting Firms               | Provides filtered tender datasets for clients             |
| Government Contractors         | Enables faster monitoring and decision-making             |
| Automated Data Archival        | Saves tender history for audit, tracking, or analysis     |
| Extendable with Email Alerts   | Can be expanded to send daily filtered tenders via email  |

### 🛠️ Tools
- Python (requests, retry, pandas)
- Tabula-py
- Excel (districts list)
- JSON
