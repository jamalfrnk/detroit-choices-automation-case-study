# Detroit Choices Automation Case Study

A portfolio case study documenting the design and implementation of a small-business order automation system built by Malcolm Frank.

---

## Business Challenge

Detroit Choices, a small food business, was managing incoming orders manually across multiple communication channels. This created:

- no centralized order records
- delayed internal response times
- inconsistent customer communication
- manual effort duplicated across admin tasks

The goal was to replace this with a lightweight, event-driven automation system using accessible tools — without enterprise software overhead.

---

## Solution Overview

The system connects a customer-facing order form to a structured backend and a series of automated workflows that handle internal coordination and customer communication end-to-end.

**Order lifecycle:**

1. Customer submits order on the Detroit Choices website
2. Website (hosted on Replit) stores order data in PostgreSQL
3. Webhook fires with structured order payload into Zapier
4. Zapier creates a CRM record in Google Sheets
5. Admin receives Telegram alert + internal email notification
6. Customer receives an automated order confirmation email
7. As admin updates order status, automated status emails go to the customer:
   - Order Processed
   - In Preparation
   - Ready for Pickup
8. After pickup is confirmed, a 24–48 hour delay triggers a thank-you email with a feedback/review link

---

## Architecture

```
Customer → Replit Web App → PostgreSQL → Webhook → Zapier
                                                       ├── Google Sheets CRM
                                                       ├── Telegram Admin Alert
                                                       ├── Internal Email
                                                       └── Customer Emails (confirmation + status + thank-you)
```

See the full Mermaid diagram in [architecture/system-architecture.md](architecture/system-architecture.md).

---

## Tech Stack

| Layer | Tool |
|---|---|
| Web Application | Replit |
| Database | PostgreSQL |
| Secrets Management | Replit Environment Variables |
| Event Trigger | Webhooks |
| Automation Orchestration | Zapier |
| CRM | Google Sheets |
| Admin Notifications | Telegram Bot |
| Customer Emails | Email via Zapier |
| Documentation / Portfolio | Git + GitHub |

---

## Repository Contents

```
Detroit-Choices-Automation-Case-Study/
├── README.md                          — this file
├── PROJECT_SUMMARY.md                 — full case study narrative
├── TECH_STACK.md                      — technology stack breakdown
├── DATA_MODEL.md                      — database schema and status lifecycle
├── EVENT_FLOW.md                      — step-by-step event flow documentation
├── TASK_LIST.md                       — completion and review checklist
├── SYSTEM_PROMPT.md                   — repo governance and documentation principles
├── SYSTEM.md                          — repo setup and operating guide
├── .gitignore
│
├── architecture/
│   └── system-architecture.md         — component breakdown + Mermaid diagram
│
├── workflows/
│   ├── zapier-flow-description.md     — detailed Zapier workflow documentation
│   └── sample-webhook-payload.json    — example webhook payload
│
├── artifacts/
│   ├── meeting-notes.md               — project notes and decisions
│   ├── workflow-logic.md              — workflow logic documentation
│   └── implementation-notes.md        — implementation and integration notes
│
├── demo/
│   └── walkthrough-notes.md           — demo script for presenting the project
│
└── screenshots/
    ├── detroit_choices_sys_arch.png   — system architecture diagram
    └── tg_admin_portal.gif            — Telegram admin portal demo
```

---

## Screenshots

**System Architecture Diagram**

![System Architecture](screenshots/detroit_choices_sys_arch.png)

**Telegram Admin Portal**

![Telegram Admin Portal](screenshots/tg_admin_portal.gif)

---

## Why This Case Study Matters

This project demonstrates a practical, production-deployed automation system built for a real small business using widely available tools.

It shows:

- **systems thinking** — connecting web application, database, event triggers, and multi-channel communications into a coherent workflow
- **implementation ability** — the system was fully designed and deployed, not just theorized
- **automation architecture** — event-driven design using webhooks and Zapier as the orchestration layer
- **CRM and operations design** — lightweight Google Sheets CRM integrated into an automated status lifecycle
- **customer lifecycle management** — structured communication from order confirmation through post-pickup follow-up

The architecture is deliberately lightweight and accessible, demonstrating how practical automation can be built without expensive enterprise platforms.

---

Built by **Malcolm Frank**
