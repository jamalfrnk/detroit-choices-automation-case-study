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

> "Detroit Choices is a small food business that was managing all incoming orders manually — through DMs, texts, and phone calls. There was no centralized record of what was ordered, when, or what the status was. Communication with customers was inconsistent, and admin overhead was high.
>
> The goal was to build a lightweight automation system that could handle order intake, CRM tracking, internal alerts, and the full customer communication lifecycle — without enterprise software."

**Key point to land:** Small business problem, practical solution, accessible tools.

---

## Segment 2: Architecture Walkthrough (60–90 seconds)

**Goal:** Show the system design and how the pieces connect.

**Reference:** `screenshots/detroit_choices_sys_arch.png` or the Mermaid diagram in `architecture/system-architecture.md`

**Script outline:**

> "Here's how the system works end to end.
>
> A customer submits an order form on the Detroit Choices website, which is hosted on Replit. The backend writes that order to a PostgreSQL database — that's the system of record.
>
> Immediately after the write, the backend fires a webhook with the structured order data into Zapier. From there, Zapier branches into four actions simultaneously: it creates a CRM row in Google Sheets, sends a Telegram alert to the admin, sends an internal email notification, and sends the customer an order confirmation.
>
> As the admin updates the order status in the Google Sheets CRM, Zapier picks up those changes and sends the customer the right status email — processed, in preparation, ready for pickup. Once the order is marked picked up, there's a 24 to 48 hour delay, then a thank-you email goes out with a feedback link."

**Key point to land:** One event drives the entire pipeline automatically.

---

## Segment 3: Live System Reference (60–90 seconds)

**Goal:** Ground the architecture in something real.

**Reference materials:**
- `screenshots/tg_admin_portal.gif` — Telegram admin alert demo
- Google Sheets CRM (if available for screen share)
- Zapier task history (if available)

**Script outline:**

> "Here's the Telegram admin portal — every time an order comes in, the admin gets an immediate push notification with the key details. No checking email, no dashboard login.
>
> In the Google Sheets CRM, each order is a row. The admin updates the status column here, and Zapier handles everything downstream. The customer gets the right email automatically.
>
> The webhook payload looks like this — [reference sample-webhook-payload.json] — structured JSON that Zapier can parse and route in a single workflow."

**Key point to land:** The system is real, deployed, and working — not a mockup.

---

## Segment 4: Closing — Why It Matters (30–45 seconds)

**Goal:** Connect the implementation to broader capability.

**Script outline:**

> "What this demonstrates is practical systems thinking — connecting a web application, database, event triggers, and multi-channel communications into a coherent workflow using tools that are accessible to small businesses.
>
> The architecture is event-driven, modular, and extensible. You could swap Zapier for a custom workflow engine, or swap Google Sheets for a proper CRM, without redesigning the rest of the system.
>
> The business impact is straightforward: the admin no longer manages communication manually, customers get consistent updates, and every order is tracked from submission to post-pickup feedback."

**Key point to land:** This is a production system, not a concept. It demonstrates end-to-end implementation thinking.

---

## Demo Checklist

Before the demo:

- [ ] Confirm `screenshots/detroit_choices_sys_arch.png` is ready to display
- [ ] Confirm `screenshots/tg_admin_portal.gif` plays correctly
- [ ] Have `workflows/sample-webhook-payload.json` open in a code view
- [ ] Have the Mermaid diagram rendered (GitHub or a Markdown viewer)
- [ ] Have a test order flow ready if doing a live system demo

---

## Notes for Presenter

- Keep the intro tight — the architecture walkthrough is the core
- Don't over-explain Zapier — most technical audiences understand automation platforms
- Emphasize that this is deployed and working, not theoretical
- If asked about scale: acknowledge current limitations, mention future improvement paths documented in `artifacts/implementation-notes.md`
