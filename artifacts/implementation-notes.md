# Implementation Notes — Detroit Choices Automation Case Study

Technical notes, build considerations, integration decisions, and future improvement opportunities.

---

## Environment Setup

### Replit

- The application is developed and hosted entirely within a Replit Repl
- The Replit environment handles both the web server and the database connection
- PostgreSQL is connected via a standard connection string stored as a Replit Secret

### Secrets Management

All sensitive values are stored as **Replit Secrets** (environment variables), including:

- PostgreSQL connection string
- Zapier webhook URL
- Any API keys or service credentials

No secrets are stored in code files or version control. This mirrors production-grade secrets management practices.

### PostgreSQL

- Database is provisioned and connected to the Replit application backend
- Connection is managed via environment variable injection at runtime
- The `orders` table schema is defined in `DATA_MODEL.md`

---

## Integration Notes

### Webhook Integration

- The webhook fires as an HTTP POST from the Replit backend immediately after a successful database write
- The Zapier webhook URL is stored as a Replit Secret and referenced in the backend code at runtime
- The payload is structured JSON matching the `orders` table schema

### Zapier Integration

- The Zapier workflow is triggered by a **Webhooks by Zapier** trigger (Catch Hook)
- Downstream actions use Zapier's native integrations for Google Sheets, Telegram, and email
- The status change workflow uses a second Zapier Zap triggered by **Google Sheets — New or Updated Spreadsheet Row**

### Google Sheets CRM

- The CRM sheet is structured with column headers matching the order fields
- Zapier writes to the sheet using the **Google Sheets — Create Spreadsheet Row** action
- Status updates are made manually by the admin directly in the sheet
- The `Status` column change triggers the second Zapier Zap

### Telegram Bot

- The Telegram bot is configured via BotFather and the bot token is stored as a Replit Secret
- The Zapier Telegram action sends formatted messages using the Chat ID of the admin's Telegram account or group

### Email

- Outbound emails are sent via Zapier's built-in email action or a connected email provider
- All customer-facing emails are templated with order details pulled from the webhook payload or CRM row

---

## Build Decisions

| Decision | Rationale |
|---|---|
| Replit for hosting | Fast setup, integrated environment, accessible for small business implementation |
| PostgreSQL for storage | Structured, reliable, queryable — appropriate for order management |
| Webhook over polling | Event-driven design is more responsive and efficient than scheduled polling |
| Zapier for orchestration | No-code/low-code orchestration reduces development time without sacrificing capability |
| Google Sheets CRM | Accessible, familiar to non-technical admins, integrates natively with Zapier |
| Telegram for alerts | Immediate, push-based notifications without requiring a custom admin dashboard |
| Post-pickup email delay | Gives the customer time to experience the product before requesting feedback |

---

## Testing Notes

- End-to-end testing was performed by submitting test orders through the live form
- Webhook delivery was verified via Zapier's task history
- Email delivery was verified by reviewing received test emails
- Telegram alerts were verified by reviewing the admin Telegram chat
- Status change emails were tested by manually updating the Google Sheets CRM status field

---

## Known Limitations

- The Google Sheets CRM is not designed for high-volume concurrent updates
- Status change automation triggers on any row update — filtering logic should be validated carefully to avoid duplicate emails
- No built-in retry logic if the webhook delivery fails (order data is preserved in PostgreSQL regardless)
- Email deliverability depends on the email provider connected to Zapier

---

## Future Improvement Opportunities

- **Dedicated CRM platform** — migrate from Google Sheets to Airtable or a lightweight database-backed CRM for better scalability
- **Admin dashboard** — build a simple read-only dashboard for order status visibility without requiring Google Sheets access
- **SMS notifications** — add Twilio-based SMS alerts as a fallback or complement to email
- **Webhook retry and error handling** — implement retry logic in the Replit backend for webhook delivery failures
- **Automated status progression** — explore automating some status transitions (e.g., auto-set to "In Preparation" after a defined time period)
- **Analytics layer** — add basic order volume and fulfillment time tracking via Google Sheets formulas or a dedicated analytics tool
