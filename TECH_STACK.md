# Tech Stack — Detroit Choices Automation Case Study

---

## Overview

The Detroit Choices Automation System is built on a practical stack of production-ready, accessible tools. The design goal was operational reliability and fast implementation without enterprise software overhead.

---

## Technology Stack

| Layer | Tool | Role |
|---|---|---|
| Web Application | Replit | Customer-facing website and backend application hosting |
| Database | PostgreSQL | Primary system of record for all order data and status |
| Payments | Stripe | Customer payment processing before pickup |
| Secrets Management | Replit Environment Variables | Secure runtime injection of credentials and API keys |
| Event Triggers | Webhooks | Event-driven communication from backend to Zapier (order intake + status updates) |
| Automation Orchestration | Zapier | CRM sync, notifications, and customer email automation |
| CRM | Google Sheets | Auto-synced downstream order tracking view |
| Admin Control Surface | Telegram Admin Dashboard | Inline action buttons for admin fulfillment management |
| Customer Communication | Email via Zapier | Transactional and lifecycle emails across the order lifecycle |
| Documentation / Portfolio | Git + GitHub | Version control and public portfolio packaging |

---

## Component Details

### Replit

Replit is the development environment and production hosting platform for the Detroit Choices website and backend application.

- hosts the customer-facing order form
- runs the backend application logic (order intake, Stripe integration, webhook dispatch, status update callbacks)
- manages environment variables and secrets natively through the Replit Secrets interface
- handles the Telegram admin dashboard callback endpoints

### PostgreSQL

PostgreSQL is the **primary system of record** for the automation system. All order data and status changes are written here first, before any automation is triggered.

- stores all confirmed order records with a structured schema
- tracks the full order status lifecycle (`new` → `processing` → `ready_for_pickup` → `picked_up`)
- records `picked_up_at` timestamp when an order is marked as picked up
- feeds all downstream webhook events — both on initial order creation and on each status change

### Stripe

Stripe handles customer payment processing as part of the order flow.

- customer completes payment through Stripe before the order is finalized
- backend stores the confirmed order in PostgreSQL only after payment is complete
- Stripe is not the trigger for the thank-you email — that is triggered by the `picked_up` status event

### Replit Environment Variables / Secrets

All credentials, API tokens, and service endpoints are stored using Replit's built-in Secrets management.

- PostgreSQL connection string
- Stripe API keys
- Zapier webhook URLs
- Telegram bot token and chat ID
- No credentials are stored in code or version control

### Webhooks

The webhook layer is the event-driven bridge between the Replit backend and Zapier. Two distinct webhook events are fired:

1. **Order intake webhook** — fired after Stripe payment completes and the order is written to PostgreSQL
2. **Status update webhook** — fired by the backend each time the admin updates order status via the Telegram admin dashboard

Both events send structured JSON payloads to Zapier for processing. This design keeps Zapier fully reactive — no polling, no manual CRM triggers.

### Zapier

Zapier is the central automation orchestration layer. It handles two separate workflow triggers:

**Zap 1 — Order Intake:**
- receives the initial order webhook
- creates a Google Sheets CRM record
- sends a Telegram admin alert
- sends the customer an order confirmation email

**Zap 2 — Status Updates:**
- receives the status webhook from the Replit backend
- auto-syncs the Google Sheets CRM row with the new status
- sends the appropriate customer status email based on the status value
- on `picked_up`: initiates a 24-hour delay, then sends the thank-you + feedback email

### Google Sheets CRM

Google Sheets serves as a lightweight, **auto-synced downstream CRM view**. It is written to exclusively by Zapier — the admin never edits it directly.

- populated by Zapier on initial order intake
- updated by Zapier on each status change event
- provides a readable operational log of all orders and current statuses
- does not trigger any automation — it is a downstream output, not a control surface

### Telegram Admin Dashboard

The Telegram admin dashboard is the **sole control surface for admin fulfillment actions**. It provides inline action buttons that allow the admin to manage order status entirely within Telegram.

**Inline buttons:**
- **Order Being Processed** → calls Replit backend → sets `order_status = 'processing'`
- **Order Ready for Pickup** → calls Replit backend → sets `order_status = 'ready_for_pickup'`
- **Order Picked Up** → calls Replit backend → sets `order_status = 'picked_up'`, records `picked_up_at`

Each button press calls back to the Replit backend, which updates PostgreSQL and fires a status webhook to Zapier. The admin never needs to touch Google Sheets, the database, or any other system.

### Email Automation via Zapier

Customer emails are managed entirely through Zapier across the full order lifecycle:

- **Order confirmation** — sent immediately after order intake webhook
- **Order processing** — sent when `order_status = 'processing'`
- **Ready for pickup** — sent when `order_status = 'ready_for_pickup'`
- **Thank-you + feedback** — sent 24 hours after `order_status = 'picked_up'`

### Git + GitHub

Git and GitHub are used for documentation management and portfolio packaging.

- all project documentation is version-controlled
- the repository serves as a shareable, professional portfolio artifact demonstrating the system design
- commit history reflects the documentation and scaffolding process
