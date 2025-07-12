# 🏨 Project 5: Multi-Hotel Price Scraper (FastAPI + BrowseAI)

## 📅 Description
The goal of this project is to automatically scrape hotel room pricing and plan details from multiple online 
booking platforms (like Booking.com, Agoda, Goibibo, MMT, etc.), generate structured Excel reports from 
the extracted data, and email them to the user — all triggered via a FastAPI backend. 

This system enables users to monitor and compare hotel prices across different platforms in one go, saving 
time and manual effort. 

## 🔁 Working Flow
### 1. FastAPI Server 
• Provides a single GET endpoint /run-selected/

• Accepts:

    o checkin: check-in date 
    
    o checkout: check-out date 
    
    o hotels: list of selected platforms (e.g. booking, agoda, browseai) 
    
• Based on selection, calls respective scripts or functions 

### 2. Scraper Scripts 
• Each platform (e.g. Booking.com, Agoda) has its own .py script using BeautifulSoup or Selenium 

• Invoked using subprocess.run(...) from FastAPI 

• Output is saved as a .json file, e.g. booking_room_data.json 

### 3. BrowseAI Integration 
• Used for platforms like Goibibo & MMT (which rely on JavaScript-heavy content) 

• Each robot is triggered using a REST API call with: 

    o Robot ID 
    
    o Hotel URL 
    
• After a few seconds (waiting for task completion), data is retrieved via BrowseAI API

• Data is parsed using regex into a structured format

### 4. JSON to Excel Conversion 
• All JSON outputs are loaded and flattened into rows 

• Each plan under a room is split into a separate row 

• Excel column format: 

| Hotel Name | Room Type | Plan | Price | 

• Output files are named: 

    o booking_room_data.xlsx 
    
    o agoda_room_data.xlsx 
    
    o goibibo.xlsx 
    etc. 
    
### 5. Email Sending 
• Uses Gmail SMTP (port 465 with SSL) 

• Sends all .xlsx files from selected hotels

• Authentication is handled via Gmail App Password (not actual password) 

### 6. HTML Form Interface 
• A local HTML file lets users: 

    o Pick check-in and check-out dates 
    
    o Select one or more hotel platforms via checkboxes
    
• Submits form to FastAPI server's /run-selected/ endpoint 

• Server handles the request and returns status

### 📄 Scope & Utility
| Feature                            | Value                                                                         |
|------------------------------------|-------------------------------------------------------------------------------|
| Automated Hotel Scraping           | Eliminates manual comparison of prices across hotel booking platforms         |
| Multi-Platform Coverage            | Supports major hotel platforms (Booking, Agoda, Goibibo, MMT, etc.)           |
| Date-Specific Input                | Enables users to scrape real-time data for any check-in/check-out dates       |
| Targeted Hotel Selection           | Allows users to select only desired platforms for faster, focused scraping    |
| Excel-Based Output                 | Generates clean, structured Excel reports ready for review or analysis        |
| Auto Email Delivery                | Automatically sends compiled Excel files to a predefined recipient            |
| BrowseAI Cloud Integration         |  Handles JS-heavy websites(e.g., Goibibo & MMT) that can't be scraped easily  |
| Modular Script Execution           | Easily add/remove scraping modules for different hotel sources                |

### 🛠️ Tools
- FastAPI
- Python (pandas, BeautifulSoup, Selenium)
- BrowseAI
- Gmail SMTP
- HTML UI
