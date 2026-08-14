
# ADR 0001: Architecture and Design of Double-Entry Ledger Engine

- **Status:** Accepted
- **Date:** August 14, 2026
- **Authors:** CIBL Architecture Team
- **Stakeholders:** Core Backend, Financial Operations, Security & Compliance Teams
- **Decision Scope:** `services/ledger`, `packages/types`, `packages/validators`, `packages/i18n`, database infrastructure
- **Supersedes:** N/A
- **Related Systems:** Payment, Wallet, Settlement, Exchange, Blockchain, Treasury, Compliance

---

# 1. Context & Problem Statement

CIBL is an enterprise-grade digital asset transfer, payment, custody, settlement,
and network-liquidity platform.

The financial core of CIBL must maintain balances with bank-grade consistency,
complete auditability, deterministic reconstruction, and strong protection
against accidental or malicious financial state corruption.

Traditional balance-based implementations such as:

```sql
UPDATE accounts
SET balance = balance + 100;
are insufficient as the authoritative financial record because they can introduce:
race conditions;
lost updates;
non-auditable balance changes;
unexplained balance drift;
accidental asset creation;
accidental asset destruction;
inconsistent rollback behavior;
insufficient historical reconstruction;
weak idempotency guarantees;
difficulties during reconciliation;
difficulties proving financial correctness to auditors.
Therefore, CIBL requires a dedicated double-entry ledger engine.
The ledger is the authoritative accounting record for internal financial movements.
External blockchain state, wallet balances, exchange balances, payment-provider balances, and cached balances are not authoritative substitutes for the ledger.
2. Goals
The Ledger Engine MUST provide:
mathematically balanced double-entry accounting;
immutable financial history;
deterministic balance reconstruction;
strong idempotency;
concurrency-safe posting;
asset/currency isolation;
transactional atomicity;
complete auditability;
reconciliation support;
support for fiat and digital assets;
multi-network asset identification;
safe reversal/correction through compensating entries;
localized machine-readable error keys;
point-in-time balance queries;
operational observability;
support for high-volume transaction processing.
3. Non-Goals
The Ledger Engine is NOT responsible for:
blockchain node operation;
private-key custody;
transaction signing;
blockchain transaction broadcasting;
wallet generation;
KYC/AML decision making;
exchange-rate calculation;
payment-provider communication;
customer authentication;
UI balance rendering;
notification delivery.
Those responsibilities belong to other CIBL services.
For example:
services/blockchain
services/wallet
services/payment
services/compliance
services/exchange
services/notification
The Ledger Engine receives validated financial instructions from those systems and records the resulting accounting events.
4. Decision Drivers
4.1 Zero Financial Inconsistency
Every posted journal MUST be balanced.
For a single asset:
Σ(DEBIT) = Σ(CREDIT)
or equivalently:
Σ(DEBIT) - Σ(CREDIT) = 0
The invariant MUST be evaluated independently for every asset/currency.
A journal containing multiple assets MUST NOT balance BTC against USDBL merely because the numerical values happen to be equal.
4.2 Complete Audit Trail
Every financial state transition MUST be reconstructable from immutable journal and posting history.
The system MUST be able to answer:
What changed?
When did it change?
Which account changed?
Which asset changed?
Why did it change?
Which business operation caused it?
Which request created it?
Which service initiated it?
Which user/customer/entity was involved?
Which previous journal or transaction does it reference?
4.3 Immutability
Posted ledger journals and postings MUST NOT be updated or deleted.
Corrections MUST be performed using compensating/reversal journals.
Example:
Original:

Customer        +100 USDBL
Merchant        -100 USDBL

Correction:

Customer        -100 USDBL
Merchant        +100 USDBL
The original journal remains permanently available.
4.4 Idempotency
Every externally initiated financial operation MUST contain a unique idempotency/reference key.
A repeated request MUST NOT create duplicate financial postings.
Example:
reference_id = payment_01JXYZ...
The same reference MUST always resolve to the same financial operation.
Conflicting reuse of an existing reference ID MUST be rejected.
4.5 Concurrency Control
Concurrent posting against the same financial accounts MUST be safe.
The implementation MUST use PostgreSQL transactional guarantees and, where required:
SELECT ...
FOR UPDATE;
or an equivalent concurrency-control strategy.
Serializable isolation MAY be used for operations requiring the strongest transactional guarantees.
The exact isolation strategy MUST be selected per operation based on measured contention and throughput characteristics.
5. Accounting Model
5.1 Account Types
CIBL accounts use the following accounting classes:
ASSET
LIABILITY
EQUITY
REVENUE
EXPENSE
Examples:
ASSET
  - Hot Wallet BTC
  - Custody ETH
  - Bank USDBL Reserve

LIABILITY
  - Customer BTC Liability
  - Customer USDBL Liability

REVENUE
  - Transaction Fees
  - Withdrawal Fees

EXPENSE
  - Network Fees
  - Provider Fees
6. Fundamental Invariants
The following invariants are mandatory.
6.1 Journal Balance
For every journal_id and every asset_id:
SUM(DEBIT) = SUM(CREDIT)
A journal MUST NOT be posted if this invariant fails.
6.2 Positive Posting Amount
Every posting MUST satisfy:
amount > 0
Direction determines whether the posting is debit or credit.
Negative posting amounts MUST NOT be used.
6.3 Asset Isolation
Every posting belongs to exactly one asset.
Assets MUST NOT be implicitly converted inside the Ledger Engine.
For example:
BTC != ETH
BTC != USDBL
ETH != USDBL
Conversion belongs to the Exchange/Rate domain.
6.4 Immutable Posted State
Once a journal reaches:
POSTED
its financial postings cannot be modified or deleted.
6.5 Atomicity
A journal and all of its postings MUST be committed atomically.
Either:
ALL POSTINGS COMMIT
or:
NO POSTINGS COMMIT
Partial journals are forbidden.
6.6 Idempotency
For the same valid reference_id:
POST(reference_id) == POST(reference_id)
Repeated submission MUST return the previously created journal rather than creating a new one.
7. Multi-Asset Transactions
CIBL MUST support transactions involving multiple assets.
Example:
Customer sells:

1 BTC

and receives:

60,000 USDBL
This is NOT a single balanced numerical journal.
Instead, the accounting representation contains separate asset legs:
BTC leg:

Customer BTC        CREDIT  1 BTC
Treasury BTC        DEBIT   1 BTC


USDBL leg:

Treasury USDBL      CREDIT  60,000 USDBL
Customer USDBL      DEBIT   60,000 USDBL
Each asset remains independently balanced.
The economic relationship between the legs is represented by transaction metadata, business references, and/or an exchange/order identifier.
8. Database Schema
PostgreSQL is the authoritative persistence layer.
8.1 Assets
An explicit asset registry is required.
CREATE TABLE ledger_assets (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    code VARCHAR(32) NOT NULL,
    name VARCHAR(128) NOT NULL,

    asset_type VARCHAR(32) NOT NULL
        CHECK (
            asset_type IN (
                'FIAT',
                'NATIVE',
                'TOKEN'
            )
        ),

    network VARCHAR(32),

    contract_address VARCHAR(128),

    decimals SMALLINT NOT NULL
        CHECK (decimals >= 0 AND decimals <= 36),

    status VARCHAR(16) NOT NULL DEFAULT 'ACTIVE'
        CHECK (
            status IN (
                'ACTIVE',
                'SUSPENDED',
                'DEPRECATED'
            )
        ),

    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),

    UNIQUE (code, network, contract_address)
);
Examples:
USD
BTC / BITCOIN
ETH / ETHEREUM
TRX / TRON
USDBL / Ethereum
USDBL / Solana
The exact asset identity MUST NOT rely solely on the ticker symbol.
9. Ledger Accounts
CREATE TABLE ledger_accounts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    account_number VARCHAR(64) NOT NULL UNIQUE,

    holder_id UUID,

    account_type VARCHAR(32) NOT NULL
        CHECK (
            account_type IN (
                'ASSET',
                'LIABILITY',
                'EQUITY',
                'REVENUE',
                'EXPENSE'
            )
        ),

    asset_id UUID NOT NULL
        REFERENCES ledger_assets(id),

    status VARCHAR(16) NOT NULL DEFAULT 'ACTIVE'
        CHECK (
            status IN (
                'ACTIVE',
                'SUSPENDED',
                'CLOSED'
            )
        ),

    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),

    closed_at TIMESTAMPTZ
);
A ledger account belongs to exactly one asset.
This prevents accidental cross-asset balance aggregation.
10. Ledger Journals
CREATE TABLE ledger_journals (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    reference_id VARCHAR(128) NOT NULL UNIQUE,

    transaction_type VARCHAR(64) NOT NULL,

    description TEXT,

    status VARCHAR(16) NOT NULL DEFAULT 'POSTED'
        CHECK (
            status IN (
                'PENDING',
                'POSTED',
                'REVERSED',
                'FAILED'
            )
        ),

    reversal_of UUID
        REFERENCES ledger_journals(id),

    metadata JSONB NOT NULL DEFAULT '{}'::jsonb,

    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),

    posted_at TIMESTAMPTZ,

    reversed_at TIMESTAMPTZ
);
reference_id is the idempotency key for the originating financial operation.
11. Ledger Entries
CREATE TABLE ledger_entries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    journal_id UUID NOT NULL
        REFERENCES ledger_journals(id),

    account_id UUID NOT NULL
        REFERENCES ledger_accounts(id),

    asset_id UUID NOT NULL
        REFERENCES ledger_assets(id),

    direction VARCHAR(6) NOT NULL
        CHECK (
            direction IN (
                'DEBIT',
                'CREDIT'
            )
        ),

    amount NUMERIC(36, 18) NOT NULL
        CHECK (amount > 0),

    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),

    metadata JSONB NOT NULL DEFAULT '{}'::jsonb
);
asset_id is intentionally stored directly on the entry in addition to the account relationship.
The application/database layer MUST verify that:
ledger_entries.asset_id == ledger_accounts.asset_id
This redundant value acts as a defensive financial invariant and simplifies indexing and reconciliation.
12. Required Indexes
CREATE INDEX idx_ledger_entries_account_created
    ON ledger_entries(account_id, created_at);

CREATE INDEX idx_ledger_entries_journal
    ON ledger_entries(journal_id);

CREATE INDEX idx_ledger_entries_asset_created
    ON ledger_entries(asset_id, created_at);

CREATE INDEX idx_ledger_journals_created
    ON ledger_journals(created_at);

CREATE INDEX idx_ledger_journals_transaction_type
    ON ledger_journals(transaction_type);
13. Immutability Enforcement
Application-level restrictions alone are insufficient.
The database MUST enforce immutability for posted ledger records.
The implementation SHOULD use PostgreSQL triggers or equivalent database-level protection to prevent:
UPDATE ledger_entries;
DELETE FROM ledger_entries;
and prevent modification/deletion of:
POSTED journals
after posting.
The only supported correction mechanism is a compensating journal.
14. Posting Algorithm
A ledger posting operation follows this logical sequence:
1. Receive posting request
        ↓
2. Validate request
        ↓
3. Validate idempotency reference
        ↓
4. Begin database transaction
        ↓
5. Lock affected accounts if required
        ↓
6. Validate account status
        ↓
7. Validate asset identity
        ↓
8. Validate amounts
        ↓
9. Construct journal
        ↓
10. Construct postings
        ↓
11. Validate double-entry invariant
        ↓
12. Persist journal
        ↓
13. Persist all postings
        ↓
14. Commit transaction
        ↓
15. Emit post-commit event
No external network call MUST occur inside the database transaction unless there is an explicitly documented reason.
15. Idempotent Posting
The posting service MUST first resolve the reference_id.
Pseudo-code:
const existing = await journalRepository.findByReferenceId(referenceId);

if (existing) {
    return existing;
}

return database.transaction(async (tx) => {
    // Re-check inside transaction to prevent race conditions.

    const existingInsideTransaction =
        await tx.journal.findByReferenceId(referenceId);

    if (existingInsideTransaction) {
        return existingInsideTransaction;
    }

    // Create journal and entries.
});
The unique database constraint remains the final protection against races.
16. Balance Calculation
The authoritative balance is derived from ledger postings.
For an account:
Balance =
    SUM(DEBIT)
    -
    SUM(CREDIT)
The exact sign presentation depends on account type.
For operational purposes, the system MAY maintain materialized/cached balances, but such balances are derived data and MUST NOT replace the immutable ledger.
17. Balance Snapshots
For high-volume accounts, CIBL MAY maintain periodic balance snapshots.
Example:
ledger_balance_snapshots
A snapshot MUST contain:
account_id
asset_id
snapshot_balance
snapshot_at
last_entry_id
The snapshot MUST always be reconstructable and verifiable against the underlying ledger.
18. Reversals and Corrections
Ledger entries cannot be edited.
Instead:
Original Journal
        ↓
Reversal Journal
Example:
J1:
Customer     DEBIT   100 USDBL
Merchant     CREDIT  100 USDBL
Reversal:
J2:
Customer     CREDIT  100 USDBL
Merchant     DEBIT   100 USDBL
The original journal remains immutable.
19. Transaction Types
The Ledger Engine SHOULD support standardized transaction types.
Examples:
DEPOSIT
WITHDRAWAL
TRANSFER
PAYMENT
REFUND
FEE
EXCHANGE
SETTLEMENT
ADJUSTMENT
REVERSAL
CHARGEBACK
TREASURY_TRANSFER
CUSTODY_TRANSFER
Services MUST NOT invent arbitrary transaction semantics without registering the new transaction type.
20. Fees
Fees MUST be represented as explicit ledger postings.
Example:
Customer receives      DEBIT   99 USDBL
Fee Revenue            CREDIT   1 USDBL
Treasury/Source        CREDIT 100 USDBL
The exact account structure depends on the business operation.
Fees MUST NOT be silently subtracted from a balance without a corresponding ledger entry.
21. Blockchain Deposits
A blockchain deposit MUST NOT directly mutate a customer's balance.
Instead:
Blockchain
    ↓
services/blockchain
    ↓
Deposit Detection
    ↓
Confirmation Policy
    ↓
Ledger Posting
    ↓
Customer Balance
The blockchain transaction ID MUST be stored as an external reference.
Example metadata:
{
  "network": "BITCOIN",
  "txHash": "external-reference",
  "blockHeight": 123456,
  "confirmations": 6
}
The exact blockchain identifier is an external reference and does not replace the CIBL ledger journal ID.
22. Blockchain Withdrawals
Withdrawal flow:
Customer Request
        ↓
Risk / Compliance
        ↓
Ledger Reservation / Posting
        ↓
Blockchain Service
        ↓
Transaction Signing
        ↓
Broadcast
        ↓
Confirmation
        ↓
Settlement / Finalization
Private keys MUST NOT be stored in the Ledger Engine.
Fireblocks/HSM/custody systems remain responsible for key custody and signing.
23. Fireblocks Integration
The Ledger Engine MUST remain independent of Fireblocks.
The architecture is:
services/ledger
       │
       │ financial accounting
       ↓
services/wallet / services/blockchain
       │
       ↓
Fireblocks
The ledger records the financial state.
Fireblocks records custody and signing operations.
Neither system should be treated as a direct replacement for the other.
24. Reconciliation
CIBL MUST implement reconciliation processes between:
Internal Ledger
        ↕
Blockchain State

Internal Ledger
        ↕
Custody Provider

Internal Ledger
        ↕
Bank / Fiat Provider

Internal Ledger
        ↕
Exchange / Liquidity Provider
Reconciliation discrepancies MUST generate explicit operational records.
A reconciliation process MUST NOT silently modify ledger history.
Corrections require authorized compensating journals.
25. Authorization and Separation of Duties
Posting operations MUST be authorized according to transaction risk.
High-risk operations MAY require:
Maker
   ↓
Validation
   ↓
Approver
   ↓
Ledger Posting
Administrative users MUST NOT receive unrestricted direct database access.
Direct modification of ledger tables in production is prohibited.
26. Localization
The Ledger Engine MUST NOT hard-code human-readable error messages.
Errors MUST expose stable translation keys.
Examples:
ledger.errors.unbalanced_entry
ledger.errors.account_not_found
ledger.errors.account_closed
ledger.errors.asset_mismatch
ledger.errors.invalid_amount
ledger.errors.duplicate_reference
ledger.errors.journal_immutable
ledger.errors.invalid_reversal
ledger.errors.insufficient_balance
ledger.errors.concurrent_posting
The localization layer is implemented through:
packages/i18n
The backend returns structured errors such as:
{
  "code": "LEDGER_UNBALANCED_ENTRY",
  "translationKey": "ledger.errors.unbalanced_entry"
}
The presentation layer is responsible for translating the message.
27. Precision and Monetary Representation
Floating-point arithmetic MUST NOT be used for financial amounts.
Forbidden:
number
for authoritative monetary calculations.
Amounts MUST use:
PostgreSQL NUMERIC
and application-level decimal arithmetic.
Asset precision MUST be determined from the asset registry.
Examples:
BTC      8 decimals
ETH     18 decimals
USDBL   asset-defined precision
The system MUST reject values exceeding the supported asset precision.
28. Account Hierarchy
CIBL SHOULD support hierarchical account structures.
Example:
Assets
├── Custody
│   ├── BTC
│   ├── ETH
│   └── USDBL
│
Liabilities
├── Customer Balances
│   ├── BTC
│   ├── ETH
│   └── USDBL
│
Revenue
├── Trading Fees
├── Payment Fees
└── Withdrawal Fees
Parent accounts are organizational/accounting structures and MUST NOT be confused with transactional accounts.
29. Ledger Service Boundary
The primary implementation is:
services/ledger/
Expected structure:
services/ledger/
├── src/
│   ├── accounts/
│   ├── journals/
│   ├── postings/
│   ├── balances/
│   ├── reversals/
│   ├── reconciliation/
│   ├── idempotency/
│   ├── audit/
│   ├── ledger.module.ts
│   └── main.ts
│
├── test/
├── nest-cli.json
├── package.json
└── tsconfig.json
The service MUST expose a domain-level API rather than allowing other services to manipulate ledger tables directly.
30. Package Boundaries
Shared types and validation logic SHOULD be located in:
packages/types
packages/validators
packages/i18n
packages/errors
packages/utils
The ledger-specific implementation remains inside:
services/ledger
Other services communicate with the ledger through service APIs or approved internal interfaces.
31. API Contract
A conceptual posting request:
interface CreateJournalRequest {
    referenceId: string;

    transactionType: string;

    description?: string;

    postings: Array<{
        accountId: string;
        assetId: string;
        direction: "DEBIT" | "CREDIT";
        amount: string;
    }>;

    metadata?: Record<string, unknown>;
}
Amounts are represented as strings at API boundaries to prevent JavaScript floating-point corruption.
32. Validation Rules
Before posting, the service MUST validate:
referenceId exists
referenceId is unique
journal has >= 2 postings
every posting has a valid account
every posting has a valid asset
account asset == posting asset
amount > 0
amount precision <= asset precision
account is active
journal is balanced per asset
transaction type is valid
metadata is valid
33. Database Transaction Boundary
The following operations MUST execute within one PostgreSQL transaction:
Create Journal
+
Create Entries
+
Required Account Locks
+
Required Balance Reservation
External calls such as:
Fireblocks
Blockchain RPC
Email
Webhook
Kafka
HTTP APIs
MUST NOT be considered part of the PostgreSQL transaction.
34. Events and Outbox Pattern
Post-commit integrations SHOULD use the transactional outbox pattern.
Example:
Database Transaction
    ├── ledger_journal
    ├── ledger_entries
    └── outbox_event
              ↓
         Commit
              ↓
       Event Publisher
              ↓
      Other CIBL Services
This prevents the following failure:
Ledger committed
        ↓
Event publishing failed
without leaving the system with a recoverable event.
35. Audit Logging
Ledger activity MUST produce audit metadata sufficient to identify:
actor
service
request ID
correlation ID
reference ID
journal ID
timestamp
source system
operation type
Audit records MUST NOT expose private keys, secrets, passwords, authentication tokens, or sensitive credentials.
36. Security Requirements
The Ledger Engine MUST enforce:
least-privilege database credentials;
encrypted database connections;
encrypted backups;
restricted production DB access;
database audit logging;
application authorization;
service-to-service authentication;
secret management through approved secret infrastructure;
no secrets in source control;
no private keys in ledger databases;
no direct production table mutation by operators.
37. Data Retention
Ledger history is financial evidence and MUST NOT be treated as disposable application data.
Retention requirements MUST comply with:
applicable financial regulations;
jurisdictional requirements;
CIBL compliance policies;
contractual obligations.
Archival strategies MUST preserve immutability and reconstruction capability.
38. Partitioning
At high transaction volumes, ledger_entries MAY use time-based partitioning.
Potential strategy:
ledger_entries
├── 2026_01
├── 2026_02
├── 2026_03
└── ...
Partitioning MUST NOT change accounting semantics.
The logical ledger remains one immutable dataset.
Partitioning should only be introduced after measurable workload requirements justify the additional operational complexity.
39. Performance Requirements
The system SHOULD optimize for:
high write throughput;
predictable transaction latency;
low lock contention;
efficient account-history queries;
efficient journal lookup;
efficient reconciliation;
efficient balance reconstruction.
Performance optimizations MUST NOT weaken financial invariants.
Correctness always takes precedence over raw throughput.
40. Failure Handling
If posting fails before commit:
ROLLBACK
No financial state is created.
If posting succeeds:
COMMIT
the journal becomes authoritative.
If a downstream system fails after commit:
Ledger remains committed.
Downstream operation is retried/reconciled.
The ledger MUST NOT be rolled back merely because an external system failed.
41. Testing Strategy
The Ledger Engine MUST include:
Unit Tests
balancing validation;
asset validation;
precision validation;
account rules;
reversal rules;
idempotency.
Integration Tests
PostgreSQL transactions;
locking;
concurrent posting;
rollback;
unique reference enforcement;
database immutability.
Property-Based Tests
The system SHOULD generate random balanced posting sets and verify:
Σ Debit(asset) == Σ Credit(asset)
for every asset.
Concurrency Tests
Multiple simultaneous requests against the same account MUST be tested.
Reconciliation Tests
Ledger state MUST be compared against simulated external blockchain/custody states.
42. Example: Simple Transfer
Customer A sends 100 USDBL to Customer B.
Journal:
TRANSFER

Customer A     CREDIT   100 USDBL
Customer B     DEBIT    100 USDBL
Invariant:
DEBIT  = 100
CREDIT = 100
Balanced.
43. Example: Payment With Fee
Customer pays 100 USDBL with a 1 USDBL fee.
Customer        CREDIT   101 USDBL
Merchant        DEBIT    100 USDBL
Fee Revenue     DEBIT      1 USDBL
Balanced:
DEBIT  = 101
CREDIT = 101
44. Example: Blockchain Deposit
Customer deposits 0.5 BTC.
CIBL BTC Custody       DEBIT    0.5 BTC
Customer BTC Liability CREDIT   0.5 BTC
The blockchain transaction hash is stored as external metadata.
45. Example: Blockchain Withdrawal
Customer withdraws 0.2 BTC and pays a 0.001 BTC fee.
The exact accounting depends on the custody and fee model, but all economic effects MUST be explicitly represented.
Example:
Customer BTC Liability     DEBIT    0.201 BTC
External/Custody BTC       CREDIT   0.200 BTC
Network Fee Expense        CREDIT   0.001 BTC
The final chart of accounts MUST be approved by Financial Operations.
46. Example: Reversal
Original:
Customer A     CREDIT 100 USDBL
Customer B     DEBIT  100 USDBL
Reversal:
Customer A     DEBIT  100 USDBL
Customer B     CREDIT 100 USDBL
No original posting is modified.
47. Operational Monitoring
The Ledger Engine MUST expose metrics for:
ledger_postings_total
ledger_posting_failures_total
ledger_unbalanced_attempts_total
ledger_duplicate_reference_total
ledger_reversal_total
ledger_reconciliation_discrepancies_total
ledger_posting_latency
ledger_db_lock_wait
ledger_outbox_pending
Critical invariant violations MUST generate alerts.
48. Reconciliation Alerts
The following conditions MUST be considered critical:
Ledger imbalance
Negative balance where prohibited
Duplicate external transaction
Missing blockchain settlement
Unexpected custody balance
Unprocessed outbox events
Broken journal reference
Asset mismatch
Such conditions MUST enter an operational investigation workflow.
49. Access Model
Only authorized services may post to the ledger.
Conceptually:
Payment Service
      │
Wallet Service
      │
Exchange Service
      │
Settlement Service
      │
Blockchain Service
      │
      ▼
 Ledger API
      │
      ▼
 PostgreSQL
Direct database writes from those services are prohibited.
50. Architectural Principle
The Ledger is the financial source of truth.
The following are derived or external systems:
UI balance
Redis balance cache
Blockchain balance
Wallet provider balance
Exchange balance
Fireblocks balance
Analytics database
They may be compared against the ledger, but they do not supersede it.
51. Consequences
Positive Consequences
strong financial correctness;
complete audit history;
deterministic reconstruction;
safe concurrency;
reliable reconciliation;
clear asset isolation;
strong idempotency;
support for multiple blockchain networks;
compatibility with custody providers;
controlled financial corrections;
scalable PostgreSQL architecture.
Negative Consequences
greater implementation complexity;
more database writes;
more complicated testing;
additional reconciliation infrastructure;
stricter development discipline;
higher operational responsibility;
database-level immutability requires careful migration management.
These costs are accepted because financial correctness is more important than implementation simplicity.
52. Rejected Alternatives
Option 1: Event Sourcing + Graph Database
Rejected as the primary financial ledger because it introduces unnecessary complexity for the core accounting invariant.
Event sourcing concepts MAY still be used for integration events and audit streams.
Option 2: PostgreSQL Double-Entry Ledger
Accepted.
PostgreSQL provides:
ACID transactions;
row-level locking;
strong constraints;
mature replication;
mature backup/restore;
partitioning;
indexing;
operational maturity.
Option 3: Managed Ledger Database
Services such as AWS QLDB were considered but rejected because CIBL requires:
infrastructure control;
deployment flexibility;
sovereign/multi-region options;
predictable operational control;
direct PostgreSQL ecosystem integration.
Option 4: Simple Balance Table
Rejected.
A balance table may exist as a cache/materialized projection, but it MUST NOT be the authoritative financial record.
53. Future Extensions
The following may be introduced without changing the fundamental architecture:
Multi-region ledger replicas
Ledger sharding
Advanced balance snapshots
Zero-knowledge audit proofs
Merkleized audit chains
Hardware-backed signing workflows
Advanced reconciliation engine
Regulatory reporting
Real-time accounting streams
Double-entry accounting analytics
Any extension MUST preserve the core ledger invariants.
54. Final Decision
CIBL will implement a custom PostgreSQL-backed double-entry Ledger Engine within:
services/ledger
The Ledger Engine is the authoritative financial source of truth.
The system MUST enforce:
Double Entry
+
Per-Asset Balance
+
Immutability
+
Idempotency
+
Atomicity
+
Concurrency Safety
+
Auditability
+
Asset Isolation
All financial services must integrate with the Ledger through explicit service/domain interfaces and MUST NOT directly modify ledger persistence.
All financial corrections must be represented through compensating journals.
The architecture defined in this ADR is considered Accepted and forms the baseline financial-accounting architecture for CIBL.
55. Architectural Invariants — Quick Reference
The following rules are non-negotiable:
1. Every posted journal must balance.
2. Balance must be checked independently per asset.
3. Every posting amount must be positive.
4. Negative amounts are forbidden.
5. Posted entries cannot be updated.
6. Posted entries cannot be deleted.
7. Corrections require compensating journals.
8. Every external financial operation requires idempotency.
9. Asset identity must be explicit.
10. Floating-point numbers cannot represent authoritative money.
11. Financial state changes must be atomic.
12. External network calls cannot silently mutate ledger state.
13. Direct database writes by business services are forbidden.
14. Ledger history must be reconstructable.
15. Reconciliation must never silently modify history.
16. Secrets and private keys must never be stored in ledger records.
17. Localization uses stable translation keys.
18. Correctness takes precedence over throughput.
56. Status
ACCEPTED
This ADR is the authoritative architectural decision for the CIBL Ledger Engine.
Changes to the fundamental accounting model, immutability guarantees, asset-balance invariants, or source-of-truth definition require a new ADR or an explicit superseding ADR.

