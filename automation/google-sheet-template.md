# Google Sheet Setup Guide
# ScaleUp Genius Hub — Workshop Registration Tracker

## Step 1: Create a new Google Sheet
Go to sheets.google.com → click the + (Blank spreadsheet) → rename it "Workshop Registrations"

## Step 2: Set up the header row (Row 1)
Create these exact column headers in Row 1:

| A | B | C | D | E | F | G | H | I |
|---|---|---|---|---|---|---|---|---|
| First Name | Last Name | Email | Phone | Workshop Date | Workshop Time | Zoom Link | Status | Sent At |

## Step 3: Set up data validation for Column H (Status)
- Click Column H header to select the whole column
- Go to Data → Data validation
- Criteria: List of items → type: PENDING, SENT, FAILED
- Default for new rows will be blank (Make fills it with SENT after sending)

## Step 4: Add a default Zoom link formula (optional)
If your Zoom link is always the same for all workshops, put this in G2 and drag down:
="https://zoom.us/j/YOUR_ZOOM_ID"
Replace YOUR_ZOOM_ID with your actual Zoom meeting ID.

## Step 5: Get your Sheet ID
Your Sheet ID is in the URL:
https://docs.google.com/spreadsheets/d/ >>> THIS PART <<< /edit
Copy this and paste it into the Make scenario blueprint JSON where it says PASTE_YOUR_GOOGLE_SHEET_ID_HERE

## Step 6: Connect your Google Form (optional but recommended)
Instead of manually entering registrations, connect a Google Form:
- Go to Google Forms → create your registration form
- Fields: First Name, Last Name, Email, Phone Number
- Click Responses tab → Link to Sheets → select your "Workshop Registrations" sheet
- Map the form fields to the correct columns
- Workshop Date, Time, and Zoom Link you can either:
  a) Add as hidden fields pre-filled in the form URL, OR
  b) Fill them manually in the Sheet for each workshop session

## Example data row:
| Priya | Sharma | priya@gmail.com | 919876543210 | Saturday 5 July 2025 | 11:00 AM | https://zoom.us/j/123456789 | | |

## IMPORTANT: Phone number format
Phone must include country code WITHOUT the + sign
India: 91XXXXXXXXXX (e.g. 919876543210)
UAE: 971XXXXXXXXX
Singapore: 65XXXXXXXX

## What happens when Make runs:
1. Make sees a new row where Column H is empty
2. Sends WhatsApp to Column D phone number
3. Sends email to Column C email
4. Adds contact to Brevo automation list (triggers the 7-email sequence)
5. Writes SENT in Column H and the timestamp in Column I
6. That row is never processed again
