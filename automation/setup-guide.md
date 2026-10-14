# Complete Setup Guide — ScaleUp Genius Hub Automation
# One-time setup: ~45 minutes total
# After setup: fully automatic forever

---

## What this system does (once set up)

When someone registers for your workshop:
→ WhatsApp confirmation sent instantly
→ Email confirmation sent instantly
→ They're added to a 7-email sequence automatically
→ 24hr WhatsApp reminder sent the day before
→ 1hr WhatsApp reminder sent an hour before
→ Post-workshop pitch email sent 3 hours after
→ Follow-ups sent on day 2, day 5, and day 12

You do nothing after setup.

---

## STEP 1 — Create your Google Sheet (5 mins)
See: google-sheet-template.md

1. Go to sheets.google.com
2. Create blank sheet, name it "Workshop Registrations"
3. Add headers in Row 1 exactly as shown in the template file
4. Copy the Sheet ID from the URL bar

---

## STEP 2 — Set up Brevo email (10 mins)

1. Go to brevo.com → Sign Up Free
2. Verify your sender email (Settings → Senders and IP → Add Sender)
3. Get your API key: Settings → API Keys → Generate a new key → copy it
4. Create 7 email templates (copy from email-sequence.md):
   - Go to Email → Templates → New Template
   - Create each email, name them Email_1 through Email_7
   - Save and activate each one
   - Note the Template ID number shown after saving
5. Create an automation list:
   - Go to Contacts → Lists → Create a list
   - Name it "Workshop Registrants"
   - Note the List ID number

---

## STEP 3 — Set up the 7-email automation in Brevo (10 mins)

1. Go to Automations → Create a workflow
2. Trigger: Contact added to list → select "Workshop Registrants"
3. Add Email step: send Email_1 immediately
4. Add Wait step: 1 day
5. Add Email step: send Email_2
6. Add Wait step: 1 day
7. Add Email step: send Email_3
(Continue this pattern through all 7 emails with the wait times from email-sequence.md)
8. Save and activate the workflow

---

## STEP 4 — Set up Interakt WhatsApp (10 mins)

1. Go to interakt.shop → Start Free Trial
2. Connect your WhatsApp business number (Settings → WhatsApp Account)
3. Get your API key (Settings → API → copy the key)
4. Submit all 5 WhatsApp templates (copy from whatsapp-templates.md):
   - Templates → Create Template
   - Submit each one, wait for Meta approval (5–30 mins)
   - You cannot send until approved — do this first

---

## STEP 5 — Build the Make scenario (15 mins)

1. Go to make.com → Sign Up Free
2. Create a new scenario
3. Add Module 1: Google Sheets → Watch new rows
   - Connect your Google account
   - Select your "Workshop Registrations" sheet
   - Set limit to 1
4. Add Module 2: HTTP → Make a request
   - Method: POST
   - URL: https://api.interakt.ai/v1/public/message/
   - Add header: Authorization = Basic YOUR_INTERAKT_API_KEY
   - Add header: Content-Type = application/json
   - Copy the body JSON from make-scenario-blueprint.json (id: 3)
   - Map the phone and name fields from Module 1
5. Add Module 3: Brevo → Send a transactional email
   - Connect with your API key
   - Template ID: paste Email_1 template ID
   - Map email and name from Module 1
6. Add Module 4: Brevo → Add contact to list
   - List ID: paste your "Workshop Registrants" list ID
   - Map email and all contact fields from Module 1
7. Add Module 5: Google Sheets → Update a row
   - Same sheet
   - Row: map the row number from Module 1
   - Column H: type SENT
   - Column I: {{now}}
8. Set scheduling: click clock icon → every 15 minutes
9. Turn the scenario ON

---

## STEP 6 — Create reminder scenarios (5 mins)

Create Scenario 2: "24hr Workshop Reminder"
- Trigger: Google Sheets → Search rows where Workshop Date = tomorrow AND Status = SENT
- Action: HTTP POST to Interakt with template "workshop_reminder_24hr"
- Schedule: Run daily at 9:00 AM

Create Scenario 3: "1hr Workshop Reminder"
- Trigger: Google Sheets → Search rows where Workshop Date = today AND Status = SENT
- Action: HTTP POST to Interakt with template "workshop_reminder_1hr"
- Schedule: Run daily, check every hour

---

## STEP 7 — Test everything (5 mins)

1. Add a test row to your Google Sheet with YOUR name, email, and phone
2. In Make, click "Run once" on Scenario 1
3. Check your WhatsApp — you should receive the confirmation message
4. Check your email — you should receive the confirmation email
5. Check Brevo → Contacts — you should appear in "Workshop Registrants"
6. Check your Google Sheet — Column H should now say "SENT"

If something fails:
- Click the red bubble in Make to see the error message
- Most common fix: wrong API key or phone number format (must include country code)

---

## STEP 8 — Go live

Share your Google Form link on your landing page and Instagram.
Every new submission = fully automated onboarding. You do nothing.

---

## Monthly cost estimate

| Tool | Plan | Cost |
|------|------|------|
| Make | Free (1,000 operations/mo) | ₹0 |
| Brevo | Free (300 emails/day) | ₹0 |
| Interakt | Starter | ~₹999/mo |
| Google Sheets | Free | ₹0 |
| **Total** | | **~₹999/month** |

Upgrade Make to Starter (₹1,800/mo) only when you exceed 1,000 operations.

---

## Files in this folder

| File | Purpose |
|------|---------|
| whatsapp-templates.md | 5 WhatsApp message templates — copy-paste into Interakt |
| email-sequence.md | 7 emails — copy-paste into Brevo |
| make-scenario-blueprint.json | Full automation logic — reference when building in Make |
| google-sheet-template.md | Sheet structure + setup instructions |
| setup-guide.md | This file — the master guide |
