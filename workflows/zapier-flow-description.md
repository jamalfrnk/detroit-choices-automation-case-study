# Zapier Flow Description — Detroit Choices Automation Case Study

This document describes the Zapier automation workflow in detail, covering each step from webhook intake through post-pickup follow-up.

---

## Overview

The Detroit Choices automation system uses two Zapier Zaps, both triggered by webhook events from the Replit backend:

1. **Order Intake Zap** — triggered by the order intake webhook after Stripe payment completes
2. **Status Update Zap** — triggered by status webhook events fired by the backend when the admin acts through the Telegram admin dashboard

Neither Zap is triggered by Google Sheets row changes. Google Sheets is an auto-synced downstream view — not a trigger source.

---

## Zap 1: Order Intake Workflow

### Trigger: Order Intake Webhook

**Zapier App:** Webhooks by Zapier
**Trigger Event:** Catch Hook

The Zap is triggered when the Replit backend fires a POST webhook after Stripe payment is confirmed and the order is written to PostgreSQL.

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
- `status` (value: `new`)
- `stripe_payment_id`

See `workflows/sample-webhook-payload.json` for a complete example.

---

### Action 1: Create Google Sheets CRM Row

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
| Status | `status` (value: `new`) |

This record is a downstream view. The admin does not edit it directly.

---

### Action 2: Send Telegram Admin Alert

**Zapier App:** Telegram
**Action Event:** Send Message

A formatted alert is sent to the Telegram admin dashboard immediately after the CRM row is created. The message includes the order details and inline action buttons configured in the Telegram bot.

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

The admin manages all fulfillment actions by pressing the inline buttons in this message (Order Being Processed / Order Ready for Pickup / Order Picked Up).

---

### Action 3: Send Customer Order Confirmation Email

**Zapier App:** Email
**Action Event:** Send Outbound Email

An automated confirmation email is sent to the customer using the email address from the webhook payload.

**Email fields:**
- **To:** `email` from webhook payload
- **Subject:** `Your Detroit Choices Order Has Been Received`
- **Body:** Order acknowledgment with order details and notification that updates will follow

---

## Zap 2: Status Update Workflow

### Trigger: Status Update Webhook from Replit Backend

**Zapier App:** Webhooks by Zapier
**Trigger Event:** Catch Hook

This Zap is triggered when the Replit backend fires a status update webhook. This happens each time the admin clicks an inline button in the Telegram admin dashboard, which calls back to the backend, updates PostgreSQL, and then fires the webhook.

**Incoming payload fields:**
- `order_id`
- `customer_name`
- `email`
- `product`
- `status` (updated value: `processing`, `ready_for_pickup`, or `picked_up`)
- `status_updated_at`
- `picked_up_at` (present only when `status = 'picked_up'`)

---

### Action 1: Auto-Update Google Sheets CRM Row

**Zapier App:** Google Sheets
**Action Event:** Update Spreadsheet Row

Zapier finds the existing CRM row by `order_id` and updates the `Status` column to the new value from the webhook payload.

---

### Action 2: Route by Status Value

Zapier uses a **Paths** step to evaluate the `status` field and route to the appropriate action.

#### Path A — Status: `processing`

**Condition:** `status` equals `processing`
**Action:** Send Outbound Email to customer
**Subject:** `Your Detroit Choices Order Is Being Processed`
**Body:** Confirmation that the team has received the order and is processing it

#### Path B — Status: `ready_for_pickup`

**Condition:** `status` equals `ready_for_pickup`
**Action:** Send Outbound Email to customer
**Subject:** `Your Detroit Choices Order Is Ready for Pickup`
**Body:** Pickup notification with relevant instructions

#### Path C — Status: `picked_up`

**Condition:** `status` equals `picked_up`

**Action Step 1:** Zapier Delay
- **Duration:** 24 hours
- Gives the customer time to experience the product before receiving a feedback request

**Action Step 2:** Send Outbound Email to customer
- **Subject:** `Thank You for Your Detroit Choices Order`
- **Body:** Thank-you message with a link to a feedback form or review platform

---

## Business Value

The two-Zap design separates the order intake workflow from the status update workflow, keeping each flow independent and maintainable.

**Engineering value:**
- Both Zaps are webhook-triggered — fully reactive, no polling
- Status automation is driven by backend events, not CRM row changes — clean separation of concerns
- Each Zap can be modified or debugged independently
- Zapier's task history provides a complete audit log of every action

**Business value:**
- Admin has one control surface (Telegram) for all fulfillment actions
- Google Sheets CRM is always accurate without manual updates
- Customers receive consistent, timely communication at every stage
- Post-pickup feedback collection is automated and timed correctly — 24 hours after pickup, not immediately
