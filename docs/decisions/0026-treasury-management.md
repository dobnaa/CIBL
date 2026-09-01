# ADR 0025: Treasury Management

## 1. Metadata

| Field | Value |
|---|---|
| Status | **Accepted** |
| Version | 1.0 |
| Date | 2026-08-19 |
| Authors | CIBL Architecture Team |
| Decision Scope | `services/account` (TREASURY classification), policy layer over `services/liquidity` |
| Supersedes | N/A |
| Superseded By | N/A |
| Related | ADR 0000 (Principle 3), ADR 0011, ADR 0012, ADR 0021 |

---

## 2. Executive Summary

Treasury Management governs CIBL's **own** operational capital — reserve
requirements, capital allocation across asset classes and venues, and
counterparty risk limits for where CIBL holds its own funds (banks,
custodians, exchanges) — as distinct from customer funds. Its most
important architectural rule is **segregation**: `TREASURY`-classified
accounts (ADR 0021 §12) must never commingle with customer-owned
accounts at the Ledger level, and Treasury sets the policy targets that
Liquidity Engine (ADR 0012) mechanically enforces, rather than
duplicating Liquidity Engine's threshold-monitoring machinery.

---

## 3. Context

ADR 0012 (Liquidity Engine) already handles threshold-based rebalancing
across custody tiers, fiat, and exchange venues — but it takes
`minimumBalance`/`targetBalance` as given configuration (ADR 0012 §29).
Treasury Management is the missing layer that decides what those
targets should be, based on CIBL's own capital position, reserve
requirements, and counterparty risk appetite — a strategic/financial
function, not an operational monitoring one.

## 4. Problem Statement

Define how CIBL's own capital is represented in the Ledger (segregated
from customer funds), who sets Liquidity Engine's targets and why, and
what counterparty concentration limits govern where CIBL's operational
capital may be held.

---

## 5. Business Drivers

- Regulatory capital/reserve requirements (jurisdiction-dependent) apply
  specifically to CIBL's own funds, not customer funds — this
  distinction must be structurally unambiguous, not just a bookkeeping
  convention.
- Counterparty concentration risk (e.g. too much of CIBL's own capital
  at a single bank or custodian) is a real operational risk requiring
  explicit limits and monitoring.

## 6. Functional Requirements

- FR1: `TREASURY`-classified `BankAccount`s (ADR 0021 §12) are
  structurally segregated from `CHECKING`/`SAVINGS`/`MERCHANT_
  SETTLEMENT` accounts at the Ledger level — a Treasury account's
  composed Wallets reference `ledger_accounts` rows whose `account_type`
  (ADR 0001 §12.1) is `ASSET` or `EQUITY`, never `LIABILITY` (customer
  liability accounts remain exclusively customer-owned).
- FR2: Treasury sets and periodically reviews `LiquidityThreshold`
  values (ADR 0012 §14) for CIBL-owned venues, rather than each venue's
  threshold being set ad hoc by whichever team happens to operate it.
- FR3: Enforce counterparty concentration limits (e.g. no more than a
  configured percentage of CIBL's own capital at a single custodian or
  bank) as a distinct policy check, separate from Liquidity Engine's
  per-venue threshold monitoring.

## 7. Non-Functional Requirements

- NFR1: A Treasury account can never be the source or destination of a
  customer-initiated transfer (ADR 0002's `transfer` API) — only
  internal, Treasury-authorized movements (e.g. capital injection,
  operational expense funding) may post against it.
- NFR2: Counterparty concentration checks (FR3) run on a scheduled basis
  (not necessarily real-time, since capital allocation changes far less
  frequently than customer transaction volume) but must alert
  immediately if a limit is breached, not wait for the next scheduled
  cycle to report it.

---

## 8. Architecture Principles

Applies ADR 0000 Principle 3 (Treasury is part of the platform's
financial core) and Principle 4 (Ledger is the Source of Truth — FR1's
segregation is enforced through the existing `account_type`/
`asset_class` model, ADR 0001 §12.1 and ADR 0011 §9.1, not a new parallel
mechanism).

---

## 9. Decision

Treasury Management is implemented as a policy layer, not a new
balance-holding service: `TREASURY`-classified `BankAccount`s (already
defined in ADR 0021) compose Wallets backed by `ASSET`/`EQUITY`-type
`ledger_accounts` (FR1), and a new, lightweight Treasury policy function
(within `services/account` or a thin dedicated module — an
implementation detail, not requiring a new Core Ownership service) sets
`LiquidityThreshold` rows (ADR 0012 §14) and runs counterparty
concentration checks (FR3) on a schedule.

---

## 10. Scope

Treasury account segregation, the Treasury-sets-targets/Liquidity-
enforces-targets relationship with ADR 0012, and counterparty
concentration limits.

## 11. Out of Scope

- Liquidity Engine's own threshold-monitoring and rebalance-request
  mechanics — ADR 0012, unchanged by this ADR.
- Specific reserve-requirement percentages for any given jurisdiction —
  an operational/legal determination, configured (§29), not architected
  here.
- Internal Transfer Engine's mechanics for moving funds between Treasury
  and operational accounts — ADR 0026.

---

## 12. Domain Model

```
TreasuryPolicy
 - id, tenantId
 - assetId, venue                    (same shape as ADR 0012's LiquidityThreshold)
 - targetAllocationPercentage        of total CIBL-owned capital in this asset/venue
 - counterpartyConcentrationLimit    FR3, max percent of total capital at one counterparty

CounterpartyExposure (derived/projection, mirrors ADR 0012 section 12's
                        LiquidityPosition pattern - not a new balance store)
 - counterpartyId (a bank, custodian, or exchange)
 - totalExposure                     aggregated from TREASURY account balances
                                          held via that counterparty
 - concentrationPercentage
```

---

## 13. Component Model

```
Treasury Policy (periodic review, human-in-the-loop for target-setting)
        |
        v  writes
services/liquidity: liquidity_thresholds (ADR 0012 section 14)
        |
        v  Liquidity Engine's own existing monitoring loop (ADR 0012 section 19)
   RebalanceRequest -> services/custody / services/fiat (unchanged)

Separately:
Treasury Policy -> CounterpartyExposure calculation (scheduled, NFR2)
        |
        v  breach detected
   treasury.concentration.limit_breached event (immediate alert, NFR2)
```

Treasury does not duplicate Liquidity Engine's rebalancing execution
path (§9) — it only supplies the targets Liquidity Engine already knows
how to act on.

---

## 14. Data Model

```sql
CREATE TABLE treasury_policies (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_id UUID NOT NULL,
    asset_id UUID NOT NULL,
    venue VARCHAR(24) NOT NULL,
    target_allocation_percentage NUMERIC(5,2) NOT NULL,
    counterparty_concentration_limit NUMERIC(5,2) NOT NULL,
    reviewed_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    reviewed_by UUID NOT NULL
);
```

No `balance` column — `CounterpartyExposure` (§12) is computed on
demand from existing `ledger_accounts`/`liquidity_positions` data
(ADR 0001, ADR 0012), consistent with every prior ADR's rejection of
parallel balance storage.

---

## 15. Lifecycle

Not entity-lifecycle-bound in the traditional sense — `treasury_
policies` rows are periodically reviewed and updated (§16), not created
or closed like an account.

## 16. State Machine

```
Policy review cycle: [CURRENT] --scheduled or triggered review--> [UNDER_REVIEW]
                                --approved by treasury officer--> [CURRENT] (new values)
```

---

## 17. API Design

```ts
setTreasuryPolicy(assetId, venue, targetAllocationPercentage, concentrationLimit, reviewedBy): Promise<TreasuryPolicy>
getCounterpartyExposure(): Promise<CounterpartyExposure[]>
```

`setTreasuryPolicy` internally calls `services/liquidity`'s existing
threshold-configuration path (ADR 0012 §29) rather than exposing a
separate mechanism Liquidity Engine doesn't know about.

---

## 18. Event Model

`treasury.policy.updated`, `treasury.concentration.limit_breached`
(NFR2's immediate-alert event). Consumes `liquidity.threshold.breached`
(ADR 0012 §18) as one input to whether a policy target itself needs
reconsideration (a persistent breach may mean the target, not just the
current balance, is wrong).

---

## 19. Sequence Diagrams

### 19.1 Treasury Sets a New Target, Liquidity Engine Enforces It

```
Treasury Officer -> setTreasuryPolicy(BTC, CUSTODY_HOT, target=2%, concentrationLimit=30%)
   -> treasury_policies row updated
   -> internally calls services/liquidity's threshold config (ADR 0012 section 14)
      to set liquidity_thresholds accordingly
[services/liquidity's existing monitoring loop, unmodified, now enforces
 the new target exactly as it always has for any other threshold]
```

### 19.2 Counterparty Concentration Breach

```
[Scheduled job, NFR2]
Treasury: computes CounterpartyExposure for each counterparty
   -> Custodian X holds 35% of CIBL's total BTC capital
   -> exceeds counterparty_concentration_limit (30%)
   -> treasury.concentration.limit_breached event (immediate, not waiting
      for next scheduled cycle to report)
   -> services/notification / services/risk alerted
```

---

## 20. Deployment Model

Implemented as a module within `services/account` (or a comparably
lightweight service) rather than a new Core Ownership system — Treasury
Management is a policy-setting function over existing services
(Liquidity, Ledger), not a new balance-holding or transaction-processing
system in its own right.

## 21. Scaling Strategy

Not a high-throughput concern — treasury policy review happens on a
human-timescale cadence (periodic review, §16), not per-transaction.

## 22. High Availability

Treasury policy-setting is not on any customer-facing critical path;
its availability requirement is low relative to Ledger/Wallet/Custody
(mirroring ADR 0012 §22's reasoning for Liquidity Engine generally).

## 23. Disaster Recovery

`treasury_policies` is DR Tier 2 (ADR 0018 §12) — reconstructable from
`services/liquidity`'s own threshold configuration (which Treasury
writes to) plus a manual re-review if the policy-setting history itself
is lost, since the underlying Ledger/Liquidity state (Tier 1/3
respectively) is unaffected.

---

## 24. Security

`setTreasuryPolicy` requires an authenticated, authorized Treasury
officer role (ADR 0008 §17's authorization model) — this is a
capital-allocation decision with real financial consequence, subject to
the same rigor as any other high-consequence configuration change.

## 25. Compliance

Reserve-requirement compliance (a jurisdiction-specific regulatory
obligation) is verified against `CounterpartyExposure` and Treasury
account balances (FR1's segregated `ASSET`/`EQUITY` accounts) — this
segregation is precisely what makes "does CIBL hold sufficient reserves,
separately from customer liabilities" a directly answerable, auditable
question rather than requiring manual reconciliation.

## 26. Audit

`treasury_policies.reviewed_by` (§14) ensures every target-setting
decision is attributable to a specific accountable individual, not an
anonymous system default.

## 27. Observability

Metrics: `treasury_policy_updated_total`, `treasury_concentration_
breach_total` (should be rare; any occurrence is investigated),
`treasury_counterparty_exposure_percentage` (per counterparty, tracked
over time to spot concentration trending toward a limit before it is
actually breached).

## 28. Performance

Not a performance-sensitive system — periodic, human-paced review
cycles (§21).

## 29. Configuration

Reserve-requirement percentages and counterparty concentration limits
(§14) are themselves the configuration this ADR's policy layer manages
— they are set by treasury/compliance officers per jurisdiction, not
hard-coded.

## 30. Failure Scenarios

If the scheduled counterparty-exposure calculation (§19.2) fails to
run, this is itself alertable (a missed-schedule alert, distinct from a
limit-breach alert) — silence from a scheduled compliance-adjacent job
is itself a signal worth monitoring, not just its outputs.

## 31. Recovery Procedures

See §23 — reconstruction from Liquidity Engine's existing configuration
plus manual treasury review is the recovery path; no specialized
procedure beyond that.

---

## 32. Testing Strategy

A specific test confirming FR1's segregation: no `TREASURY`-classified
account's composed Wallet can ever reference a `LIABILITY`-type
`ledger_account` (a schema/application-level invariant check); tests for
counterparty concentration calculation correctness given known exposure
inputs; a test confirming NFR1 — a customer-initiated transfer request
targeting a Treasury account's Wallet is rejected.

## 33. Migration Strategy

New venues or counterparties are additive `treasury_policies` rows; no
schema change required to add a new counterparty to monitor.

## 34. Backward Compatibility

`setTreasuryPolicy`/`getCounterpartyExposure` (§17) are the stable
contract for any future treasury-adjacent tooling (e.g. a treasury
dashboard) to build against.

## 35. Alternatives Considered

**Alternative: A dedicated `services/treasury` Core Ownership system,
separate from `services/account` and `services/liquidity`.** Rejected
for v1 — Treasury Management's actual function (set policy targets,
monitor concentration) is thin enough to not warrant a new Core
Ownership system per ADR 0000 Principle 3; if Treasury's scope grows
substantially (e.g. active treasury trading/hedging strategies), a
dedicated service and ADR can be introduced later without disrupting
this ADR's segregation model (FR1).

**Alternative: Commingle Treasury and customer funds in shared venues
for operational simplicity (e.g. one hot wallet for both).** Rejected
outright — this is a fundamental regulatory and risk-management
violation in virtually every jurisdiction CIBL targets; FR1's structural
segregation at the Ledger `account_type` level is non-negotiable, not a
convenience trade-off.

## 36. Trade-offs

Implementing Treasury as a thin policy layer over existing services
(§20) rather than a full dedicated service means less architectural
ceremony now, at the cost of potentially needing a larger refactor if
Treasury's scope grows significantly later; accepted as the
appropriately-scoped decision for the platform's current stage (mirroring
ADR 0000 Principle 10's modular-monolith-to-distributed-services
philosophy).

## 37. Risks

- **Risk:** Treasury and customer funds become commingled through a
  configuration error (e.g. a Treasury account's Wallet accidentally
  backed by a `LIABILITY`-type account). **Mitigation:** §32's schema/
  application-level invariant test makes this a caught bug, not a
  silent violation.
- **Risk:** Counterparty concentration monitoring (§19.2) lags real
  exposure changes since it runs on a schedule, not in real time.
  **Mitigation:** NFR2's explicit acceptance of scheduled (not
  real-time) monitoring, justified by capital allocation's inherently
  slower pace of change than transaction volume — if this assumption
  proves wrong operationally, the schedule frequency is configuration
  (§29), not an architectural constraint.

## 38. Consequences

**Positive:** clean, auditable segregation between CIBL's own capital
and customer funds; Treasury targets and Liquidity Engine's mechanical
enforcement remain cleanly separated concerns, each independently
testable and reasoned about. **Negative:** Treasury's current thin-layer
implementation (§20) may need to grow into a dedicated service if its
scope expands significantly (e.g. active capital markets activity),
which this ADR anticipates but does not preempt.

## 39. Future Evolution

If Treasury's scope grows to include active strategies (e.g. yield
generation on reserve assets, hedging), a dedicated `services/treasury`
and its own ADR would formalize that, building on — not replacing —
this ADR's FR1 segregation guarantee.

## 40. References

- ADR 0000 — Core Architecture Principles (Principle 3, 4)
- ADR 0001 §12.1 — `ledger_accounts.account_type`, the mechanism FR1's segregation relies on
- ADR 0011 — Asset Model
- ADR 0012 — Liquidity Engine (the mechanical enforcement layer this ADR sets targets for)
- ADR 0021 — Banking Core (`TREASURY` classification, defined there and specialized here)

---

## Status

**ACCEPTED.** The Treasury/customer fund segregation rule (FR1) and the
Treasury-sets-targets/Liquidity-enforces-targets division of
responsibility with ADR 0012 (§9) are binding.
