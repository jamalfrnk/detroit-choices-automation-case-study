# Tech Stack — Detroit Choices Automation Case Study

---

## Overview

The Detroit Choices Automation System is built on a practical stack of production-ready, accessible tools. The goal was to build a reliable operational system without enterprise software overhead.

---

## Technology Stack

| Layer | Tool | Role |
|---|---|---|
| Web Application | Replit | Development environment and hosting |
| Database | PostgreSQL | Primary order data store |
| Secrets Management | Replit Environment Variables | Secure storage of API keys and credentials |
| Event Trigger | Webhooks | Event-driven communication between app and automation |
| Automation Orchestration | Zapier | Central workflow engine |
| CRM | Google Sheets | Lightweight order tracking and status management |
| Admin Notifications | Telegram Bot | Real-time internal alerts |
| Customer Communication | Email via Zapier | Transactional and lifecycle emails |
| Documentation / Portfolio | Git + GitHub | Version control and public portfolio packaging |

---

## Component Details

### Replit

Replit serves as both the development environment and the production hosting platform for the Detroit Choices website and order intake application.

- hosts the customer-facing order form
- runs the backend application logic
- provides an integrated environment for development, testing, and deployment
- manages environment variables and secrets natively through the Replit Secrets interface

### PostgreSQL

PostgreSQL is the primary system of record for the automation system.

- stores all incoming order records
- enforces structured schema for order data
- provides a reliable, queryable data store before automation processing begins
- feeds the webhook trigger with structured order information

### Replit Environment Variables / Secrets

All API keys, credentials, and service tokens are stored using Replit's built-in Secrets management.

- no credentials are stored in code or version control
- environment variables are injected at runtime
- this pattern mirrors production-grade secrets management practices

### Webhooks

The webhook layer is the event-driven bridge between the Replit application and Zapier.

- fires when a new order is submitted and written to the database
- sends a structured JSON payload containing all relevant order fields
- enables Zapier to begin processing without polling or manual triggers
- see `workflows/sample-webhook-payload.json` for the payload structure

### Zapier

Zapier is the central automation orchestration layer.

- receives the incoming webhook
- parses and routes order data
- creates CRM records in Google Sheets
- sends internal notifications
- sends customer emails
- manages status change triggers
- handles post-pickup follow-up delays and thank-you emails

### Google Sheets CRM

A Google Sheets document functions as a lightweight CRM.

- stores structured order records created by Zapier
- allows administrators to view and update order status
- status changes in the sheet trigger downstream Zapier workflows
- accessible without specialized CRM software

### Telegram Bot

A Telegram bot provides real-time internal alerting.

- notifies administrators immediately when a new order arrives
- delivers key order information (customer name, product, contact) directly to the admin's Telegram
- no app switching required for initial awareness

### Email Automation via Zapier

Email notifications are managed entirely through Zapier, covering the full customer communication lifecycle:

- order confirmation email (sent immediately on order receipt)
- order processed status email
- in preparation status email
- ready for pickup status email
- post-pickup thank-you email with feedback/review link

### Git + GitHub

Git and GitHub are used for documentation management and portfolio packaging.

- all project documentation is version-controlled
- the repository serves as a shareable, professional portfolio artifact
- commit history reflects the documentation process
