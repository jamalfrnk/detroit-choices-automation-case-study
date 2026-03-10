# Meeting Notes — Detroit Choices Automation Case Study

This file captures project conversations, stakeholder discussions, and key decisions made during the design and implementation of the automation system. Notes are organized chronologically, oldest first.

---

## Note Template

```
### [Date] — [Meeting Type / Topic]

**Participants:** Malcolm Frank, [other attendees]

**Discussion Topics:**
-

**Business Requirements Identified:**
-

**Operational Problems Identified:**
-

**Automation Opportunities:**
-

**Implementation Decisions:**
-

**Follow-Up Items:**
- [ ]
```

---

## Session Notes

---

### Sun, Nov 16 — Prompt Engineering Workshop / Detroit Choices Project Kickoff

**Participants:** Malcolm Frank, business owner (Detroit Choices)

**Discussion Topics:**
- Introduction to prompt engineering tools and workflow
- Overview of the Detroit Choices brand (ice cream business, Detroit → Atlanta expansion)
- Planning for automation and website build using Replit and Zapier
- Setup of tooling: Replit, Zapier, GitHub, Google Drive

**Business Requirements Identified:**
- Marketing content generation: Instagram captions, headlines, hashtags, flyer copy
- Customer-facing website with order intake
- Automated email responses and social media posting
- Central storage for prompts, assets, and notes in GitHub/Google Drive

**Operational Problems Identified:**
- No existing digital order intake or automation
- Marketing assets produced manually with no systematic process
- No centralized file storage or prompt library

**Automation Opportunities:**
- Zapier for email automation and social media scheduling
- Replit for web application development
- ChatGPT for marketing content generation
- GitHub as prompt and asset version control

**Implementation Decisions:**
- Use Replit for web application hosting and development
- Use Zapier for workflow automation (free tier initially)
- Use GitHub for all code, prompts, and assets
- Create dedicated Google Drive folder for shared files
- Build website with React + Tailwind; Stripe payment integration planned for later phase

**Follow-Up Items:**
- [ ] Create Replit account and start landing page project
- [ ] Set up Zapier account
- [ ] Create GitHub repository for prompts and code
- [ ] Register domain for Detroit Choices or affiliate marketing site
- [ ] Share prompt engineering cheat sheet with client

---

### Thu, Nov 20 — Cluely and GitHub Setup Call

**Participants:** Malcolm Frank, business owner

**Discussion Topics:**
- Tool onboarding: Cluely (AI meeting recorder), GitHub, Google Drive
- GitHub repository setup and file management
- Meeting logistics and scheduling

**Business Requirements Identified:**
- Central repository for all meeting notes, prompts, and code
- Consistent documentation process going forward

**Operational Problems Identified:**
- No existing version-controlled storage for project files
- No consistent meeting recording or notes process

**Implementation Decisions:**
- Create private GitHub repo for course/project materials
- Use Google Drive for shared document exchange
- Use Cluely for automatic meeting note recording
- Schedule regular sessions (minimum twice weekly)

**Follow-Up Items:**
- [ ] Complete GitHub setup; create private repo
- [ ] Upload session notes to repo
- [ ] Set up Cluely on client's device with microphone permissions
- [ ] Send calendar invite for follow-up session

---

### Fri, Nov 21 — Detroit Choices Website Planning

**Participants:** Malcolm Frank, business owner

**Discussion Topics:**
- Detroit Choices website scope and priorities
- Affiliate marketing project planning
- Replit usage and billing management
- Prompt management via GitHub

**Business Requirements Identified:**
- Website pages: Home, Menu, Our Story, Visit Us
- Hero section for brand positioning ("exotic flavors")
- Menu page with placeholder images and cart view
- Checkout flow (payment integration deferred to later phase)
- React + Tailwind responsive frontend

**Operational Problems Identified:**
- No active website for order intake
- No system for affiliate marketing

**Implementation Decisions:**
- Prioritize Detroit Choices website before affiliate marketing work
- Use Replit for development and hosting
- Defer Zapier and OpenAI API integration until site is live
- Store all ChatGPT prompts in GitHub repo for reference

**Follow-Up Items:**
- [ ] Write page-by-page content notes (Home, Menu, Our Story, Visit Us)
- [ ] Save website build prompt to GitHub
- [ ] Set Replit usage alert to avoid unexpected overages

*Note: The session notes from Nov 21 and Dec 11 contain substantially overlapping content, suggesting one set of notes may have been filed under both dates. Both are preserved here.*

---

### Tue, Dec 2 — Separate Client Session (ATV Accessories Project)

**Participants:** Malcolm Frank, separate client (ATV/moped accessories business)

**Discussion Topics:**
- Landing page for ATV, side-by-side, moped, and motorcycle accessories
- QR code linking business card to landing page for conference use
- Future full website scope with product catalog and drop-shipping integration

**Note:** This session covers a separate client project unrelated to Detroit Choices. Included here for completeness as context for Malcolm's concurrent workload during the Detroit Choices build.

**Implementation Decisions:**
- Build lightweight responsive landing page (no e-commerce at this stage)
- Use Replit for hosting; Zapier deferred until full site phase
- Hostinger for domain and hosting

**Follow-Up Items:**
- [ ] Deliver landing page within five days
- [ ] Schedule Friday screen-share review before conference

---

### Thu, Dec 11 — Detroit Choices Website Planning (Continued)

**Participants:** Malcolm Frank, business owner

**Discussion Topics:**
- Detroit Choices website priorities and timeline
- Affiliate marketing parallel planning
- Replit billing and usage management
- GitHub as prompt/code library

**Business Requirements Identified:**
- Website with pages: Home, Menu, Our Story, Visit Us
- React + Tailwind frontend; Stripe integration deferred
- Hero section highlighting brand and "exotic flavors" tagline

**Implementation Decisions:**
- Detroit Choices website takes priority over affiliate marketing
- Replit for development and hosting
- All prompts stored in GitHub repo
- Zapier and OpenAI API deferred until go-live

**Follow-Up Items:**
- [ ] Write page content notes and share before next call
- [ ] Save website prompt to GitHub
- [ ] Set Replit usage alert

---

### Thu, Jan 15 — AI Strategy Session / Automation Architecture

**Participants:** Malcolm Frank, business owner

**Discussion Topics:**
- Full automation architecture overview
- CRM structure and Google Sheets setup
- Zapier webhook configuration
- Replit backend and database planning
- Email template design

**Business Requirements Identified:**
- Order intake form on website sending data to Zapier via webhook
- Google Sheets as the CRM, replacing manual tracking
- Four automated email stages: order received, in progress, delivered, thank you
- Telegram bot for real-time admin alerts (planned)
- Replit backend outputting JSON for webhook consumption

**Operational Problems Identified:**
- Form currently submitting to placeholder endpoint with no backend
- Modal layout issues (form partially off-screen on desktop and mobile)
- Replit database storing orders locally with no CRM connection yet

**Automation Opportunities:**
- Zapier webhook: Replit form → Google Sheets → automated Gmail emails
- Telegram bot for admin order notifications
- Future: scheduled social-media posting agent

**Implementation Decisions:**
- Use Google Sheets as CRM (free, accessible) over Notion
- Zapier Starter plan required for webhook + multi-app triggers
- Replit backend updated to return JSON payload for Zapier consumption
- Email templates stored in Google Drive; Zapier pulls via Drive API
- Set Replit usage cap alert at $5 overage to monitor costs

**Follow-Up Items:**
- [ ] Fix order form modal layout for full visibility and mobile responsiveness
- [ ] Create Google Drive folder structure and email template Docs
- [ ] Set up Google Sheets CRM (columns: ID, name, email, phone, topic, message)
- [ ] Configure Zapier webhook from Replit to Google Sheets
- [ ] Test end-to-end order flow: form → sheet → email
- [ ] Add Replit usage alerts

---

### Thu, Feb 12 — Google Drive and CRM Sheet Setup

**Participants:** Malcolm Frank, business owner

**Discussion Topics:**
- Google Drive folder structure for project operations
- Google Sheets CRM structure and tab organization
- Email template creation in Google Docs
- Zapier planning for status-triggered emails

**Business Requirements Identified:**
- Centralized Google Drive for all operational files
- CRM sheet with tabs: Orders_Live, Orders_Completed, Order_Issues, Order_Status_Definitions
- Email templates for four statuses: Order Confirmed, Order On the Way, Order Delivered, Thank You
- Zapier Zap: status change in Google Sheet → send corresponding Gmail email
- Menu image management: renamed files, JPEG → WebP conversion for web performance

**Operational Problems Identified:**
- CRM structure existed but was partially complete (missing folders and tabs)
- Email template placeholders (e.g., `{customer_name}`, `{order_id}`) needed to be defined
- Zapier setup approximately 40% complete — remaining steps depend on Drive/DB connection

**Implementation Decisions:**
- Google Drive top-level folder: Detroit Choices – Operations
- Sub-folders: CRM Master Google Sheet, Orders Archive, Email Templates, Exports, Backups
- Main sheet renamed: Detroit Choices – CRM Master
- Column headers copied consistently across all tabs
- Dynamic placeholders in email templates use curly-brace format
- Menu images to be stored in a dedicated Google Drive folder

**Follow-Up Items:**
- [ ] Finish creating remaining Drive folders (Exports, Backups)
- [ ] Connect website database to Google Sheets CRM
- [ ] Build and activate Zapier Zaps for status-triggered emails
- [ ] Run full test order: submit → verify sheet entry → confirm email received
- [ ] Rename all menu image files before uploading
- [ ] Schedule next session

---

### Fri, Feb 13 — ChatGPT Agent Setup / Zapier Configuration

**Participants:** Malcolm Frank, business owner

**Discussion Topics:**
- Zapier plan upgrade and configuration
- Webhook field mapping corrections
- Replit backend output format fix
- Email template Zap testing

**Business Requirements Identified:**
- Single multi-step Zapier Zap combining all four email triggers (vs. four separate Zaps)
- Webhook must send structured JSON with correct dynamic variable syntax
- Replit backend must return JSON for webhook consumption

**Operational Problems Identified:**
- Zapier initial field mappings used literal column names instead of dynamic JSON variables
- Replit backend outputting plain text instead of JSON payload
- Some Zapier Zaps remaining in draft state after mapping changes
- Gmail connection required re-authentication before email delivery worked

**Implementation Decisions:**
- Upgrade to Zapier Starter plan ($20/month) — free tier insufficient for webhook + multi-app triggers
- Correct field mappings to use proper JSON variable syntax (e.g., `{{order_id}}`)
- Update Replit backend to output JSON for Zapier webhook intake
- Combine four email actions into one multi-step Zap

**Follow-Up Items:**
- [ ] Publish both Zapier Zaps (CRM update + email automation)
- [ ] Verify all four email steps deliver correctly
- [ ] Run full end-to-end test order
- [ ] Set calendar reminder to evaluate Zapier plan after testing phase
- [ ] Set up Telegram bot linked to Zapier for real-time order alerts

---

### Wed, Feb 18 — Telegram Bot Integration Planning

**Participants:** Malcolm Frank, business owner

**Discussion Topics:**
- Telegram bot architecture decision
- Integration approach: Zapier-only vs. custom backend
- Order status flow and email trigger alignment
- Google Sheets column alignment with bot payload

**Business Requirements Identified:**
- Admin receives Telegram notification on each new order with order details
- Admin can use inline buttons in Telegram to update order status
- Status button press triggers Zapier to update Google Sheet and send next customer email
- Pickup-only model: only "order received" and "order ready/pickup" statuses required at this stage
- "Delivered" status step omitted for current phase

**Operational Problems Identified:**
- Google Sheets column names not yet verified against Telegram bot payload fields
- No backend server configured for interactive Telegram callbacks

**Implementation Decisions:**
- **Option D selected:** Zapier handles immediate Telegram alerts; custom backend handles interactive button callbacks only
- No separate backend server needed for the initial alert — reduces cost and complexity
- Bot sends "order received" message with order details to owner's Telegram chat
- Inline button press triggers Zapier to update sheet status and send next customer email
- Align Google Sheet columns: order_id, status, timestamp, etc.

**Email and Notification Flow Confirmed:**
- Order Received → Telegram alert to admin + customer email "order received"
- Order Pending / Preparing → customer email (no fixed time frame)
- Order Ready / Pickup → new email template with pickup instructions
- Thank You → final email with Google review or survey link

**Follow-Up Items:**
- [ ] Create Telegram bot via BotFather; obtain API token
- [ ] Add bot's chat ID to Zapier Telegram action
- [ ] Configure five-step Zapier flow: webhook → sheet → Telegram alert → pending email → ready email
- [ ] Align Google Sheet columns with bot payload
- [ ] Prepare updated email template for "order ready / pickup"
- [ ] Test end-to-end flow: place test order → verify sheet → Telegram alert → button action → email

---

### Tue, Feb 24 — Telegram Bot Integration Completion

**Participants:** Malcolm Frank, business owner

**Discussion Topics:**
- Completion of Telegram bot setup
- Secret naming conventions in Replit
- End-to-end test results
- Zapier Zap publishing and Gmail re-authentication

**Business Requirements Identified:**
- All Replit secrets to use all-caps naming (e.g., `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`)
- Bot must be added as admin in the "Detroit Choices Admin" Telegram group
- Zapier Zaps must be published and Gmail connection verified before go-live

**Operational Problems Identified:**
- Telegram test initially failed due to incorrect secret case sensitivity in Replit
- Email not received until Zapier Gmail connection was re-authenticated
- Health endpoint worked; Telegram test failed until secret naming was corrected

**Implementation Decisions:**
- Update secret names to all-caps: `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`
- Delete unused `server.js` file; `telegram.ts` is the only active implementation
- Bot added as admin in the Detroit Choices Admin group
- Retrieved group chat ID (negative integer format) via API call; stored as Replit secret

**Test Results:**
- Health endpoint: passing
- Telegram notification: passing (after secret fix)
- Order appears in Google Sheet: confirmed
- Telegram notification arrives on order submission: confirmed
- Email delivery: confirmed after Gmail re-authentication

**Follow-Up Items:**
- [ ] Implement inline button callbacks in Telegram (Mark Pending, Mark Delivered, Ready for Pickup)
- [ ] Wire button actions to Zapier to trigger client status emails
- [ ] Schedule next session for payment integration and admin dashboard
- [ ] Publish Zapier Zaps and verify final Gmail connection
- [ ] Run full end-to-end order test: website → Zapier → Telegram → email

---

<!-- Add new session notes below in chronological order -->
