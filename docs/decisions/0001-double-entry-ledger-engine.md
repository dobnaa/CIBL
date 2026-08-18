# ADR 0001: Architecture and Design of the CIBL Double-Entry Ledger Engine

- Status: Accepted
- ADR: 0001
- Date: 2026-08-14
- Authors: CIBL Architecture Team
- Stakeholders:
  - Backend Engineering
  - Ledger Team
  - Blockchain Team
  - Security Team
  - Compliance Team
  - Finance Team

---


# Structure of the CIBL-double-entry-ledger-engine

1. Purpose
2. Scope
3. Why Double Entry
4. Accounting Principles
5. Ledger Philosophy
6. Design Goals
7. Non Goals

8. Core Concepts
   Accounts
   Journals
   Entries
   Transactions
   Posting
   Balance
   Pending
   Available
   Locked

9. Chart Of Accounts

10. Account Types
    Asset
    Liability
    Equity
    Revenue
    Expense
    Off Balance

11. Internal Accounts

12. External Accounts

13. Wallet Mapping

14. Fiat Mapping

15. Crypto Mapping

16. Stablecoin Mapping

17. CBDC Mapping

18. RWA Mapping

19. Merchant Mapping

20. Treasury Mapping

21. Liquidity Mapping

22. Exchange Mapping

23. Settlement Mapping

24. Journal Model

25. Posting Model

26. Posting Rules

27. Debit Credit Matrix

28. Multi Asset Ledger

29. Multi Currency Ledger

30. FX Accounting

31. Precision Rules

32. Decimal Rules

33. Atomic Posting

34. ACID Requirements

35. Idempotency

36. Reversal

37. Cancellation

38. Compensation

39. Pending Transactions

40. Holds

41. Reservations

42. Settlement

43. Clearing

44. Reconciliation

45. Daily Close

46. End Of Day

47. Month Close

48. Year Close

49. Audit

50. Audit Trail

51. Immutable Ledger

52. Hash Chain

53. Event Sourcing

54. CQRS

55. Snapshots

56. Read Models

57. Write Models

58. Performance

59. Sharding

60. Partitioning

61. High Availability

62. Replication

63. Disaster Recovery

64. APIs

65. Events

66. Security

67. Authorization

68. Monitoring

69. Metrics

70. Alerting

71. Examples

72. Sequence Diagrams

73. Failure Scenarios

74. Recovery Scenarios

75. Testing Strategy

76. Migration Strategy

77. Future Extensions

---

# Context

The General Ledger is the financial heart of the CIBL Digital Asset Infrastructure Platform.

Every movement of value across every supported asset class MUST ultimately be represented as balanced accounting entries within the ledger.

Unlike blockchain ledgers, which record asset ownership on external networks, the CIBL Ledger represents the authoritative internal financial state of the platform.

The Ledger is therefore the single financial source of truth for:

- Wallet balances
- Custody balances
- Merchant balances
- Customer balances
- Treasury balances
- Exchange balances
- Liquidity providers
- Settlement accounts
- Fee accounts
- Revenue accounts
- Suspense accounts
- Reserve accounts
- Escrow accounts
- Clearing accounts

No component is allowed to mutate balances directly.

Balances are derived exclusively from immutable postings.

---

# Decision

CIBL SHALL implement a financial-grade immutable Double Entry Ledger.

The ledger SHALL satisfy:

- Atomicity
- Consistency
- Durability
- Immutability
- Auditability
- Deterministic Replay
- Idempotency
- Multi-Tenant Isolation
- Multi-Asset Support

Every financial operation SHALL generate balanced journal entries.

The accounting equation MUST always hold.

```

Assets = Liabilities + Equity

```

No exception is permitted.



## Accounting Model

The CIBL Ledger SHALL implement a strict double-entry accounting model.

Every financial event MUST create one or more journal entries.

Each journal SHALL contain two or more postings.

The sum of all debit postings MUST equal the sum of all credit postings.

The following invariant SHALL always hold:

Σ Debit = Σ Credit

A journal that violates this rule SHALL be rejected.

No partial journal SHALL ever be committed.

---

## Ledger Hierarchy

The ledger SHALL consist of the following hierarchy.

Ledger

└── Journal

    └── Posting

        └── Account

Every posting belongs to exactly one journal.

Every journal belongs to exactly one tenant.

Every account belongs to exactly one chart of accounts.

---

## Chart of Accounts

Each tenant SHALL own an isolated Chart of Accounts.

The default account categories include:

Assets

Liabilities

Equity

Revenue

Expenses

Off-Balance Accounts

Operational Accounts

Suspense Accounts

Reserve Accounts

Escrow Accounts

Fee Accounts

Treasury Accounts

Clearing Accounts

Settlement Accounts

Blockchain Accounts

Merchant Accounts

Customer Accounts

Custody Accounts

Liquidity Accounts

Exchange Accounts

---

## Account Structure

Each account SHALL expose the following attributes.

Account ID

Tenant ID

Asset ID

Currency

Account Type

Account Class

Parent Account

Status

Normal Balance

Metadata

Created At

Updated At

Version

An account SHALL never change its accounting class after creation.

---

## Account Types

Supported account types include:

Customer Wallet

Merchant Wallet

Treasury Wallet

Liquidity Pool

Custody Vault

Exchange Inventory

Settlement Account

Fee Revenue

Fee Expense

Reserve

Escrow

Suspense

Blockchain Bridge

Internal Clearing

External Clearing

Operational

System

Each account type SHALL have predefined posting rules.

---

## Posting Model

Every posting SHALL contain:

Posting ID

Journal ID

Account ID

Direction

Amount

Asset

Currency

Exchange Rate

Reference

Correlation ID

Transaction ID

Booking Date

Value Date

Created At

Metadata

Postings SHALL be immutable.

Once committed they SHALL never change.

Corrections SHALL always occur through compensating journals.

---

## Debit and Credit Rules

Each account category defines its normal balance.

Assets

Debit increases

Credit decreases

Liabilities

Credit increases

Debit decreases

Equity

Credit increases

Debit decreases

Revenue

Credit increases

Debit decreases

Expenses

Debit increases

Credit decreases

The posting engine SHALL validate every operation before commit.

Invalid accounting directions SHALL be rejected.

---

## Journal Lifecycle

A journal progresses through the following lifecycle.

Draft

↓

Validated

↓

Committed

↓

Published

↓

Archived

Only committed journals affect balances.

Draft journals have no financial effect.

Archived journals remain immutable forever.

---

## Immutable History

Historical financial data SHALL never be edited.

The following operations are prohibited:

DELETE Journal

UPDATE Posting

UPDATE Amount

UPDATE Account Balance

Changing historical timestamps

Changing asset identifiers

Changing journal references

Every correction SHALL generate a brand new journal linked to the original journal.

The complete financial history SHALL therefore remain intact.



## Balance Engine

The Balance Engine is responsible for deriving account balances from immutable
ledger postings.

Balances SHALL NOT be treated as the source of truth.

Instead, balances are materialized views computed from committed postings.

The ledger SHALL always be capable of reconstructing balances by replaying the
entire journal history.

This guarantees deterministic financial state reconstruction.

---

## Balance Types

Each account SHALL maintain multiple balance dimensions.

### Posted Balance

Represents the total committed balance.

Only committed journal entries contribute.

Example

Posted Balance

USD 125,000.25

---

### Pending Balance

Represents transactions awaiting completion.

Examples

- Card Authorization
- Blockchain Confirmation
- Settlement Window
- ACH Processing
- Wire Transfer

Pending balances SHALL automatically expire or resolve.

---

### Available Balance

Represents spendable funds.

Formula

Available Balance

=

Posted Balance

-

Reserved Balance

-

Pending Debit

+

Pending Credit

---

### Reserved Balance

Reserved funds cannot be spent.

Reservation examples include:

- Exchange Orders
- Payment Authorization
- Liquidity Reservation
- OTC Trades
- Smart Contract Escrow
- Compliance Hold
- Court Order
- Treasury Allocation

Reservations SHALL never modify Posted Balance.

---

### Locked Balance

Locked funds cannot move until released.

Examples

AML Investigation

Sanctions Review

Fraud Investigation

Judicial Freeze

Administrative Lock

Operational Freeze

---

### Total Balance

Total Balance

=

Posted

+

Pending Credit

-

Pending Debit

Displayed for reporting only.

Financial settlement SHALL use Posted Balance.

---

## Balance Calculation

Balances SHALL be computed using immutable postings.

No balance field SHALL ever become authoritative.

Materialized balance caches MAY exist for performance.

Whenever cache inconsistency is detected:

Replay Journal

↓

Recalculate Balance

↓

Replace Cache

This guarantees correctness.

---

## Balance Snapshot

Periodic snapshots MAY be generated.

Purpose

- Reporting
- Faster Recovery
- Analytics
- Performance Optimization

Snapshots SHALL NOT replace journal history.

---

## Posting Engine

The Posting Engine validates every accounting movement before commit.

Validation includes

- Balanced journal
- Existing accounts
- Tenant isolation
- Currency validation
- Asset validation
- Decimal precision
- Frozen account validation
- Compliance restrictions
- Risk restrictions
- Duplicate detection
- Idempotency

Only after successful validation may the journal commit.

---

## Atomic Commit

Every journal SHALL commit atomically.

Valid

Debit

100 USD

Credit

100 USD

Committed

Invalid

Debit

100 USD

Credit

90 USD

Rejected

Partial commits SHALL NEVER occur.

---

## Posting Sequence

Receive Command

↓

Validate

↓

Resolve Accounts

↓

Check Policies

↓

Validate Precision

↓

Validate Balance

↓

Generate Journal

↓

Generate Postings

↓

Commit Transaction

↓

Publish Events

↓

Update Balance Cache

---

## Precision Rules

Every asset defines:

Minimum Unit

Decimal Places

Maximum Precision

Examples

USD

2 decimals

BTC

8 decimals

ETH

18 decimals

USDC

6 decimals

XAU Token

3 decimals

Operations violating asset precision SHALL fail validation.

---

## Negative Balance Policy

Accounts may individually define negative balance rules.

Supported modes

Never

Allowed

Credit Line

Temporary

Overdraft

Collateralized

Examples

Customer Wallet

Not Allowed

Treasury Account

Allowed

Liquidity Provider

Allowed

Reserve Account

Not Allowed

The policy engine validates every posting.

---

## Multi-Asset Accounting

Every posting references exactly one asset.

Examples

USD

EUR

BTC

ETH

USDC

Digital Dollar

Tokenized Gold

Apple Stock Token

Real Estate Token

Carbon Credit

Asset conversion SHALL NEVER occur inside the ledger.

Conversion belongs to Exchange or FX services.

The Ledger only records the accounting result.

---

## Cross Asset Transactions

Cross-asset transactions require multiple journals.

Example

Customer swaps

BTC

↓

USDC

Ledger

Journal A

BTC Debit

BTC Treasury Credit

Journal B

USDC Treasury Debit

USDC Customer Credit

Exchange pricing remains outside the Ledger.

---

## FX Accounting

Foreign exchange operations SHALL generate independent accounting entries.

Example

Debit

USD Account

Credit

FX Clearing

Debit

FX Clearing

Credit

EUR Account

Realized gains and losses SHALL be posted separately.

---

## Journal Integrity

Every journal SHALL include:

Journal ID

Tenant ID

Transaction ID

Correlation ID

Request ID

Actor

Timestamp

Business Context

Source Service

Ledger Version

Every field becomes immutable after commit.


## Idempotency

Every financial command SHALL be idempotent.

Clients MUST provide an Idempotency Key for all state-changing operations.

Examples:

- Create Payment
- Transfer Funds
- Reserve Balance
- Release Hold
- Settlement
- FX Conversion
- Withdrawal
- Deposit

The ledger SHALL persist the Idempotency Key together with:

- Tenant ID
- Request Hash
- Response
- Status
- Created Timestamp
- Expiration Timestamp

Repeated requests using the same Idempotency Key SHALL return the original response without creating additional journal entries.

Duplicate financial transactions are strictly prohibited.

---

## Concurrency Control

The ledger SHALL guarantee consistency under concurrent workloads.

Concurrent modifications to the same account SHALL be serialized.

Supported mechanisms include:

- Optimistic Locking
- Pessimistic Locking (when required)
- Version Checking
- Transaction Isolation
- Atomic Database Transactions

Lost updates SHALL never occur.

---

## Account Versioning

Every account SHALL contain a monotonically increasing version number.

Example

Account

Version: 148

↓

New Posting

↓

Version: 149

Concurrent writes using stale versions SHALL be rejected.

Example

Expected Version: 148

Current Version: 150

↓

409 Conflict

↓

Retry Required

---

## Transaction Isolation

Ledger transactions SHALL execute using strict database isolation.

Minimum recommended isolation level:

REPEATABLE READ

Preferred level for critical posting operations:

SERIALIZABLE

Dirty Reads

NOT ALLOWED

Non-repeatable Reads

NOT ALLOWED

Phantom Reads

NOT ALLOWED

---

## Distributed Transactions

The Ledger SHALL NOT participate in distributed two-phase commit (2PC).

Instead, cross-service consistency SHALL be achieved using:

- Domain Events
- Outbox Pattern
- Inbox Pattern
- Saga Pattern
- Compensating Transactions

The Ledger always commits first.

Downstream services react asynchronously.

---

## Event Publishing

After a successful journal commit, the Ledger SHALL publish immutable domain events.

Publishing SHALL occur through the Transactional Outbox Pattern.

Workflow

Commit Database Transaction

↓

Persist Outbox Record

↓

Background Publisher

↓

Event Bus

↓

Consumers

No event SHALL be published before the journal commit succeeds.

---

## Ledger Events

The Ledger SHALL emit standardized events.

Examples include:

JournalCreated

JournalCommitted

PostingCreated

BalanceUpdated

BalanceReserved

ReservationReleased

AccountCreated

AccountFrozen

AccountUnfrozen

JournalReversed

CorrectionPosted

LedgerSnapshotCreated

ReconciliationCompleted

Each event SHALL include:

- Event ID
- Event Type
- Tenant ID
- Correlation ID
- Causation ID
- Aggregate ID
- Aggregate Version
- Timestamp
- Producer
- Payload

Events SHALL be immutable.

---

## Reversal Engine

Committed journals SHALL NEVER be edited.

When a correction is required, the ledger SHALL generate a compensating journal.

Example

Original Journal

Debit Cash ............. 100 USD

Credit Revenue .........100 USD

Correction

Debit Revenue ..........100 USD

Credit Cash ............100 USD

The original journal remains unchanged.

The financial history remains complete.

---

## Correction Policy

Permitted actions:

✓ Reverse Journal

✓ Create Adjustment Journal

✓ Append Metadata

Forbidden actions:

✗ Delete Journal

✗ Edit Posting

✗ Change Amount

✗ Change Timestamp

✗ Change Account

Financial history SHALL remain append-only.

---

## Reconciliation Engine

The Ledger SHALL support automated reconciliation against external systems.

Supported reconciliation targets:

- Blockchain Networks
- Bank Statements
- Custody Providers
- Payment Gateways
- Liquidity Providers
- Exchanges
- Clearing Systems

Differences SHALL produce reconciliation exceptions.

Exceptions SHALL require investigation.

---

## Recovery and Replay

The ledger SHALL support deterministic replay.

Recovery workflow

Load Snapshot

↓

Replay Journals

↓

Rebuild Balances

↓

Verify Integrity

↓

Resume Processing

Replay SHALL always produce identical balances.

Any mismatch SHALL trigger an integrity alert.

---

## Ledger API Principles

The Ledger API SHALL expose only accounting operations.

Examples:

POST /journals

POST /reversals

POST /reservations

POST /holds

POST /accounts

GET /accounts/{id}

GET /balances

GET /journals

GET /postings

Business workflows SHALL NOT manipulate balances directly.

All financial state changes MUST occur through accounting commands.



## Multi-Tenant Ledger

The CIBL Ledger SHALL support strict tenant isolation.

Each journal, posting, account, balance, reservation, and audit record SHALL belong to exactly one tenant.

Every request SHALL carry a Tenant Context.

Tenant Context includes:

- Tenant ID
- Organization ID
- Environment
- Region
- Regulatory Domain

Cross-tenant postings are strictly prohibited.

Shared infrastructure SHALL NOT imply shared financial state.

---

## Multi-Currency Accounting

The Ledger SHALL support accounting in any fiat currency.

Examples include:

- USD
- EUR
- GBP
- CHF
- AED
- SAR
- JPY
- CNY
- INR
- CAD
- AUD
- BRL
- MXN

Each posting SHALL reference exactly one accounting currency.

Foreign exchange operations SHALL be represented as independent accounting transactions.

Exchange rates SHALL never be embedded into ledger logic.

Rates SHALL be supplied by the FX Service.

---

## Multi-Asset Ledger

The Ledger SHALL be asset-agnostic.

Supported asset classes include:

- Fiat Currency
- Cryptocurrency
- Stablecoin
- CBDC
- Security Token
- Tokenized Bond
- Tokenized Fund
- Tokenized Equity
- Tokenized Commodity
- Tokenized Gold
- Tokenized Real Estate
- Carbon Credit
- NFT
- Loyalty Point
- Internal Settlement Token

Every asset SHALL define:

- Asset ID
- Symbol
- Precision
- Decimal Places
- Settlement Rules
- Transfer Rules
- Compliance Policy
- Risk Policy

The Ledger SHALL never contain asset-specific business logic.

---

## Scalability

The Ledger SHALL scale horizontally.

Scalability strategies include:

- Read Replicas
- CQRS
- Event Streaming
- Database Partitioning
- Sharding
- Snapshotting
- Materialized Views
- Async Reporting
- Connection Pooling

The architecture SHALL support millions of journal entries per day.

Read workloads SHALL never block write workloads.

---

## Partitioning Strategy

Ledger data MAY be partitioned by:

- Tenant
- Region
- Time Period
- Asset
- Organization

Partitioning SHALL remain transparent to clients.

Financial integrity SHALL remain identical regardless of partition layout.

---

## High Availability

The Ledger SHALL operate in highly available configurations.

Supported deployment models:

- Active / Active
- Active / Passive

Automatic failover SHALL preserve committed financial data.

Recovery Point Objective (RPO)

0 for committed journals.

Recovery Time Objective (RTO)

As defined by deployment requirements.

---

## Disaster Recovery

Recovery SHALL support:

- Point-in-Time Recovery
- Snapshot Recovery
- Event Replay
- Journal Replay

Backups SHALL be encrypted.

Recovery procedures SHALL be periodically tested.

No committed journal SHALL be lost.

---

## Security Model

The Ledger SHALL implement defense in depth.

Security controls include:

- Mutual TLS
- OAuth2
- JWT Validation
- RBAC
- ABAC
- Encryption at Rest
- Encryption in Transit
- Secret Management
- Key Rotation
- Audit Logging

Only authorized services MAY create accounting journals.

Direct database access SHALL never bypass business validation.

---

## Regulatory Compliance

The Ledger SHALL satisfy enterprise financial compliance requirements.

Supported regulatory objectives include:

- SOX
- PCI DSS
- ISO 27001
- SOC 2
- GDPR
- FATF
- AML
- Travel Rule

The Ledger SHALL provide complete financial traceability.

Every journal SHALL be attributable to:

- User
- Organization
- Service
- API Request
- Correlation ID
- Timestamp

---

## Monitoring

The Ledger SHALL expose operational metrics.

Examples include:

- Journals per Second
- Postings per Second
- Commit Latency
- Replay Duration
- Reconciliation Failures
- Reservation Count
- Balance Cache Hit Ratio
- Dead Letter Queue Size
- Event Publication Latency

Metrics SHALL integrate with OpenTelemetry.

---

## Audit

Every accounting action SHALL be auditable.

Audit records SHALL include:

- Who
- What
- When
- Where
- Why
- Correlation ID
- Previous State
- New State

Audit data SHALL remain immutable.

---

## Alternatives Considered

### Mutable Balance Ledger

Rejected.

Reason:

Historical reconstruction becomes unreliable.

---

### Single Entry Accounting

Rejected.

Reason:

Financial inconsistency.

Insufficient auditability.

---

### Blockchain as Internal Ledger

Rejected.

Reason:

External blockchains cannot satisfy all internal accounting requirements.

Settlement latency.

Operational cost.

Limited flexibility.

---

### Event Store Only

Rejected.

Reason:

Accounting semantics remain insufficient without double-entry principles.

---

## Consequences

Positive

- Financial correctness
- Deterministic replay
- Regulatory readiness
- Complete auditability
- Multi-tenant isolation
- Multi-asset support
- High scalability
- Vendor independence
- Immutable history
- Enterprise-grade reliability

Trade-offs

- Increased implementation complexity
- Higher operational overhead
- Additional storage requirements
- More sophisticated reconciliation workflows

These trade-offs are accepted because financial integrity is the highest architectural priority.

---

# Final Decision

CIBL SHALL adopt an immutable, append-only, double-entry accounting engine as the single financial source of truth for all assets managed by the platform.

Every movement of value, regardless of asset type or business domain, SHALL ultimately be represented as balanced journal postings.

No service SHALL modify balances directly.

Balances SHALL always be derived from immutable accounting records.

This decision is permanent unless superseded by a future accepted Architecture Decision Record.



---

# Fundamental Principles

The ledger SHALL follow these immutable principles.

1. No mutable balances

2. No balance overwrite

3. No direct balance updates

4. No account deletion

5. No journal modification

6. No posting modification

7. No historical mutation

8. Every transaction balanced

9. Every transaction auditable

10. Every transaction replayable

---

# Ledger Responsibilities

The Ledger owns only financial state.

It SHALL NOT perform:

- Blockchain communication
- Wallet generation
- Signature generation
- KYC
- AML
- Pricing
- Exchange execution
- Risk scoring

Instead, other services submit accounting commands.

The ledger validates and commits them.


# Ledger Account Model

The Ledger Engine organizes all financial records using a hierarchical Chart of Accounts (CoA).

Each account represents a financial container that participates in one or more double-entry transactions.

An account never stores business logic. It only represents financial state.

## Account Categories

The Ledger Engine supports the following account categories:

- Assets
- Liabilities
- Equity
- Revenue
- Expenses
- Off-Balance Accounts
- Memorandum Accounts
- Clearing Accounts
- Suspense Accounts

Every account belongs to exactly one category.

---

# Account Hierarchy

Accounts may be organized hierarchically.

Example:
```
Assets
├── Cash
├── Bank Accounts
├── Fiat Wallets
│   ├── USD
│   ├── EUR
│   └── GBP
├── Crypto Wallets
│   ├── Bitcoin
│   ├── Ethereum
│   └── Solana
└── Custody Assets
```
Hierarchy exists for reporting purposes only.

Posting always occurs on leaf accounts.

---

# Account Structure

Each account contains:

- Account ID
- Tenant ID
- Parent Account ID
- Account Code
- Account Name
- Account Type
- Account Category
- Asset Identifier
- Currency
- Status
- Created At
- Updated At
- Metadata

Account IDs are immutable.

---

# Account Status

Accounts may have one of the following states:

- Pending
- Active
- Frozen
- Suspended
- Closed
- Archived

Closed accounts cannot receive new postings.

Historical records remain accessible indefinitely.

---

# Asset Isolation

Each ledger account represents exactly one asset.

Examples:

Customer Wallet (USD)

Customer Wallet (EUR)

Customer Wallet (BTC)

Customer Wallet (USDC)

Different assets must never share the same ledger account.

---

# Multi-Currency Design

Currencies are isolated.

Examples:

USD Ledger

EUR Ledger

GBP Ledger

BTC Ledger

ETH Ledger

USDT Ledger

Cross-currency operations require dedicated conversion journals.

---

# Balance Model

Balances are derived from postings.

The Ledger Engine does not treat balances as the primary source of truth.

Balances may be materialized for performance optimization but must always be reproducible from postings.

---

# Available Balance

Available Balance represents spendable funds.

Formula:

Available Balance = Book Balance − Reserved Balance

Reserved amounts are maintained independently from posted balances.

---

# Book Balance

Book Balance equals:

Total Credits − Total Debits

or

Total Debits − Total Credits

depending on account category.

The accounting rule is determined by the account type.

---

# Pending Balance

Pending balances represent transactions that have been authorized but not settled.

Pending balances are excluded from finalized accounting reports.

---

# Reserved Balance

Reserved balances are used for:

- Payment Authorization
- Exchange Orders
- Settlement Locks
- Withdrawal Requests
- Compliance Holds
- Risk Controls

Reserved balances never modify posted ledger entries.

---

# Posting Rules

Every posting must satisfy:

- One asset
- One account
- One amount
- One direction
- One journal
- Immutable state

Postings cannot be updated after commitment.

---

# Journal Lifecycle

Every journal follows the same lifecycle:

Draft

↓

Validation

↓

Posting

↓

Committed

↓

Archived

A committed journal cannot return to a previous state.

---

# Journal Validation

Before commitment, the Ledger Engine validates:

- Debit equals Credit
- Valid Accounts
- Active Accounts
- Supported Assets
- Tenant Isolation
- Idempotency
- Accounting Rules
- Decimal Precision
- Posting Permissions

Any validation failure aborts the entire journal.

---

# Financial Invariants

The following invariants are mandatory:

- Total Debits equal Total Credits
- Negative postings are prohibited
- Zero-value postings are prohibited
- Every posting belongs to exactly one journal
- Every journal contains at least two postings
- Journals are immutable
- Postings are immutable
- Assets cannot be mixed
- Tenants cannot be mixed
- Ledger integrity is never compromised

Violation of any invariant results in transaction rejection.

---

# Transaction Atomicity

A journal is committed using a single atomic database transaction.

Either:

- All postings succeed

or

- None are persisted

Partial financial state is impossible.

---

# Ledger Consistency

The Ledger Engine guarantees:

- ACID Transactions
- Strong Consistency
- Deterministic Processing
- Repeatable Reads
- Serializable Financial State

Eventual consistency is never applied to ledger writes.

---

# Immutable Audit Trail

Every committed journal permanently records:

- Creation Time
- Author
- Service
- Correlation ID
- Trace ID
- Tenant
- Asset
- Metadata
- Related Business Object

Audit information is append-only.

Historical records are never deleted.

# Idempotency

## Overview

Financial operations must be executed exactly once, regardless of retries, network failures, duplicate requests, or client-side resubmissions.

The Ledger Engine implements strict idempotency guarantees for every write operation.

Idempotency is mandatory for:

- Deposits
- Withdrawals
- Transfers
- Exchange Orders
- Settlement Operations
- Merchant Payments
- Refunds
- Blockchain Synchronization
- Treasury Operations
- Fee Collection

---

# Idempotency Key

Every financial command MUST include an Idempotency Key.

Example:

```
Idempotency-Key:
2ef84641-d0d3-4428-b90d-91b1de66d203
```

The key must be unique within the configured retention period.

---

# Idempotency Storage

The Ledger stores:

- Idempotency Key
- Tenant ID
- Operation Type
- Request Hash
- Journal ID
- Processing Status
- Response Snapshot
- Creation Time
- Expiration Time

Duplicate requests never create new journals.

---

# Duplicate Request Handling

If an identical request is received:

1. Validate the Idempotency Key.
2. Compare the request hash.
3. If hashes match:
   - Return the original response.
4. If hashes differ:
   - Reject the request.

This prevents accidental replay with modified payloads.

---

# Journal Numbering

Every committed journal receives a globally unique identifier.

Properties:

- Immutable
- Monotonic
- Collision-free
- Audit-friendly

Journal identifiers are never reused.

---

# Posting Numbering

Every posting receives its own immutable identifier.

Posting IDs are globally unique.

A posting can never belong to multiple journals.

---

# Balance Calculation

Balances are calculated as:

```
Opening Balance
+ Credits
− Debits
= Current Balance
```

Balance computation is deterministic.

Running the same calculation multiple times always produces identical results.

---

# Balance Snapshot

To improve performance, periodic balance snapshots may be generated.

Snapshots are optimization artifacts only.

Deleting all snapshots must never affect accounting correctness.

Balances can always be reconstructed from postings.

---

# Snapshot Rules

Snapshots:

- Are immutable
- Are versioned
- Can be regenerated
- Never replace ledger postings

Snapshots cannot be manually edited.

---

# Ledger Events

Every successful journal generates immutable domain events.

Typical events include:

- JournalCreated
- JournalCommitted
- PostingCreated
- BalanceUpdated
- ReconciliationCompleted
- JournalReversed

Events are emitted only after successful commitment.

---

# Event Ordering

Ledger events preserve causal ordering.

Within a journal:

1. Journal Created
2. Posting Created
3. Journal Committed
4. Balance Updated
5. Downstream Events

Consumers must process events in sequence.

---

# Reversal

Committed journals cannot be modified.

Corrections are performed using reversal journals.

A reversal journal creates the opposite postings while preserving the original accounting history.

---

# Reversal Rules

A reversal:

- References the original journal
- Mirrors every posting
- Preserves audit history
- Never deletes records
- Creates a new immutable journal

The original journal remains unchanged.

---

# Adjustment Journals

Business corrections that are not exact reversals require adjustment journals.

Adjustment journals:

- Produce new postings
- Reference affected journals
- Maintain a complete audit trail
- Preserve accounting integrity

---

# Reconciliation

Reconciliation verifies that ledger balances remain consistent with external financial systems.

Supported reconciliation targets include:

- Blockchain Networks
- Banking Systems
- Payment Gateways
- Custody Providers
- Liquidity Providers
- Exchange Engines
- Treasury Systems

---

# Reconciliation Types

The Ledger Engine supports:

- Continuous Reconciliation
- Scheduled Reconciliation
- Manual Reconciliation
- Event-Driven Reconciliation

Every reconciliation generates an immutable report.

---

# Reconciliation Results

Possible outcomes:

- Matched
- Missing
- Unexpected
- Amount Mismatch
- Duplicate
- Timing Difference

Any mismatch generates an investigation record.

---

# Ledger Integrity Verification

The platform periodically verifies:

- Journal completeness
- Posting integrity
- Balance correctness
- Event consistency
- Referential integrity
- Account hierarchy validity

Integrity verification runs independently from business operations.

---

# Ledger Locking Strategy

Financial writes use optimistic concurrency where possible.

Critical operations may use pessimistic locking when strict serialization is required.

The locking strategy prevents:

- Lost Updates
- Double Spending
- Concurrent Balance Corruption

---

# Precision

Financial amounts are never stored using floating-point types.

Accepted representations include:

- Decimal128
- Arbitrary Precision Decimal
- BigInt with Scale

Floating-point arithmetic is prohibited.

---

# Rounding

Rounding follows asset-specific precision rules.

Examples:

- USD → 2 decimals
- BTC → 8 decimals
- ETH → 18 decimals
- USDC → 6 decimals

Precision rules are defined by the Asset Registry.

---

# Ledger Guarantees

The Ledger Engine guarantees:

- Strong Consistency
- Atomic Transactions
- Immutability
- Auditability
- Deterministic Processing
- Exactly-Once Posting
- Financial Correctness
- Replay Safety
- Tenant Isolation
- Asset Isolation

These guarantees apply to every financial transaction processed by the CIBL platform.


# Posting Engine

## Overview

The Posting Engine is the core execution component of the Ledger Engine.

Its responsibility is to transform validated business commands into immutable accounting postings while preserving all financial invariants.

The Posting Engine never performs business validation. It only executes accounting operations that have already been authorized by upstream services.

Responsibilities include:

- Journal Creation
- Posting Generation
- Balance Calculation
- Atomic Commit
- Event Publication
- Audit Recording

---

# Posting Pipeline

Every financial operation follows the same execution pipeline.

```

Business Command

↓

Authorization

↓

Compliance Validation

↓

Risk Validation

↓

Ledger Validation

↓

Journal Creation

↓

Posting Generation

↓

Atomic Commit

↓

Balance Update

↓

Ledger Events

↓

External Event Bus

```

No step may be skipped.

---

# Posting Lifecycle

Each posting transitions through the following lifecycle:

```

Created

↓

Validated

↓

Persisted

↓

Committed

↓

Published

↓

Archived

```

Committed postings are immutable.

---

# Posting Structure

Each posting contains:

- Posting ID
- Journal ID
- Tenant ID
- Ledger Account ID
- Asset ID
- Direction
- Amount
- Effective Time
- Booking Time
- Reference ID
- Correlation ID
- Trace ID
- Metadata

All fields except metadata are immutable.

---

# Posting Direction

Only two posting directions exist.

- Debit
- Credit

No additional posting types are allowed.

Business meaning is derived from the affected account category.

---

# Posting Validation Rules

Before persistence the Posting Engine validates:

- Journal exists
- Account exists
- Account is active
- Asset matches account
- Currency matches asset
- Decimal precision is valid
- Amount is positive
- Tenant ownership is valid
- Journal remains balanced

Failure of any validation rejects the complete transaction.

---

# Journal Builder

The Journal Builder transforms business commands into accounting entries.

Example:

Customer deposits 100 USD.

Generated journal:

Debit

Customer Cash Account

100 USD

Credit

Platform Liability Account

100 USD

The Journal Builder contains no persistence logic.

---

# Journal Commit

Journal commitment occurs within a single database transaction.

The commit process includes:

1. Insert Journal
2. Insert Postings
3. Update Balances
4. Write Audit Record
5. Publish Events

Either every operation succeeds or none are persisted.

---

# Event Publication

Ledger events are published only after successful database commitment.

This guarantees that downstream consumers never receive events for failed financial operations.

The recommended implementation uses the Transactional Outbox Pattern.

---

# Transactional Outbox

Each committed journal creates an outbox record.

The Event Dispatcher publishes outbox records asynchronously.

Advantages include:

- No lost events
- Retry support
- Exactly-once publication
- Database consistency
- Message durability

---

# Posting Ordering

Postings inside a journal maintain deterministic ordering.

Ordering is based on:

1. Journal Sequence
2. Posting Sequence

Consumers must preserve ordering during replay.

---

# Batch Posting

The Ledger Engine supports batch journal execution.

A batch consists of multiple journals executed within a controlled workflow.

Failure strategies include:

- Fail Fast
- Continue On Error
- Compensating Batch

The strategy is configurable by workflow.

---

# Cross-Service Posting

External services never write directly into ledger tables.

Instead they submit financial commands through the Ledger API.

Example producers:

- Wallet Service
- Payment Service
- Exchange Service
- Settlement Service
- Custody Service
- Treasury Service
- Merchant Service

The Ledger remains the only accounting authority.

---

# Ledger API Contract

The Posting Engine exposes command-oriented APIs.

Examples include:

- CreateJournal
- CommitJournal
- ReverseJournal
- ReserveFunds
- ReleaseFunds
- GetBalance
- GetJournal
- GetPosting
- ListTransactions

The API does not expose direct table access.

---

# Failure Handling

Failures are categorized as:

- Validation Failure
- Business Rule Failure
- Database Failure
- Network Failure
- Infrastructure Failure
- Duplicate Request

Each category has deterministic retry behavior.

---

# Retry Policy

Retryable failures include:

- Temporary Database Errors
- Network Interruptions
- Message Broker Failures

Non-retryable failures include:

- Validation Errors
- Unbalanced Journals
- Invalid Accounts
- Closed Accounts
- Duplicate Journals

Retries must always preserve idempotency.

---

# Performance Objectives

Target performance goals:

- Journal Validation < 5 ms
- Posting Generation < 2 ms
- Database Commit < 20 ms
- Balance Query < 10 ms
- Event Publication < 100 ms

These values represent target objectives and may vary depending on deployment architecture.

---

# Scalability

The Posting Engine is stateless.

Horizontal scaling is achieved by deploying multiple identical instances behind a load balancer.

No in-memory financial state may exist.

Persistent state resides exclusively within the Ledger database.

---

# Security Considerations

Every posting operation requires:

- Authenticated Service Identity
- Authorized Scope
- Correlation ID
- Trace ID
- Audit Context
- Immutable Request Log

All communication must use TLS.

Sensitive metadata must be encrypted at rest.

---

# Operational Principles

The Ledger Engine is considered a Tier-0 service.

Operational requirements include:

- Zero data loss
- Continuous backups
- High availability
- Disaster recovery support
- Continuous integrity verification
- Immutable audit logs
- Full observability

The Ledger Engine must remain available independently of downstream services.


11. Journal Lifecycle
# Journal States
```
DRAFT
    │
    ▼
VALIDATING
    │
    ▼
READY
    │
    ▼
POSTING
    │
    ▼
POSTED
    │
    ├─────────────► REVERSED
    │
    └─────────────► FAILED
```
# State Descriptions
DRAFT
Journal is created but not yet validated.
No balance is affected.
# VALIDATING
•System performs:
•account existence validation
•tenant validation
•currency validation
•balance rule validation
•posting rule validation
•idempotency validation

# READY
Journal is accepted.
Waiting for posting.
# POSTING
Atomic transaction begins.
Debit/Credit rows are inserted.
Balances are updated.
Audit events emitted.
# POSTED
Ledger mutation completed.
Journal becomes immutable.
# FAILED
Posting aborted.
Nothing committed.
# REVERSED
Compensating journal generated.
Original journal preserved forever. 

12. Posting Algorithm
Pseudo code
CreateJournal()

↓

Validate()

↓

CheckIdempotency()

↓

BeginDatabaseTransaction()

↓

InsertJournal()

↓

InsertDebitPostings()

↓

InsertCreditPostings()

↓

UpdateBalances()

↓

CreateAuditEvent()

↓

Commit()

↓

PublishLedgerPostedEvent()
13. Posting Rules
Every journal must satisfy:
Σ Debit == Σ Credit
Every posting:
Amount > 0
Currency:
Same Journal
↓

Single Currency
Cross-currency transfers require:
Exchange Journal

+

Settlement Journal

+

FX Journal
Never mixed.
14. Account Categories
ASSET

LIABILITY

EQUITY

REVENUE

EXPENSE

OFF_BALANCE
Each account has immutable category.
15. Balance Types
Supported:
Current Balance

Available Balance

Pending Balance

Reserved Balance

Locked Balance
Formula
Available

=

Current

-

Reserved

-

Locked

+

PendingCredits

-

PendingDebits
16. Account Status
ACTIVE

FROZEN

LOCKED

SUSPENDED

CLOSED
Rules
ACTIVE
Normal operations.
FROZEN
Debit prohibited.
Credit optional.
LOCKED
No posting allowed.
SUSPENDED
Temporary administrative state.
CLOSED
Historical only.
No mutation.
17. Posting Constraints
System validates:
Account Exists
Currency Matches
Tenant Matches
Not Closed
Not Locked
Positive Amount
Balanced Journal
Duplicate Request
Overflow
Precision
Frozen Rules
Compliance Lock
Risk Lock
Settlement Lock
18. Immutable History
Ledger never updates:
Journal

Posting

Balance Snapshot

Audit Record
Updates create:
Correction Journal

or

Reversal Journal
Never overwrite.
19. Balance Snapshots
Snapshots generated:
Hourly

Daily

Monthly

Yearly

On-demand
Snapshots include:
Balance

Available

Reserved

Pending

Hash

Timestamp
Useful for:
auditing
reconciliation
reporting
disaster recovery
20. Idempotency
Every mutation contains:
Idempotency-Key
Flow
Incoming Request

↓

Lookup Key

↓

Exists?

Yes → Return Previous Result

No → Execute
Guarantees:
no duplicate transfer
safe retries
distributed consistency












# 1. Purpose

This Architecture Decision Record defines the accounting model used by CIBL.

The Ledger Engine is the financial source of truth for every asset managed by CIBL.

Every movement of value—whether Fiat, Cryptocurrency, Stablecoin, Internal Credit,
Fees, Treasury Transfers, Settlement, Escrow, or Smart Contract operations—must be
represented as immutable double-entry journal postings.

No balance is stored as mutable business state.

Balances are always derived from ledger postings or from validated snapshots.

---

# 2. Scope

This ADR applies to:

• Internal Wallets

• External Wallets

• Customer Accounts

• Merchant Accounts

• Treasury Accounts

• Settlement Accounts

• Fee Accounts

• Escrow Accounts

• Liquidity Pools

• Stablecoin Reserve

• Blockchain Bridges

• Fireblocks Wallets

• Cold Wallets

• Hot Wallets

• Smart Contracts

• Internal Accounting

---

# 3. Goals

The Ledger Engine must provide:

• Mathematical correctness

• Auditability

• Traceability

• Immutability

• Scalability

• Multi-currency support

• Multi-chain support

• Regulatory compliance

• Fraud resistance

• Disaster recovery

---

# 4. Non Goals

The Ledger is NOT responsible for:

• Blockchain synchronization

• Wallet generation

• Signing transactions

• RPC communication

• Price feeds

• KYC

• AML

• Authentication

Those concerns belong to their own services.

The Ledger records financial facts only.

---

# 5. Core Principles

The Ledger follows the following principles.

## Principle 1

Money cannot be created.

## Principle 2

Money cannot disappear.

## Principle 3

Every Debit has exactly one or more Credits.

## Principle 4

Every Credit has exactly one or more Debits.

## Principle 5

The journal must balance.

Debit Total = Credit Total

Always.

Without exception.

---

# 6. Accounting Equation

For every Journal:

Σ Debit = Σ Credit

Equivalent:

Σ Debit − Σ Credit = 0

This invariant is absolute.

Violation means database corruption or software defect.

The transaction MUST fail immediately.

---

# 7. Immutability

Ledger records are immutable.

The following operations are forbidden:

UPDATE ledger_entries

DELETE ledger_entries

Updating monetary values

Changing account references

Changing asset references

Changing posting direction

Changing posting amount

Historical records never change.

Errors are corrected only by creating a compensating journal.

---

# 8. Source of Truth

The Ledger is the financial source of truth.

Wallet balances

Merchant balances

Treasury balances

Reserve balances

Settlement balances

must all originate from Ledger postings.

No independent mutable balance tables are allowed unless explicitly documented as cache.

---

# 9. Supported Assets

The ledger supports:

Fiat

USD

EUR

GBP

AED

JPY

...

Native Blockchain Assets

BTC

ETH

TRX

BNB

SOL

MATIC

AVAX

...

Tokens

ERC20

TRC20

SPL

BEP20

Stablecoins

USDT

USDC

DAI

USDBL

NFT assets

ERC721

ERC1155

Future assets may be added without changing accounting principles.

---

# 10. Multi-Asset Isolation

Every account belongs to exactly one asset.

Examples

Customer BTC Wallet

Customer ETH Wallet

Customer USD Wallet

Customer USDT Wallet

Each account has exactly one Asset ID.

Assets must never mix.

BTC cannot be transferred into an ETH account.

USDT cannot appear in a BTC ledger account.

The database enforces this invariant.

---

# 11. Currency Precision

Amounts are stored using

NUMERIC(36,18)

No floating point values.

No DOUBLE.

No FLOAT.

No REAL.

All calculations use arbitrary precision decimal arithmetic.

---

# 12. Idempotency

Every financial operation requires a globally unique reference_id.

Examples:

Payment

Withdrawal

Deposit

Settlement

Webhook

Blockchain confirmation

Retry

Duplicate requests using the same reference_id must not generate new journals.

Instead the existing journal must be returned.

---

# 13. Journal Lifecycle

A journal may exist in one of the following states.

PENDING

↓

POSTED

↓

REVERSED

FAILED journals never affect balances.

POSTED journals become immutable.

REVERSED journals remain immutable and are compensated by another journal.

---

# 14. Ledger Accounts

A Ledger Account represents a single accounting container for exactly one asset.

Each account belongs to one owner and one asset.

An account may represent:

- Customer Wallet
- Merchant Wallet
- Treasury Wallet
- Settlement Account
- Liquidity Pool
- Escrow
- Revenue Account
- Expense Account
- Blockchain Hot Wallet
- Blockchain Cold Wallet
- Fireblocks Vault
- Internal Clearing Account

Every account has a unique account number.

Account numbers never change.

Closed accounts remain in the database permanently.

Historical records are never deleted.

---

# 15. Posting Rules

Each Journal contains one or more Ledger Entries.

Minimum entries:

- 2

Example

Debit

Customer BTC Wallet

1 BTC

Credit

Treasury BTC Wallet

1 BTC

Balanced.

More complex examples may contain many entries.

Example:

Customer Payment

Debit Customer Wallet

100 USD

Credit Merchant

97 USD

Credit Platform Fee

2 USD

Credit Tax

1 USD

Still balanced.

100 Debit

100 Credit

---

# 16. Transaction Lifecycle

A financial transaction follows these stages.

1.

Request received

↓

2.

Validation

↓

3.

Authorization

↓

4.

Business Rules

↓

5.

Database Transaction Begins

↓

6.

Ledger Journal Created

↓

7.

Ledger Entries Created

↓

8.

Balance Verification

↓

9.

Commit

↓

10.

Domain Events Published

↓

11.

Notification

↓

12.

Audit Log

No external side effects occur before database commit.

---

# 17. Database Transactions

Every posting executes inside a single ACID transaction.

Required guarantees

Atomicity

Consistency

Isolation

Durability

Partial commits are forbidden.

---

# 18. Isolation Level

Default:

SERIALIZABLE

Alternative:

REPEATABLE READ

combined with

SELECT ...

FOR UPDATE

where required.

READ COMMITTED is insufficient for financial operations.

---

# 19. Concurrency Strategy

Simultaneous writes to the same ledger account must never corrupt balances.

When posting:

Lock affected accounts.

Validate balances.

Insert journal.

Insert entries.

Commit.

Deadlocks must be detected.

Transactions may retry automatically.

---

# 20. Idempotent Processing

Every external request carries:

reference_id

Examples

withdrawal

deposit

block confirmation

payment

refund

webhook

retry

If the same reference_id already exists

Return existing result.

Never create duplicate journals.

---

# 21. Compensating Journals

Historical data cannot be modified.

Mistakes are corrected by creating another journal.

Example

Incorrect

Debit

100

Correct amount

90

Solution

Create reversal

Credit

100

Create new journal

Debit

90

History remains complete.

---

# 22. Reversals

Reversal rules

Original journal remains immutable.

Original journal remains visible.

New journal references original journal.

Audit chain remains intact.

Money movement is mathematically reversed.

---

# 23. Snapshots

Balances may be reconstructed from:

Ledger Entries

or

Ledger Snapshot + Entries after Snapshot

Snapshots are performance optimizations only.

Snapshots never replace journal history.

Snapshots may be rebuilt at any time.

---

# 24. Balance Calculation

Balance is calculated as

Assets

Debit − Credit

Liabilities

Credit − Debit

Revenue

Credit − Debit

Expenses

Debit − Credit

Equity

Credit − Debit

The calculation rules never vary.

---

# 25. Precision

Floating point arithmetic is forbidden.

Examples

❌ float

❌ double

❌ JavaScript Number for financial persistence

Allowed

Decimal

BigInt (when appropriate)

NUMERIC(36,18)

All calculations must be deterministic.

---

# 26. Multi-Currency

Every Ledger Entry belongs to exactly one asset.

Examples

BTC

ETH

USDT

USDC

USD

EUR

USDBL

Assets cannot mix.

Cross-currency operations require exchange journals.

Example

Debit

USD

Credit

BTC

Exchange Rate

Recorded separately.

---

# 27. Exchange Operations

Currency conversion is modeled as multiple journals.

Example

Customer buys BTC.

Journal 1

Debit Customer USD

Credit Exchange USD

Journal 2

Debit Exchange BTC

Credit Customer BTC

Exchange rates are immutable metadata.

They never overwrite historical data.

---

# 28. Error Handling

The Ledger Engine must fail safely.

If any invariant cannot be satisfied:

- Roll back the entire database transaction.
- Persist nothing.
- Return a localized error.
- Write an audit log.
- Emit an operational metric.

Financial correctness is always preferred over availability.

---

# 29. Error Codes

Every ledger error exposes a stable internal code.

Examples

LEDGER_UNBALANCED_JOURNAL

LEDGER_ACCOUNT_NOT_FOUND

LEDGER_ASSET_MISMATCH

LEDGER_DUPLICATE_REFERENCE

LEDGER_ACCOUNT_CLOSED

LEDGER_NEGATIVE_AMOUNT

LEDGER_INVALID_DIRECTION

LEDGER_INVALID_ASSET

LEDGER_CONCURRENCY_CONFLICT

LEDGER_POSTED_JOURNAL_IMMUTABLE

Error messages returned to users are localized through translation keys.

Example

ledger.errors.unbalanced_entry

ledger.errors.asset_mismatch

ledger.errors.account_closed

---

# 30. Localization

The Ledger Engine never returns hard-coded human-readable messages.

Instead it returns:

Code

Translation Key

Parameters

Example

{
  "code": "LEDGER_ACCOUNT_CLOSED",
  "messageKey": "ledger.errors.account_closed",
  "params": {
      "accountNumber": "100001245"
  }
}

Localization is performed by the API layer.

---

# 31. Audit Trail

Every financial operation must be auditable.

Audit records include:

- Journal ID
- Reference ID
- Request ID
- Correlation ID
- Actor
- Service Name
- Timestamp
- IP Address (when applicable)
- Authentication Subject
- Previous State
- New State

Audit records are append-only.

---

# 32. Compliance

The Ledger must support regulatory requirements including:

- SOX
- PCI DSS
- ISO 27001
- AML
- KYC
- FATF Travel Rule
- Local financial regulations

Compliance requirements must never violate accounting invariants.

---

# 33. Event Publishing

After a successful database commit the Ledger publishes domain events.

Examples

LedgerJournalPosted

LedgerJournalReversed

LedgerAccountOpened

LedgerAccountClosed

BalanceSnapshotCreated

Events are never published before commit.

---

# 34. Outbox Pattern

The Ledger uses the Transactional Outbox Pattern.

Workflow

Database Transaction

↓

Journal Created

↓

Entries Created

↓

Outbox Event Written

↓

Commit

↓

Background Publisher

↓

Kafka / RabbitMQ / NATS

This guarantees reliable event delivery.

---

# 35. Monitoring

The following metrics must be exported.

ledger_posted_total

ledger_reversed_total

ledger_failed_total

ledger_balance_snapshots_total

ledger_db_transaction_seconds

ledger_deadlock_total

ledger_retry_total

ledger_posting_latency

Metrics should be compatible with Prometheus.

---

# 36. Logging

Logs are structured.

Recommended fields

timestamp

service

environment

requestId

correlationId

journalId

referenceId

accountId

assetId

severity

message

Sensitive values must never appear in logs.

---

# 37. Security

Ledger APIs require authenticated service identities.

Recommended mechanisms

mTLS

JWT

OIDC

Service Accounts

Every write operation must be authorized.

Read-only access may be restricted by role.

---

# 38. Secrets

The Ledger never stores:

Private Keys

Seed Phrases

Fireblocks API Secrets

RPC Credentials

Exchange Secrets

Secrets belong to dedicated secret-management systems.

Examples

HashiCorp Vault

AWS Secrets Manager

Azure Key Vault

Google Secret Manager

---

# 39. Disaster Recovery

Recovery objectives

RPO

Near Zero

RTO

Defined by deployment environment.

Database backups must be verified regularly.

Recovery drills must be performed periodically.

---

# 40. Backup Strategy

Backups include:

Database

Migration History

Configuration

Encryption Keys (managed separately)

Backups must be encrypted.

Backups must be immutable.

Backups must be tested.

---

# 41. High Availability

The Ledger must tolerate:

Node failure

Pod failure

Container restart

Availability zones failure

Replication must never compromise consistency.

Correctness has higher priority than availability.

---

# 42. Performance

The Ledger is optimized for:

High write throughput

Low read latency

Large journal history

Efficient reconciliation

Indexes must be reviewed periodically.

Large tables should use partitioning.

---

# 43. Partitioning

Ledger Entries are expected to become the largest table.

Recommended partition key

created_at

Alternative

journal_id hash partitioning

Partition strategy must preserve query efficiency.

---

# 44. Reconciliation

Periodic reconciliation verifies:

Account balances

Blockchain balances

Treasury balances

Exchange balances

Fireblocks balances

Reserve balances

Any mismatch generates an operational alert.

---

# 45. Observability

Every request receives:

Trace ID

Span ID

Correlation ID

Recommended standard

OpenTelemetry

Tracing spans include:

Validation

Posting

Database

Publishing

Notifications

---

# 46. Testing Strategy

Required test categories

Unit Tests

Integration Tests

Database Tests

Property-Based Tests

Concurrency Tests

Performance Tests

Chaos Tests

Recovery Tests

Every accounting invariant must be covered by automated tests.

---

# 47. Future Evolution

The architecture is designed to support future extensions without redesign.

Possible future capabilities include:

Multi-region ledgers

Sharded ledger partitions

Additional blockchain networks

CBDCs

Tokenized securities

Real-time settlement

Programmable payments

Smart contract accounting

Cross-chain bridge accounting

DAO treasury accounting

---

# 48. Decision

The CIBL platform adopts a PostgreSQL-based immutable double-entry ledger as the single financial source of truth.

Every financial movement must be represented as balanced journal entries.

Historical records are immutable.

Corrections occur only through compensating journals.

Financial correctness is prioritized above convenience, performance, and implementation simplicity.

This decision is considered permanent unless superseded by a future ADR.

---

## References

- PostgreSQL Documentation
- Martin Fowler — Accounting Patterns
- IFRS Accounting Principles
- PCI DSS
- ISO 27001
- OpenTelemetry Specification
- Fireblocks Documentation
- Bitcoin Developer Documentation
- Ethereum Yellow Paper
- NestJS Documentation
