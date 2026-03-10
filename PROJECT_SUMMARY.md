# Detroit Choices Automation System — Project Summary

## Overview

The Detroit Choices Automation System is a lightweight operational automation platform designed to streamline order intake, internal coordination, customer communication, and operational visibility for a small business.

The system integrates a website order form, PostgreSQL database storage, webhook-triggered automation, Zapier workflow orchestration, a Google Sheets-based CRM, and both internal and customer-facing notifications.

This architecture allows the business to automatically capture orders, manage internal operations, communicate with customers throughout the fulfillment lifecycle, and collect feedback after order completion.

---

## Business Challenge

Before implementing automation, incoming orders required manual coordination across multiple communication channels. This created several operational inefficiencies:

- manual order tracking
- delayed response time
- fragmented communication between team members
- lack of centralized order records
- inconsistent customer communication
- difficulty collecting feedback after fulfillment

The business needed a system that could:

- automatically capture and store order data
- centralize operational records
- notify administrators when orders are received
- communicate order status to customers
- collect customer feedback after pickup

The goal was to build a **lightweight operational automation system using accessible tools rather than expensive enterprise software.**

---

## Solution

The Detroit Choices Automation System was designed as an **event-driven automation pipeline** connecting website order intake, database storage, workflow automation, CRM tracking, and customer communication.

The system works as follows:

1. A customer submits an order through the website.
2. The website stores the order data in a PostgreSQL database.
3. A webhook event sends the order information into Zapier.
4. Zapier processes the order and creates a CRM record in Google Sheets.
5. Internal notifications are sent to administrators via Telegram and email.
6. A confirmation email is sent to the customer.
7. As order status changes within the CRM, automated updates are sent to the customer.
8. Once the order is picked up, the system sends a follow-up thank-you message requesting feedback.

This automation pipeline allows the business to handle orders efficiently while providing a consistent customer experience.

---

## System Architecture

The system connects several components into a unified automation workflow:

Customer Website (Replit Application)  
→ PostgreSQL Order Database  
→ Webhook Event Trigger  
→ Zapier Automation Workflows  
→ Google Sheets CRM  
→ Internal Notifications (Telegram + Email)  
→ Customer Status Emails  
→ Post-Pickup Feedback Request

The PostgreSQL database acts as the primary data store for incoming orders, while Zapier orchestrates the operational workflow and communication processes.

---

## Malcolm Frank's Role

Malcolm Frank designed and implemented the automation system and its architecture.

Responsibilities included:

- designing the automation architecture
- building the website environment in Replit
- configuring PostgreSQL database storage
- managing environment variables and application secrets
- implementing webhook-based automation
- building the Google Sheets CRM structure
- designing Zapier workflows
- implementing internal alerting and customer messaging
- testing and refining the automation system

---

## Technology Stack

The system uses a practical combination of tools designed for fast implementation and operational reliability:

- Replit (application hosting and development environment)
- PostgreSQL database (order storage)
- environment variables for secrets and API keys
- webhooks for event-driven triggers
- Zapier for workflow automation
- Google Sheets as a lightweight CRM
- Telegram bot for internal alerts
- email automation for customer communication

---

## Business Impact

The automation system improved operations in several key ways:

- eliminated manual order entry
- created centralized order tracking
- improved internal coordination
- automated customer communication
- enabled structured feedback collection
- reduced administrative workload

This allowed the business to focus more on fulfillment and customer experience rather than manual coordination.

---

## Why This Case Study Matters

This project demonstrates practical automation architecture using modern workflow tools combined with traditional application components such as databases and web services.

The system integrates:

- application development
- database design
- event-driven automation
- CRM integration
- operational workflow design
- customer lifecycle messaging

It demonstrates how a lightweight automation stack can be used to create a scalable operational system for small businesses.