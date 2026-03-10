# Zapier Flow Description — Detroit Choices Automation Case Study

This document describes the Zapier automation workflow in detail, covering each step from webhook intake through post-pickup follow-up.

---

## Overview

The Detroit Choices automation system relies on two Zapier Zaps:

1. **Order Intake Zap** — triggered by incoming webhook, handles new order processing
2. **Status Update Zap** — triggered by Google Sheets row updates, handles customer status emails

---

## Zap 1: Order Intake Workflow

### Trigger: Webhook Intake

**Zapier App:** Webhooks by Zapier
**Trigger Event:** Catch Hook

The Zapier Zap is triggered when the Replit backend fires a POST webhook containing the structured order payload.

**Incoming payload fields:**
- `order_id`
- `customer_name`
- `phone`
- `email`
- `product`
- `quantity`
- `special_notes`
- `order_source`
- `submitted_at`
- `status`

See `workflows/sample-webhook-payload.json` for a complete example.

---

### Action 1: Create Google Sheets CRM Entry

**Zapier App:** Google Sheets
**Action Event:** Create Spreadsheet Row

Zapier maps the incoming webhook fields to the corresponding columns in the Google Sheets CRM.

**Column mapping:**

| Google Sheets Column | Webhook Field |
|---|---|
| Order ID | `order_id` |
| Customer Name | `customer_name` |
| Phone | `phone` |
| Email | `email` |
| Product | `product` |
| Quantity | `quantity` |
| Special Notes | `special_notes` |
| Order Source | `order_source` |
| Submitted At | `submitted_at` |
| Status | `status` (value: `New`) |

---

### Action 2: Send Telegram Admin Alert

**Zapier App:** Telegram
**Action Event:** Send Message

A formatted alert is sent to the admin's Telegram account or group immediately after the CRM entry is created.

**Message format:**
```
New Order Received
Order ID: DC-1001
Customer: Jane Doe
Product: Ice Cream Cake (x1)
Phone: (404) 555-0199
Email: jane@example.com
Notes: Add candles and chocolate drizzle
```

---

### Action 3: Send Internal Email Notification

**Zapier App:** Email
**Action Event:** Send Outbound Email

An internal notification is sent to the business owner or admin email address with full order details.

**Email fields:**
- **To:** Business owner email address
- **Subject:** `New Detroit Choices Order: [order_id]`
- **Body:** Full order details including customer name, product, quantity, contact info, and special notes

---

### Action 4: Send Customer Order Confirmation Email

**Zapier App:** Email
**Action Event:** Send Outbound Email

An automated confirmation email is sent to the customer using the email address from the webhook payload.

**Email fields:**
- **To:** `email` from webhook payload
- **Subject:** `Your Detroit Choices Order Has Been Received`
- **Body:** Order acknowledgment with order details, and notification that updates will follow as the order progresses

---

## Zap 2: Status Update Workflow

### Trigger: Google Sheets Row Updated

**Zapier App:** Google Sheets
**Trigger Event:** New or Updated Spreadsheet Row

This Zap monitors the CRM sheet for any row where the `Status` column is updated by the admin.

---

### Action 5: Route by Status Value (Paths or Filter)

Zapier uses a **Paths** step (or sequential Filter steps) to evaluate the updated `Status` field and route to the appropriate email action.

#### Path A — Status: Processed

**Condition:** `Status` equals `Processed`
**Action:** Send Outbound Email to customer
**Subject:** `Your Detroit Choices Order Has Been Processed`
**Body:** Confirmation that the order has been received and is being processed

#### Path B — Status: In Preparation

**Condition:** `Status` equals `In Preparation`
**Action:** Send Outbound Email to customer
**Subject:** `Your Detroit Choices Order Is Being Prepared`
**Body:** Notification that the team is actively preparing the order

#### Path C — Status: Ready for Pickup

**Condition:** `Status` equals `Ready for Pickup`
**Action:** Send Outbound Email to customer
**Subject:** `Your Detroit Choices Order Is Ready for Pickup`
**Body:** Pickup notification with any relevant location or timing information

#### Path D — Status: Picked Up

**Condition:** `Status` equals `Picked Up`

**Action Step 1:** Zapier Delay
- **Duration:** 24–48 hours
- This delay gives the customer time to experience the product before receiving a feedback request

**Action Step 2:** Send Outbound Email to customer
**Subject:** `Thank You for Your Detroit Choices Order`
**Body:** Thank-you message including a link to a feedback form or review platform

---

## Business Value

The two-Zap design separates the order intake workflow from the ongoing status management workflow. This makes each Zap simpler to maintain and debug independently.

**Engineering value:**
- Event-driven design — no polling required
- Stateless webhook intake — Zapier processes each order independently
- Modular — each action can be modified without affecting the others
- Auditable — Zapier's task history provides a log of every action taken

**Business value:**
- Zero manual email writing for routine communications
- Admin can focus on fulfillment, not coordination
- Customers receive consistent, timely communication at every stage
- Feedback collection is automated and happens at the right time (post-experience, not immediately)
