# ADR-0010: Multi-Tenant Architecture

- Status: Accepted
- Date: 2026-08-17
- Deciders: CIBL Core Architecture Team
- Supersedes: None
- Superseded by: None

---

# Context

CIBL is designed as a Digital Asset Infrastructure Platform serving multiple organizations simultaneously.

The platform supports:

- Banks
- Exchanges
- Payment Service Providers
- FinTechs
- Custodians
- Brokers
- Governments
- CBDC Operators
- Enterprises
- Embedded Finance Platforms
- White-label Wallet Providers
- Marketplace Operators

Each organization requires complete logical isolation while sharing the same infrastructure.

The platform must support:

- Millions of organizations
- Millions of wallets
- Billions of transactions
- Regional deployment
- Regulatory isolation
- Organization-specific configurations
- Independent billing
- Independent API keys
- Independent workflows
- Independent compliance policies

---

# Decision

CIBL adopts a hierarchical multi-tenant architecture.

Hierarchy:

Platform

↓

Organization

↓

Workspace

↓

Business Unit

↓

Application

↓

User

↓

Wallet

↓

Account

↓

Assets

Every object belongs to an Organization.

Organization is the primary security boundary.

---

# Tenant Model

Every database object includes:

- tenant_id
- organization_id
- workspace_id (optional)

Example

Organization

```
Bank A
```

Workspace

```
Retail
Corporate
Treasury
```

Business Unit

```
Payments
Custody
Trading
Settlement
```

---

# Organization Isolation

Every request carries

```
Organization ID
```

All services validate

```
Organization ownership
```

before processing.

Cross-organization access is forbidden unless explicitly configured.

---

# Tenant Identification

Tenant may be identified by:

- JWT Claims
- API Key
- OAuth Client
- mTLS Certificate
- Domain
- Organization Header

Example

```
X-Organization-ID
```

---

# Resource Ownership

Every resource belongs to exactly one tenant.

Examples

Wallet

```
organization_id
wallet_id
```

Ledger Account

```
organization_id
account_id
```

Transaction

```
organization_id
transaction_id
```

API Key

```
organization_id
key_id
```

Webhook

```
organization_id
webhook_id
```

---

# Identity Hierarchy

Platform Admin

↓

Organization Admin

↓

Workspace Admin

↓

Operator

↓

Developer

↓

Read-only User

Permissions inherit downward.

---

# Tenant Configuration

Each organization has independent configuration.

Examples

Supported Assets

```
BTC

ETH

USDC
```

Fiat

```
USD

EUR

AED
```

Settlement Rules

```
Instant

T+1

T+2
```

Risk Policies

```
Custom
```

Compliance Rules

```
Jurisdiction-specific
```

---

# Database Strategy

Primary architecture

Shared Database

Shared Schema

Tenant Isolation by Row-Level Security

Benefits

- Lower operational cost
- Easier scaling
- Shared migrations
- Unified analytics

Optional enterprise deployment

Dedicated Database

Dedicated Infrastructure

Dedicated Network

Dedicated HSM

---

# Row-Level Security

Every query is automatically filtered.

Example

Instead of

```
SELECT *
FROM wallets
```

Platform executes

```
SELECT *
FROM wallets
WHERE organization_id = current_organization
```

No service bypasses tenant filters.

---

# Storage Isolation

Files stored using

```
organization-id/path/file
```

Example

```
org-123

wallets/

kyc/

exports/

reports/
```

---

# Cache Isolation

Redis keys include organization.

Example

```
org:123:user:456

org:123:wallet:789

org:123:balances
```

---

# Queue Isolation

Events contain

```
organization_id
```

Consumers ignore unrelated tenants.

---

# Event Isolation

Example

```
PaymentCreated

Organization

Wallet

Asset

Amount

Timestamp
```

Consumers always verify organization ownership.

---

# Billing Isolation

Usage tracked separately.

Examples

API Calls

Transactions

Wallets

Custody

Storage

Reports

Webhooks

Streaming Connections

Invoices are generated per organization.

---

# API Isolation

API Keys belong to one organization.

API Key

↓

Organization

↓

Permissions

↓

Rate Limits

↓

Audit

API Keys cannot access another tenant.

---

# Secrets Management

Every organization has independent secrets.

Examples

Webhook Secrets

API Keys

OAuth Secrets

Signing Keys

Encryption Keys

---

# Compliance Isolation

Each organization configures

KYC

AML

Sanctions

Travel Rule

Risk Policies

Transaction Limits

Country Restrictions

---

# Asset Visibility

Organization chooses supported assets.

Example

Organization A

```
BTC
ETH
USDC
```

Organization B

```
Gold Token
Digital USD
Treasury Bond Token
```

Assets unavailable to an organization remain invisible.

---

# Workflow Isolation

Approval workflows are tenant-specific.

Example

Organization A

```
1 Approval
```

Organization B

```
4 Approvals
```

---

# Branding

Organizations customize

Logo

Theme

Email Templates

Notification Templates

Payment Pages

Hosted Checkout

Portal

Wallet UI

---

# White-label Support

Each tenant may expose

Custom Domain

Custom SSL

Custom Branding

Custom Email

Custom SDK Configuration

---

# Monitoring

Metrics include

organization_id

service

region

environment

Examples

Transactions/sec

Latency

Errors

Wallet Count

API Usage

Settlement Volume

---

# Regional Deployment

Organizations may specify

Primary Region

Secondary Region

Data Residency

Allowed Jurisdictions

---

# Disaster Recovery

Each tenant supports

Independent Backup

Independent Restore

Independent Export

Independent Audit

---

# Security

Tenant isolation is enforced by

Authentication

Authorization

Database Policies

Message Validation

Storage Policies

API Gateway

Audit Logging

---

# Scalability

Designed Capacity

Millions of Organizations

Hundreds of Millions of Wallets

Billions of Ledger Entries

Millions of API Keys

Millions of Webhooks

Global Multi-Region Deployment

---

# Consequences

Benefits

- Strong tenant isolation
- Enterprise-grade security
- White-label capability
- Independent compliance
- Horizontal scalability
- Efficient infrastructure utilization
- Simplified operations
- Regulatory flexibility

Trade-offs

- Additional filtering overhead
- More complex authorization model
- Increased metadata management
- Cross-tenant analytics require privileged services

---

# Related ADRs

ADR-0006 Event Driven Architecture

ADR-0008 Security Model

ADR-0009 API Design Principles

ADR-0011 Asset Model

ADR-0014 Compliance Engine

ADR-0017 Observability