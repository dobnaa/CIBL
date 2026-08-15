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