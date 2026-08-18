# ADR-0002: Wallet Engine

- Status: Accepted
- Date: 2026-08-17
- Authors: CIBL Architecture Team

---

# Context

The Wallet Engine is responsible for managing digital asset ownership within the CIBL Digital Asset Infrastructure Platform.

A wallet is not merely a blockchain address.

Instead, it is the logical representation of asset ownership, permissions, policies, balances, identities, and settlement capabilities.

The Wallet Engine SHALL support both custodial and non-custodial models while presenting a unified API.

Wallets SHALL operate consistently across all supported asset classes, including:

- Fiat Currency
- Cryptocurrency
- Stablecoins
- CBDCs
- Tokenized Securities
- Tokenized Commodities
- Tokenized Real Estate
- Digital Gold
- Carbon Credits
- NFTs
- Internal Settlement Assets

Wallet behavior SHALL remain independent of blockchain implementation details.

---

# Decision

CIBL SHALL implement a unified Wallet Engine.

The Wallet Engine SHALL abstract:

- Wallet lifecycle
- Asset ownership
- Blockchain addresses
- Account permissions
- Transaction preparation
- Policy enforcement
- Custody integration
- Ledger integration
- Compliance integration
- Risk validation

Wallets SHALL expose a consistent interface regardless of underlying blockchain or custody provider.

---

# Goals

The Wallet Engine SHALL provide:

- Wallet as a Service (WaaS)
- Multi-chain support
- Multi-asset support
- Multi-tenant isolation
- Enterprise security
- High scalability
- Deterministic behavior
- Policy-driven operations
- Extensibility
- Regulatory readiness

---

# Non-Goals

The Wallet Engine SHALL NOT:

- Execute AML decisions
- Perform KYC verification
- Price assets
- Execute exchange orders
- Calculate FX rates
- Broadcast blockchain transactions directly
- Store private keys internally
- Perform accounting

Those responsibilities belong to dedicated platform services.

---

# Core Principles

The Wallet Engine SHALL follow these principles.

1. Wallets are logical entities.

2. Addresses belong to wallets.

3. Keys belong to custody providers.

4. Balances belong to the Ledger.

5. Policies determine permissions.

6. Blockchain interaction occurs through the Blockchain Gateway.

7. Signing occurs through the Signing Engine.

8. Every action is auditable.

9. Every operation is event-driven.

10. Wallets remain blockchain-agnostic.

---

# Wallet Responsibilities

The Wallet Engine owns:

- Wallet lifecycle
- Address management
- Wallet metadata
- Wallet permissions
- Wallet policies
- Wallet ownership
- Wallet recovery metadata
- Wallet labels
- Wallet relationships
- Wallet routing

The Wallet Engine SHALL NOT own:

- Private keys
- Ledger balances
- Blockchain nodes
- Smart contracts
- Exchange rates
- AML decisions
- Settlement execution

## Wallet Architecture

The Wallet Engine SHALL consist of the following logical components.

Wallet API

↓

Wallet Service

↓

Policy Engine

↓

Address Manager

↓

Custody Adapter

↓

Blockchain Gateway

↓

Signing Engine

↓

Ledger

↓

Event Bus

Each component SHALL have a single responsibility.

---

## Wallet Model

A wallet SHALL contain:

- Wallet ID
- Tenant ID
- Organization ID
- Owner Type
- Owner ID
- Wallet Type
- Wallet Status
- Network Configuration
- Policy Set
- Metadata
- Tags
- Labels
- Created At
- Updated At

Wallet IDs SHALL be globally unique.

Wallet metadata SHALL be extensible.

---

## Wallet Types

Supported wallet types include:

- Personal Wallet
- Business Wallet
- Merchant Wallet
- Treasury Wallet
- Settlement Wallet
- Custody Wallet
- Liquidity Wallet
- Exchange Wallet
- Escrow Wallet
- Fee Wallet
- Reserve Wallet
- Cold Wallet
- Warm Wallet
- Hot Wallet
- Compliance Wallet
- Recovery Wallet

Custom wallet types MAY be added without modifying the engine.

---

## Wallet Status

Wallet lifecycle states include:

Creating

Active

Locked

Frozen

Restricted

Recovering

Archived

Deleted (Logical Only)

Wallet records SHALL never be physically deleted.

---

## Wallet Ownership

Supported ownership models include:

Individual

Organization

Merchant

Institution

Exchange

Treasury

Internal System

Shared Ownership

Multi-signature Group

Programmatic Wallet

Ownership SHALL remain independent of blockchain implementation.

---

## Wallet Identity

Each wallet SHALL expose:

Wallet UUID

Display Name

Alias

External Reference

Tenant Reference

Organization Reference

Public Identifier

Internal Identifier

Blockchain addresses SHALL NOT be used as wallet identifiers.

Wallet identity SHALL remain stable even if blockchain addresses change.


•Wallet Lifecycle
•Address Management
•HD Wallets (BIP32/BIP39/BIP44)
•MPC Wallets
•Smart Contract Wallets
•Account Abstraction (ERC-4337)
•Multi-Chain Wallets
•Address Pools
•Deposit Address Rotation
•Wallet Policies
•Wallet Limits
•Wallet Metadata
•Wallet Events