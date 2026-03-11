# Detroit Choices Automation System — Project Summary

## Overview

The Detroit Choices Automation System is a production-deployed operational automation platform designed to handle order intake, payment, internal coordination, and customer communication for a small food business.

The system connects a Replit-hosted website, Stripe payment processing, PostgreSQL order storage, webhook-triggered automation, Zapier workflow orchestration, a Telegram admin dashboard, auto-synced Google Sheets CRM, and customer-facing email automation into a single cohesive pipeline.

---

## Business Challenge

Before implementing automation, incoming orders required manual coordination across phone calls, texts, and direct messages. This created several operational problems:

- no centralized order records
- delayed internal response times
- fragmented team communication
- inconsistent customer updates
- no structured feedback collection after fulfillment

The business needed a system that could automatically capture and store orders, notify administrators in real time, give the admin a simple control surface for managing order status, communicate with customers throughout the fulfillment lifecycle, and collect feedback after pickup — all without manual data entry.

---

## Solution

The Detroit Choices Automation System is built as an **event-driven pipeline** with two distinct webhook flows: one for new order intake, and one for status updates driven by admin actions.

**Order intake flow:**

1. Customer submits an order on the Detroit Choices website
2. Customer completes payment through Stripe
3. Replit backend stores the confirmed order in PostgreSQL (`order_status = 'new'`)
4. Backend fires a webhook to Zapier
5. Zapier creates a Google Sheets CRM record, sends a Telegram admin alert with order details, and sends the customer an automated order confirmation email

**Admin fulfillment control flow:**

6. Admin manages all fulfillment actions through the **Telegram admin dashboard** using inline buttons:
   - **Order Being Processed** → backend sets `order_status = 'processing'`
   - **Order Ready for Pickup** → backend sets `order_status = 'ready_for_pickup'`
   - **Order Picked Up** → backend sets `order_status = 'picked_up'` and `picked_up_at = CURRENT_TIMESTAMP`
7. Each button press calls back to the Replit backend
8. Backend updates PostgreSQL and fires a status webhook to Zapier
9. Zapier auto-syncs the Google Sheets CRM row and sends the appropriate customer status email

**Post-pickup follow-up:**

10. When `order_status = 'picked_up'`, the backend fires a final webhook to Zapier
11. Zapier waits 24 hours
12. Zapier sends a thank-you email with a feedback/review link

The admin never manually edits Google Sheets. PostgreSQL is the system of record. Google Sheets is an auto-synced downstream view.

---

## Malcolm Frank's Role

Malcolm Frank designed and implemented the full automation system.

Responsibilities included:

- designing the end-to-end automation architecture
- building the website and backend application in Replit
- configuring PostgreSQL database schema and order status lifecycle
- integrating Stripe for payment processing
- managing all environment variables and application secrets in Replit
- implementing webhook-based automation for both order intake and status events
- building the Telegram admin dashboard with inline status action buttons
- configuring Zapier workflows for CRM sync, notifications, and customer emails
- testing and validating the full pipeline end-to-end

---

## System Architecture

```
Customer Order Form (Replit)
        ↓
  Stripe Payment
        ↓
 PostgreSQL (system of record)
        ↓
  Webhook → Zapier (order intake)
        ↓
  ┌─────────────────────────────┐
  │ Google Sheets CRM (auto)    │
  │ Telegram Admin Dashboard    │
  │ Customer Confirmation Email │
  └─────────────────────────────┘
        ↓
  Admin clicks Telegram button
        ↓
  Replit Backend → PostgreSQL update
        ↓
  Webhook → Zapier (status event)
        ↓
  ┌────────────────────────────┐
  │ Google Sheets CRM (sync)   │
  │ Customer Status Email      │
  └────────────────────────────┘
        ↓
  [picked_up] → 24hr delay → Thank-You Email + Feedback Link
```

See the Mermaid diagram in [architecture/system-architecture.md](architecture/system-architecture.md).

---

## Technology Stack

| Tool | Role |
|---|---|
| Replit | Web application development and hosting |
| PostgreSQL | Primary system of record (order data and status) |
| Stripe | Customer payment processing |
| Replit Secrets | Environment variable and credentials management |
| Webhooks | Event-driven triggers (order intake + status updates) |
| Zapier | Automation orchestration (CRM sync, notifications, emails) |
| Google Sheets | Auto-synced downstream CRM view |
| Telegram Admin Dashboard | Admin fulfillment control surface (inline buttons) |
| Email via Zapier | Customer lifecycle emails |
| Git + GitHub | Documentation and portfolio packaging |

---

## Business Impact

The automation system improved operations in several meaningful ways:

- eliminated manual order entry and tracking
- gave the admin a single-tool control surface via Telegram
- removed the need to manually update any spreadsheet or CRM
- automated all customer communication from confirmation through thank-you
- enabled structured post-pickup feedback collection on a consistent schedule
- reduced administrative workload so the business can focus on fulfillment

---

## Why This Case Study Matters

This project demonstrates practical automation architecture with real deployment, not a prototype.

It integrates:

- full-stack application development (Replit + PostgreSQL)
- payment processing (Stripe)
- event-driven backend design (webhook-triggered status pipeline)
- admin UX design (Telegram as the sole fulfillment control surface)
- workflow automation (Zapier orchestrating CRM sync and customer emails)
- customer lifecycle management (confirmation → status updates → post-pickup follow-up)

It demonstrates how a lightweight, accessible automation stack can be used to build a reliable operational system for a small business — one that is also extensible toward more sophisticated infrastructure as the business scales.
