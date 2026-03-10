# Workflow Logic — Detroit Choices Automation Case Study

This document explains the logic behind the automation workflows in plain English and technical terms.

---

## Overview

The automation system is built around a single core event: **a customer submits an order**. Everything else flows from that trigger.

The workflow logic can be broken into three phases:

1. **Order Intake Phase** — capture, store, and route the order
2. **Notification Phase** — alert administrators and confirm to the customer
3. **Lifecycle Phase** — track status changes and close the loop post-pickup

---

## Phase 1: Order Intake

### Plain English

When a customer submits the order form, the system immediately writes the order to a database and fires an event into the automation pipeline. This happens automatically — no manual action required.

### Technical Logic

1. Customer submits HTML form → POST request to Replit backend
2. Backend validates form data
3. Backend writes record to PostgreSQL `orders` table with `status = 'New'`
4. Backend fires HTTP POST webhook to Zapier webhook URL
5. Webhook payload contains all order fields as JSON

**Trigger condition:** Successful database write
**Failure handling:** If the webhook fails to send, the order record still exists in PostgreSQL

---

## Phase 2: Notification

### Plain English

Once Zapier receives the order, it branches into multiple parallel actions: creating a CRM record, alerting the admin via Telegram, sending an internal email, and sending the customer a confirmation email.

### Technical Logic

**Zapier Trigger:** Webhooks by Zapier (Catch Hook)

**Action sequence:**

```
Webhook received
    ├── Action: Google Sheets — Create Spreadsheet Row
    │     Fields mapped: order_id, customer_name, phone, email,
    │                    product, quantity, special_notes, submitted_at, status
    │
    ├── Action: Telegram — Send Message
    │     Message: "New order received: [order_id] — [customer_name] — [product]"
    │
    ├── Action: Email — Send Outbound Email (internal)
    │     To: business owner email
    │     Subject: "New Detroit Choices Order: [order_id]"
    │     Body: Full order details
    │
    └── Action: Email — Send Outbound Email (customer confirmation)
          To: customer email from webhook payload
          Subject: "Your Detroit Choices Order Has Been Received"
          Body: Order details + "we'll keep you updated as your order progresses"
```

**Branching logic:** All four actions execute in sequence; no conditional branching at this stage.

---

## Phase 3: Lifecycle Management

### Plain English

As the admin updates the order status in Google Sheets, the system automatically sends the customer the appropriate status email. When the order is marked as picked up, the system waits and then sends a thank-you email.

### Technical Logic

**Zapier Trigger:** Google Sheets — New or Updated Spreadsheet Row

**Condition logic (Zapier Filter or Path):**

```
Status field updated
    ├── IF status == "Processed"
    │     → Send customer email: "Your order has been processed"
    │
    ├── IF status == "In Preparation"
    │     → Send customer email: "Your order is being prepared"
    │
    ├── IF status == "Ready for Pickup"
    │     → Send customer email: "Your order is ready for pickup"
    │
    └── IF status == "Picked Up"
          → Delay: 24–48 hours
          → Send customer email: "Thank you for your order — share your feedback"
                Includes: feedback form or review link
```

**Key design consideration:** The trigger fires on any row update. Zapier Paths (or Filter steps) evaluate the `Status` field value and route to the correct email action. Only one path executes per trigger event.

---

## Data Mapping

The following fields flow through the entire system:

| Field | Source | Used In |
|---|---|---|
| `order_id` | PostgreSQL | Webhook, CRM, all emails |
| `customer_name` | Form input | Webhook, CRM, all emails |
| `email` | Form input | Webhook, CRM, all customer emails |
| `phone` | Form input | Webhook, CRM, Telegram alert |
| `product` | Form input | Webhook, CRM, all emails |
| `quantity` | Form input | Webhook, CRM, emails |
| `special_notes` | Form input | Webhook, CRM, internal emails |
| `submitted_at` | Backend timestamp | Webhook, CRM |
| `status` | Database / CRM update | Status email routing |

---

## Error and Edge Case Considerations

- **Duplicate triggers:** If a CRM row is updated multiple times rapidly, multiple emails could fire. In production, this should be managed with a debounce filter or status-change-only trigger.
- **Missing email field:** Zapier should include a filter step to halt execution if the `email` field is empty before sending customer emails.
- **Webhook replay:** If the webhook is retried (e.g., after a network timeout), duplicate CRM rows could be created. Order ID deduplication logic should be considered for higher-volume deployments.
