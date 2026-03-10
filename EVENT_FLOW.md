# Event Flow — Detroit Choices Automation Case Study

This document describes the complete sequence of events from customer order submission through post-pickup follow-up.

---

## Event Sequence

### Event 1 — Order Submission

**Trigger:** Customer completes and submits the order form on the Detroit Choices website.

**What happens:**
- The customer fills in their name, contact information, product selection, quantity, and any special notes
- The form is submitted to the Replit-hosted web application backend

---

### Event 2 — Database Write

**Trigger:** Backend receives the form submission.

**What happens:**
- The backend writes a new row to the PostgreSQL `orders` table
- A unique `order_id` is assigned (e.g., `DC-1001`)
- `status` is set to `New`
- `submitted_at` timestamp is recorded

The database record is now the system of record for this order.

---

### Event 3 — Webhook Trigger

**Trigger:** Database write completes.

**What happens:**
- The backend fires a webhook event containing the structured order data as a JSON payload
- The payload includes all core order fields: `order_id`, `customer_name`, `phone`, `email`, `product`, `quantity`, `special_notes`, `order_source`, `submitted_at`, `status`
- The webhook is sent to the Zapier intake URL

See `workflows/sample-webhook-payload.json` for the exact payload structure.

---

### Event 4 — Zapier Receives Webhook

**Trigger:** Zapier Webhook trigger fires.

**What happens:**
- Zapier receives and parses the incoming JSON payload
- The automation workflow begins executing the action sequence

---

### Event 5 — CRM Record Created

**Trigger:** Zapier processes the webhook payload.

**What happens:**
- Zapier creates a new row in the Google Sheets CRM document
- The row captures all key order fields
- The `Status` column is set to `New`
- This record becomes the admin-facing operational view of the order

---

### Event 6 — Internal Telegram Alert Sent

**Trigger:** CRM record created.

**What happens:**
- Zapier sends a message via the Telegram Admin Bot
- The message notifies the administrator of the new order with key details (customer name, product, contact info)
- The admin receives real-time notification without needing to check email or a dashboard

---

### Event 7 — Internal Email Notification Sent

**Trigger:** Zapier continues workflow execution.

**What happens:**
- Zapier sends an internal email notification to the business owner or administrator
- The email includes the full order details

---

### Event 8 — Customer Order Confirmation Email Sent

**Trigger:** Zapier continues workflow execution.

**What happens:**
- Zapier sends an automated order confirmation email to the customer
- The email acknowledges receipt of the order and provides order details
- The customer is informed that the team will follow up as the order progresses

---

### Event 9 — Admin Updates Order Status

**Trigger:** Manual action by the administrator in the Google Sheets CRM.

**What happens:**
- The admin updates the `Status` field in the CRM row
- This change triggers Zapier to send a customer status update email

Status values that trigger customer emails:
- `Processed`
- `In Preparation`
- `Ready for Pickup`

---

### Event 10 — Customer Status Email Sent

**Trigger:** Google Sheets `Status` field is updated by admin.

**What happens:**
- Zapier detects the status change in Google Sheets
- Zapier sends the appropriate customer status email based on the new status value:
  - **Processed:** Confirms the order has been received and is being processed
  - **In Preparation:** Notifies the customer that their order is being prepared
  - **Ready for Pickup:** Notifies the customer their order is ready

The customer receives one email per status change as their order progresses through the lifecycle.

---

### Event 11 — Pickup Confirmed

**Trigger:** Admin marks the order as `Picked Up` in the CRM.

**What happens:**
- Zapier detects the status change to `Picked Up`
- A 24–48 hour delay step is initiated
- No immediate customer email is sent at this stage

---

### Event 12 — Thank-You Email + Feedback Request Sent

**Trigger:** 24–48 hour delay completes after pickup confirmation.

**What happens:**
- Zapier sends a thank-you email to the customer
- The email includes a link to a feedback form or review platform
- This closes the customer communication loop and initiates the post-order feedback process

---

## Summary Diagram

```
[Customer] → Order Form → Replit App → PostgreSQL → Webhook
                                                        ↓
                                                    Zapier
                                                 ↙   ↙    ↘    ↘
                                        Google  Telegram Internal Customer
                                        Sheets   Alert   Email  Confirm
                                          ↓
                                    Admin Updates Status
                                          ↓
                              Status Emails (Processed / In Prep / Ready)
                                          ↓
                               Pickup Confirmed → 24–48hr Delay
                                          ↓
                              Thank-You + Feedback Email → Customer
```

For the full Mermaid flowchart, see [architecture/system-architecture.md](architecture/system-architecture.md).
