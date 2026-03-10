# Meeting Notes — Detroit Choices Automation Case Study

This file captures relevant project conversations, stakeholder discussions, and key decisions made during the design and implementation of the automation system.

---

## Note Template

Use the following format for each session or conversation:

```
## [Date] — [Meeting Type / Topic]

**Attendees:**
-

**Agenda / Context:**
-

**Decisions Made:**
-

**Action Items:**
- [ ]

**Open Questions:**
-
```

---

## Notes

### [Date] — Initial Requirements Discovery

**Attendees:**
- Malcolm Frank
- Detroit Choices stakeholder

**Agenda / Context:**
- Initial discussion about operational pain points
- Review of current order intake process
- Exploration of automation possibilities

**Decisions Made:**
- Website order form as the primary intake channel
- PostgreSQL for structured order storage
- Zapier as the automation orchestration layer
- Google Sheets as the lightweight CRM
- Telegram for real-time admin alerts
- Email for customer communication

**Action Items:**
- [ ] Define order data schema
- [ ] Set up Replit environment
- [ ] Configure PostgreSQL connection
- [ ] Build Zapier workflow skeleton

**Open Questions:**
- What review platform should the feedback link point to?
- Should status updates be sent for every status change or only selected ones?

---

### [Date] — Architecture Review

**Attendees:**
- Malcolm Frank

**Agenda / Context:**
- Review of system architecture design
- Validation of webhook payload structure
- Confirmation of Zapier workflow steps

**Decisions Made:**
- Webhook fires immediately after database write
- Zapier handles all downstream communication
- Status change emails triggered by Google Sheets updates

**Action Items:**
- [ ] Test end-to-end order flow
- [ ] Validate Telegram bot integration
- [ ] Confirm email delivery

**Open Questions:**
- What should the 24–48 hour post-pickup delay be set to exactly?

---

<!-- Add additional meeting notes below as the project progresses -->
