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

### Event 2 — Stripe Payment

**Trigger:** Backend initiates a Stripe checkout session.

**What happens:**
- The customer completes payment through Stripe
- Stripe confirms payment to the Replit backend
- The backend proceeds to write the order to PostgreSQL only after payment is confirmed
- The `stripe_payment_id` is stored alongside the order record

---

### Event 3 — Database Write

**Trigger:** Stripe payment confirmed.

**What happens:**
- The backend writes a new row to the PostgreSQL `orders` table
- A unique `order_id` is assigned (e.g., `DC-1001`)
- `status` is set to `new`
- `submitted_at` timestamp is recorded

PostgreSQL is the system of record for this order.

---

### Event 4 — Order Intake Webhook

**Trigger:** Database write completes.

**What happens:**
- The backend fires an order intake webhook to Zapier containing the structured order data as a JSON payload
- The payload includes all core order fields: `order_id`, `customer_name`, `phone`, `email`, `product`, `quantity`, `special_notes`, `order_source`, `submitted_at`, `status`

See `workflows/sample-webhook-payload.json` for the exact payload structure.

---

### Event 5 — Zapier Receives Order Intake Webhook

**Trigger:** Zapier Webhook trigger fires.

**What happens:**
- Zapier receives and parses the incoming JSON payload
- The order intake automation workflow executes three parallel actions

---

### Event 6 — Google Sheets CRM Record Created

**Trigger:** Zapier processes the webhook payload.

**What happens:**
- Zapier creates a new row in the Google Sheets CRM document
- The row captures all key order fields with `status = 'new'`
- This record is a downstream auto-synced view — it is not the admin's control surface

---

### Event 7 — Telegram Admin Alert Sent

**Trigger:** Zapier order intake workflow.

**What happens:**
- Zapier sends an alert to the Telegram admin dashboard
- The alert includes the full order details and **three inline action buttons**:
  - Order Being Processed
  - Order Ready for Pickup
  - Order Picked Up
- The admin can now manage the order entirely from Telegram

---

### Event 8 — Customer Order Confirmation Email Sent

**Trigger:** Zapier order intake workflow.

**What happens:**
- Zapier sends an automated order confirmation email to the customer
- The email acknowledges receipt of the order and provides order details
- The customer is informed that updates will follow as the order progresses

---

### Event 9 — Admin Clicks Status Button in Telegram

**Trigger:** Admin action — admin clicks an inline button in the Telegram admin dashboard.

**What happens:**
- The button press sends a callback to the Replit backend
- The backend updates the order status in PostgreSQL:
  - **Order Being Processed** → `order_status = 'processing'`
  - **Order Ready for Pickup** → `order_status = 'ready_for_pickup'`
  - **Order Picked Up** → `order_status = 'picked_up'`, `picked_up_at = CURRENT_TIMESTAMP`
- The backend fires a **status update webhook** to Zapier

The admin does not update Google Sheets directly. All fulfillment control flows through the Telegram admin dashboard → Replit backend → PostgreSQL.

---

### Event 10 — Zapier Receives Status Update Webhook

**Trigger:** Status update webhook from Replit backend.

**What happens:**
- Zapier receives the status webhook and routes by status value
- Zapier auto-syncs the corresponding Google Sheets CRM row with the updated status
- Zapier sends the appropriate customer status email:
  - `processing` → "Your order is being processed"
  - `ready_for_pickup` → "Your order is ready for pickup"

---

### Event 11 — Pickup Confirmed

**Trigger:** Admin clicks "Order Picked Up" in the Telegram admin dashboard.

**What happens:**
- Replit backend sets `order_status = 'picked_up'` and records `picked_up_at`
- Backend fires a final status webhook to Zapier
- Zapier receives the webhook with `status = 'picked_up'`
- A 24-hour delay step is initiated in Zapier
- No immediate customer email is sent at this stage

---

### Event 12 — Thank-You Email + Feedback Request Sent

**Trigger:** 24-hour delay completes after `picked_up` webhook received by Zapier.

**What happens:**
- Zapier sends a thank-you email to the customer
- The email includes a link to a feedback form or review platform
- This closes the customer communication loop

---

## Summary Diagram

```
[Customer] → Order Form → Stripe Payment → Replit Backend → PostgreSQL
                                                                  ↓
                                              Order Intake Webhook → Zapier
                                                           ↙        ↓        ↘
                                                  Google       Telegram    Customer
                                                  Sheets       Admin       Confirm
                                                  (auto)       Dashboard   Email
                                                               ↓
                                              Admin clicks status button (Telegram)
                                                               ↓
                                              Replit Backend → PostgreSQL update
                                                               ↓
                                              Status Webhook → Zapier
                                                           ↙           ↘
                                                  Google Sheets     Customer Status
                                                  (auto-sync)       Email
                                                               ↓
                                              [picked_up] → 24hr delay → Thank-You Email
```

For the full Mermaid flowchart, see [architecture/system-architecture.md](architecture/system-architecture.md).
