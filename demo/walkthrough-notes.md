# Demo Walkthrough Notes — Detroit Choices Automation Case Study

A 3–5 minute demo script for presenting this project. Designed for a Fastlane AI review or similar technical/consulting audience.

---

## Demo Format

**Total time:** 3–5 minutes
**Format:** Screen share or slide walkthrough with live system reference
**Audience:** AI consulting founders, solutions architects, or technical hiring reviewers

---

## Segment 1: Introduction (30–45 seconds)

**Goal:** Set the context — who is this for and what problem does it solve?

**Script outline:**

> "Detroit Choices is a small food business that was managing all incoming orders manually — through texts, DMs, and phone calls. There was no centralized record of what was ordered, when, or what the status was. Customer communication was inconsistent, and the admin had no single place to manage order fulfillment.
>
> The goal was to build a lightweight, event-driven automation system that could handle order intake, payment, internal alerts, admin control, and the full customer communication lifecycle — without enterprise software."

**Key point to land:** Real small business problem, practical production solution.

---

## Segment 2: Architecture Walkthrough (75–90 seconds)

**Goal:** Walk through the system end to end — two distinct flows.

**Reference:** `screenshots/detroit_choices_sys_arch.png` or the Mermaid diagram in `architecture/system-architecture.md`

**Script outline:**

> "Here's how it works.
>
> A customer submits an order on the Detroit Choices website, which runs on Replit. They complete payment through Stripe. Once payment is confirmed, the Replit backend writes the order to PostgreSQL — that's the system of record — and immediately fires a webhook to Zapier.
>
> Zapier handles three things in parallel: it creates a record in the Google Sheets CRM, it sends an alert to the Telegram admin dashboard, and it sends the customer an order confirmation email.
>
> Here's the important design decision: the admin never touches Google Sheets. All fulfillment control happens through the Telegram admin dashboard, which has three inline buttons — Order Being Processed, Order Ready for Pickup, and Order Picked Up.
>
> When the admin clicks a button, it calls back to the Replit backend, which updates the order status in PostgreSQL and fires a status webhook to Zapier. Zapier then auto-syncs the CRM and sends the customer the appropriate status email.
>
> When the admin marks an order as Picked Up, the backend records the pickup timestamp, fires one more webhook, and Zapier waits exactly 24 hours before sending the customer a thank-you email with a feedback link."

**Key points to land:**
- Two webhook flows: order intake and status updates
- PostgreSQL is the system of record; Google Sheets is downstream auto-sync only
- Telegram is the admin control surface — no manual CRM editing required

---

## Segment 3: Live System Reference (60–75 seconds)

**Goal:** Ground the architecture in something concrete and real.

**Reference materials:**
- `screenshots/tg_admin_portal.gif` — Telegram admin dashboard demo
- `architecture/system-architecture.md` — Mermaid diagram
- `workflows/sample-webhook-payload.json` — webhook payload structure

**Script outline:**

> "Here's the Telegram admin dashboard — every time an order comes in, the admin gets an immediate alert with the order details and those three action buttons. No app switching, no CRM to update.
>
> The webhook payload looks like this — structured JSON with the order fields, which Zapier parses and routes in a single workflow. And here's the full architecture diagram showing both webhook flows — the order intake path and the status update path — and how they each drive Zapier independently.
>
> The Google Sheets CRM is always accurate, but it's a read-only view — everything in it was written by Zapier. The admin never needs to touch it."

**Key point to land:** The system is deployed and working. The control flow is clean and deliberate.

---

## Segment 4: Closing — Why It Matters (30–45 seconds)

**Goal:** Connect the implementation to broader engineering and product thinking.

**Script outline:**

> "What this demonstrates is practical systems thinking — combining a web application, database, payment processing, event-driven backends, and multi-channel automation into a coherent pipeline using accessible tools.
>
> The two-webhook architecture — one for order intake, one for status updates — keeps the flows independent and easy to maintain. Zapier is fully reactive. The admin has one tool, Telegram, that controls everything. And the CRM is always accurate without anyone manually updating it.
>
> The business impact: the admin no longer manages communication manually, customers get consistent updates at every stage, and post-pickup feedback is collected automatically — 24 hours after pickup, every time."

**Key point to land:** Production-deployed. Clean architecture. Extensible pattern.

---

## Demo Checklist

Before the demo:

- [ ] Confirm `screenshots/detroit_choices_sys_arch.png` is ready to display
- [ ] Confirm `screenshots/tg_admin_portal.gif` plays correctly
- [ ] Have `workflows/sample-webhook-payload.json` open in a code view
- [ ] Have the Mermaid diagram rendered (GitHub or a Markdown viewer)
- [ ] Have `architecture/system-architecture.md` ready to reference the two-flow description

---

## Notes for Presenter

- Lead with the admin control flow design decision — it's the most architecturally interesting part
- Be precise: "admin clicks a Telegram button" not "admin updates the CRM"
- Emphasize the two-webhook design — it demonstrates deliberate architectural thinking
- If asked about scale: acknowledge current tool limitations, reference future improvements in `artifacts/implementation-notes.md`
- If asked about Stripe: it gates the database write — only confirmed payments become orders
