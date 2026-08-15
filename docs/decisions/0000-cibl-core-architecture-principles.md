# ADR 0000: CIBL Core Architecture Principles

- **Status:** Accepted
- **Version:** 1.0
- **Date:** August 15, 2026
- **Authors:** CIBL Architecture Team
- **Audience:** All Engineering Teams

---

# 1. Purpose

This document defines the immutable architectural principles of the CIBL platform.

These principles serve as the architectural constitution of the project.

Every future architectural decision, ADR, service, package, module, or infrastructure component MUST comply with this document.

If any future design conflicts with these principles, this document takes precedence unless it is formally superseded by a new accepted ADR.

---

# 2. Vision

CIBL is an independent enterprise-grade Digital Asset Infrastructure Platform.

It provides the core infrastructure required to build digital banking, digital asset custody, payment processing, blockchain settlement, wallet infrastructure, liquidity management, and financial services.

CIBL is designed as infrastructure—not merely an application.

---

# 3. Core Principles

## Principle 1 — Independence

CIBL Core MUST remain completely independent from any third-party custody platform.

No external vendor may become a required dependency of the platform.

Examples include (but are not limited to):

- Fireblocks
- BitGo
- Copper
- Anchorage
- Coinbase Prime
- DFNS

These products may be integrated through optional connectors only.

---

## Principle 2 — Vendor Agnostic

Every subsystem shall be designed to support replacement of external providers without requiring architectural changes.

Replacing a blockchain node provider, cloud provider, custody provider, payment provider, or monitoring solution must not require changes to the business logic.

---

## Principle 3 — Core Ownership

The following systems are first-class components of CIBL and belong to the platform itself:

- Ledger Engine
- Wallet Engine
- Custody Engine
- Vault
- MPC / HSM Layer
- Signing Engine
- Blockchain Gateway
- Address Service
- Block Scanner
- Transaction Broadcaster
- Settlement Engine
- Liquidity Engine
- Compliance Engine
- Risk Engine
- Audit Engine
- Notification Engine

These are not wrappers around external products.

---

## Principle 4 — Ledger is the Source of Truth

Financial truth exists only inside the Ledger.

Balances are derived from ledger entries.

No service may mutate balances directly.

---

## Principle 5 — Immutability

Financial records are immutable.

Ledger entries cannot be modified.

Posted journals cannot be edited.

Corrections must always be implemented through compensating journals.

---

## Principle 6 — Security by Design

Security is part of the architecture—not an optional feature.

All sensitive operations must assume hostile environments.

Secrets shall never be stored in source code.

Private keys must never leave secure cryptographic boundaries.

---

## Principle 7 — API First

Every capability exposed by CIBL shall be available through well-defined APIs.

Internal services communicate through APIs and asynchronous events.

---

## Principle 8 — Event-Driven Architecture

Domain events are first-class citizens.

Services should communicate asynchronously whenever consistency requirements permit.

Synchronous communication should be minimized.

---

## Principle 9 — Domain Separation

Every business capability belongs to exactly one bounded context.

Business logic must never be duplicated across services.

---

## Principle 10 — Modular Monolith to Distributed Services

The architecture shall support evolution from modular components into independently deployable services without redesign.

---

## Principle 11 — Blockchain Native

Blockchain support is a native capability of CIBL.

Supported networks include Bitcoin, Ethereum, Tron, Solana, Polygon, BNB Chain and additional networks through extensible adapters.

Blockchain integration is implemented directly by CIBL.

---

## Principle 12 — Multi-Asset Platform

The platform supports:

- Fiat currencies
- Native blockchain assets
- Stablecoins
- ERC-20 tokens
- TRC-20 tokens
- SPL tokens
- NFTs
- Future digital asset standards

All assets are treated through a unified abstraction.

---

## Principle 13 — Smart Contract Platform

CIBL may develop, deploy and maintain its own smart contracts.

Examples include:

- ERC20
- Stablecoins
- Escrow
- Payment Contracts
- Vesting
- Staking
- Treasury
- DAO
- NFT
- Tokenization

Smart contracts are part of the platform architecture.

---

## Principle 14 — Cloud Agnostic

CIBL shall remain deployable on:

- AWS
- Azure
- Google Cloud
- Private Cloud
- On-Premise
- Sovereign Infrastructure

No cloud provider shall become mandatory.

---

## Principle 15 — Database Agnostic (Within Reason)

Business logic must remain independent from database-specific implementations whenever practical.

Infrastructure-specific optimizations must remain isolated.

---

## Principle 16 — Observability

Every subsystem must expose:

- Logs
- Metrics
- Traces
- Health Checks
- Audit Records

Operational visibility is mandatory.

---

## Principle 17 — Compliance by Design

Compliance requirements are architectural concerns.

The platform shall support:

- KYC
- AML
- Sanctions
- Transaction Monitoring
- Audit
- Regulatory Reporting

---

## Principle 18 — Extensibility

New blockchains, payment providers, assets, settlement rails and integrations must be added without modifying existing core architecture.

Extension is preferred over modification.

---

## Principle 19 — Testing

Every critical financial component requires:

- Unit Tests
- Integration Tests
- End-to-End Tests
- Property-Based Tests
- Security Tests

---

## Principle 20 — Documentation

Architecture is part of the product.

Every major architectural decision must be documented as an ADR.

No undocumented architectural changes are permitted.

---

# Core Architecture Overview

```
                    CIBL Platform

                 Digital Asset Infrastructure

 ┌──────────────────────────────────────────────────────┐
 │ Ledger Engine                                        │
 │ Wallet Engine                                        │
 │ Custody Engine                                       │
 │ MPC / HSM                                            │
 │ Signing Engine                                       │
 │ Blockchain Gateway                                   │
 │ Address Service                                      │
 │ Settlement Engine                                    │
 │ Liquidity Engine                                     │
 │ Risk Engine                                          │
 │ Compliance Engine                                    │
 │ Audit Engine                                         │
 │ Notification Engine                                  │
 └──────────────────────────────────────────────────────┘

                    │

      Bitcoin
      Ethereum
      Tron
      Solana
      Polygon
      BNB
      ...

                    │

       Optional External Connectors

      Fireblocks
      BitGo
      Copper
      Anchorage
      Coinbase Prime
      Others
```

---

# Governance

Any proposal that violates one or more principles defined in this document requires a new ADR that explicitly supersedes this document.

Otherwise, these principles are considered mandatory.

---

# Status

Accepted.

This ADR establishes the architectural constitution of the CIBL platform.