# Data Model — Detroit Choices Automation Case Study

---

## Overview

The Detroit Choices Automation System uses **PostgreSQL** as its primary system of record. All incoming customer orders are written to the database before any automation processing begins, ensuring that order data is durably stored and structured.

The database is hosted within the Replit environment alongside the web application backend.

---

## Orders Table

The core data structure is the `orders` table, which captures all information submitted via the customer order form.

### Schema

```sql
CREATE TABLE orders (
    order_id              VARCHAR(20) PRIMARY KEY,
    customer_name         VARCHAR(255) NOT NULL,
    phone                 VARCHAR(50),
    email                 VARCHAR(255) NOT NULL,
    product               VARCHAR(255) NOT NULL,
    quantity              INTEGER NOT NULL DEFAULT 1,
    special_notes         TEXT,
    order_source          VARCHAR(100) DEFAULT 'Website Form',
    stripe_payment_id     VARCHAR(255),
    submitted_at          TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    status                VARCHAR(50) NOT NULL DEFAULT 'new',
    status_updated_at     TIMESTAMPTZ,
    picked_up_at          TIMESTAMPTZ
);
```

### Field Descriptions

| Field | Type | Description |
|---|---|---|
| `order_id` | VARCHAR | Unique order identifier (e.g., `DC-1001`) |
| `customer_name` | VARCHAR | Customer's full name |
| `phone` | VARCHAR | Customer contact phone number |
| `email` | VARCHAR | Customer email address (used for automated communications) |
| `product` | VARCHAR | Product ordered |
| `quantity` | INTEGER | Number of units ordered |
| `special_notes` | TEXT | Free-text field for customization or instructions |
| `order_source` | VARCHAR | Origin of the order (e.g., `Website Form`) |
| `stripe_payment_id` | VARCHAR | Stripe payment reference; recorded when payment completes |
| `submitted_at` | TIMESTAMPTZ | Timestamp of order submission (UTC) |
| `status` | VARCHAR | Current order status (see lifecycle below) |
| `status_updated_at` | TIMESTAMPTZ | Timestamp of most recent status change |
| `picked_up_at` | TIMESTAMPTZ | Timestamp set by backend when admin marks order as `picked_up` |

---

## Status Lifecycle

Order status moves through a defined lifecycle. Each status change is written to PostgreSQL by the Replit backend (triggered by admin button presses in Telegram), then propagated to Zapier via webhook, which sends the corresponding customer email.

```
new
 └──► processing         → customer email: Order Being Processed
       └──► ready_for_pickup → customer email: Order Ready for Pickup
             └──► picked_up
                   └──► (24-hour delay in Zapier)
                         └──► Thank-You Email + Feedback Request sent
```

### Status Values

| Status | How It Is Set | Customer Email Sent |
|---|---|---|
| `new` | Backend on order creation (after Stripe payment) | Order confirmation email |
| `processing` | Backend when admin clicks "Order Being Processed" in Telegram | Order processing notification |
| `ready_for_pickup` | Backend when admin clicks "Order Ready for Pickup" in Telegram | Ready for pickup notification |
| `picked_up` | Backend when admin clicks "Order Picked Up" in Telegram; `picked_up_at` is also recorded | Thank-you + feedback email (after 24-hour delay) |

---

## How the Database Feeds Automation

When an order is written to the database, the backend application fires a **webhook event** containing the structured order data as a JSON payload.

The webhook payload maps directly to the `orders` table fields. Zapier receives this payload and begins the automation workflow — creating the CRM record, sending notifications, and initiating the customer communication sequence.

See `workflows/sample-webhook-payload.json` for the exact payload structure.

Status change automation is triggered by **backend webhook events**, not by Google Sheets. When the admin clicks a status button in the Telegram admin dashboard, the Replit backend updates PostgreSQL and fires a status webhook to Zapier. Zapier then auto-syncs the Google Sheets CRM row and sends the appropriate customer email.

Google Sheets reflects the database state as a downstream view — it does not control or trigger automation.

---

## Data Security Notes

- No customer data is stored in version control
- All database credentials are managed through Replit environment variables
- Sample data in this repository uses sanitized placeholder values only
