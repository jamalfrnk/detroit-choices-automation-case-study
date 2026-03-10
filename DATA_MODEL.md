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
    order_id          VARCHAR(20) PRIMARY KEY,
    customer_name     VARCHAR(255) NOT NULL,
    phone             VARCHAR(50),
    email             VARCHAR(255) NOT NULL,
    product           VARCHAR(255) NOT NULL,
    quantity          INTEGER NOT NULL DEFAULT 1,
    special_notes     TEXT,
    order_source      VARCHAR(100) DEFAULT 'Website Form',
    submitted_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    status            VARCHAR(50) NOT NULL DEFAULT 'New',
    status_updated_at TIMESTAMPTZ
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
| `submitted_at` | TIMESTAMPTZ | Timestamp of order submission (UTC) |
| `status` | VARCHAR | Current order status (see lifecycle below) |
| `status_updated_at` | TIMESTAMPTZ | Timestamp of most recent status change |

---

## Status Lifecycle

Order status moves through a defined lifecycle. Each status change triggers a corresponding automated customer email via Zapier.

```
New
 └──► Processed          → customer email: Order Processed
       └──► In Preparation  → customer email: In Preparation
             └──► Ready for Pickup  → customer email: Ready for Pickup
                   └──► Picked Up
                         └──► (24–48 hour delay)
                               └──► Thank-You Email + Feedback Request sent
```

### Status Values

| Status | Trigger | Customer Email Sent |
|---|---|---|
| `New` | Order submitted via website | Order confirmation email |
| `Processed` | Admin updates status | Order processed notification |
| `In Preparation` | Admin updates status | In preparation notification |
| `Ready for Pickup` | Admin updates status | Ready for pickup notification |
| `Picked Up` | Admin confirms pickup | Thank-you + feedback email (after 24–48 hour delay) |

---

## How the Database Feeds Automation

When an order is written to the database, the backend application fires a **webhook event** containing the structured order data as a JSON payload.

The webhook payload maps directly to the `orders` table fields. Zapier receives this payload and begins the automation workflow — creating the CRM record, sending notifications, and initiating the customer communication sequence.

See `workflows/sample-webhook-payload.json` for the exact payload structure.

Status change automation is triggered by Zapier monitoring the Google Sheets CRM for status field updates. The Google Sheets record mirrors the core fields from the `orders` table and serves as the admin-facing operational view.

---

## Data Security Notes

- No customer data is stored in version control
- All database credentials are managed through Replit environment variables
- Sample data in this repository uses sanitized placeholder values only
