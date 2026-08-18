# ADR-0016: Notification Architecture

- Status: Accepted
- Date: 2026-08-17
- Authors: CIBL Architecture Team

---

# Context

CIBL is a Digital Asset Infrastructure Platform serving banks, exchanges,
payment providers, fintech companies, enterprises, and regulated financial
institutions.

Every operation inside the platform produces important events that require
notification delivery.

Examples include:

- Payment completed
- Deposit confirmed
- Withdrawal approved
- Settlement completed
- Risk alert
- AML review
- KYC status changed
- Wallet created
- Address generated
- Organization invited
- API Key created
- Login detected
- MFA enabled
- Balance threshold reached
- Merchant payment received
- Invoice paid
- Subscription renewed
- Webhook delivery failed

Notification delivery must support:

- Real-time delivery
- Reliable retry
- Event fan-out
- Multi-channel communication
- Template localization
- Tenant customization
- Delivery tracking
- Regulatory notifications

Notification delivery must never block business execution.

---

# Decision

Notification becomes an independent microservice.

```
Business Event

↓

Notification Event

↓

Notification Service

↓

Channel Dispatcher

↓

Email
SMS
Push
Webhook
Slack
Teams
Discord
Internal Inbox
```

Business services only publish events.

Notification Service owns delivery.

---

# Notification Sources

Every service may publish notifications.

Examples:

Wallet Service

- WalletCreated
- WalletArchived

Ledger

- TransferCompleted
- ReversalCompleted

Payment

- PaymentSucceeded
- PaymentFailed

Settlement

- SettlementCompleted

Compliance

- KYCApproved
- KYCRejected

Risk

- TransactionFlagged

Exchange

- OrderFilled

Organization

- MemberInvited

Authentication

- PasswordChanged

API

- APIKeyCreated

Billing

- InvoicePaid

---

# Notification Categories

## Security

Examples

- Login detected
- Password changed
- MFA enabled
- API Key rotated

Priority

Critical

---

## Financial

Examples

- Deposit
- Withdrawal
- Transfer
- Settlement

Priority

High

---

## Compliance

Examples

- KYC approved
- AML review
- Sanctions alert

Priority

High

---

## Billing

Examples

- Invoice paid
- Subscription renewed

Priority

Normal

---

## Operational

Examples

- Maintenance
- Service degradation
- Scheduled downtime

Priority

Normal

---

## Marketing

Examples

- Product updates
- Promotions

Priority

Low

Can be disabled.

---

# Delivery Channels

Supported channels

- Email
- SMS
- Push Notification
- Webhook
- Slack
- Microsoft Teams
- Discord
- Internal Notification Center
- Mobile Inbox

Future

- WhatsApp
- Telegram
- Apple Business Chat

---

# Notification Preferences

Each user may configure

```
Email

Enabled

SMS

Disabled

Push

Enabled

Webhook

Enabled
```

Granularity

```
Security

Email + SMS

Payments

Email + Push

Marketing

Disabled
```

---

# Localization

Templates are localized.

```
en

payment_completed.html

fr

payment_completed.html

ar

payment_completed.html
```

Uses CIBL i18n package.

---

# Template Engine

Supports

Variables

```
{{customerName}}

{{amount}}

{{currency}}

{{transactionId}}
```

Conditional blocks

Loops

Formatting helpers

Currency formatting

Date formatting

Localization

---

# Delivery Pipeline

```
Event

↓

Template Selection

↓

Localization

↓

Rendering

↓

Queue

↓

Dispatcher

↓

Provider

↓

Delivery Receipt
```

---

# Retry Strategy

Transient failures

```
30 sec

2 min

10 min

30 min

2 hr

6 hr
```

Permanent failures

No retry.

---

# Dead Letter Queue

Messages exceeding retry policy move to DLQ.

Operations may replay.

---

# Idempotency

Every notification owns

Notification ID

Duplicate requests are ignored.

---

# Rate Limiting

Supports

Per tenant

Per user

Per destination

Per provider

Example

```
Email

1000/hour

SMS

100/hour
```

---

# Provider Abstraction

Providers are interchangeable.

Email

- SES
- SendGrid
- Mailgun

SMS

- Twilio
- MessageBird

Push

- Firebase
- APNS

---

# Internal Inbox

Platform keeps internal notifications.

Supports

Unread

Archive

Search

Priority

Expiration

---

# Notification Metadata

Includes

Notification ID

Tenant

User

Organization

Event Type

Priority

Channel

Status

Created Time

Delivered Time

Read Time

---

# Delivery Status

Possible states

Queued

Rendering

Sending

Delivered

Failed

Expired

Cancelled

---

# Webhook Notifications

Webhook notifications reuse webhook service.

Supports

Retry

Signature verification

Ordering

Replay

---

# Scheduling

Notifications may be scheduled.

Examples

Subscription reminder

Settlement reminder

Invoice reminder

Compliance expiration

---

# Escalation Rules

Critical notifications escalate.

Example

Push

↓

SMS

↓

Email

↓

Phone Integration

---

# Monitoring

Metrics

Notifications sent

Delivery latency

Provider latency

Failures

Retries

Bounce rate

Open rate

Click rate

Webhook failures

---

# Security

Templates are immutable after approval.

Sensitive fields are masked.

Secrets never appear in templates.

PII follows tenant privacy policy.

---

# Multi-Tenant Isolation

Templates

Preferences

History

Providers

Branding

All isolated by tenant.

---

# Branding

Tenant branding includes

Logo

Primary color

Footer

Email signature

Support links

Legal links

---

# Analytics

Reports

Daily volume

Delivery success

Channel distribution

Open rates

Click rates

Failure rates

Provider comparison

---

# Disaster Recovery

Notification history replicated.

Templates versioned.

Queues replicated.

No notification loss accepted.

---

# Consequences

Advantages

- Fully asynchronous
- High scalability
- Reliable delivery
- Multi-provider support
- Tenant branding
- Localization
- Retry resilience
- Event-driven integration
- Operational visibility
- Regulatory compliance

Trade-offs

- Additional infrastructure
- Queue management complexity
- Template lifecycle management
- Provider failover implementation