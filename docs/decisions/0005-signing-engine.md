ADR-0005: Signing Engine

- Status: Accepted
- Date: 2026-08-17
- Authors: CIBL Architecture Team
- Supersedes: None
- Superseded By: None

---

Context

The CIBL Digital Asset Infrastructure Platform manages digital assets, fiat assets, tokenized assets, CBDCs, payment transactions, settlements, custody operations, and institutional wallets across multiple blockchain networks.

Every blockchain transaction ultimately requires a cryptographic signature.

Examples include:

- Wallet transfers
- Settlement execution
- Merchant payouts
- Treasury movement
- Liquidity balancing
- Smart contract execution
- Bridge transfers
- Staking
- Validator operations
- Exchange withdrawals
- CBDC transactions
- Token minting
- Token burning
- Asset issuance
- Asset redemption

Signing private keys represent the highest security risk in the entire platform.

Compromise of a signing key results in irreversible asset loss.

Therefore signing cannot be implemented inside Wallet Service or Blockchain Gateway.

A dedicated Signing Engine is required.

---

Problem Statement

The platform requires:

- hardware-backed security
- MPC support
- HSM support
- multisignature support
- approval workflows
- policy enforcement
- role separation
- key isolation
- auditability
- deterministic signing
- high throughput
- low latency
- support for many blockchains

---

Decision

CIBL introduces an independent Signing Engine.

The Signing Engine owns the entire lifecycle of transaction signing.

No other service is allowed to access private keys.

Private keys never leave secure cryptographic environments.

---

Responsibilities

The Signing Engine is responsible for:

- key management abstraction
- transaction signing
- signing authorization
- multisig orchestration
- MPC coordination
- HSM integration
- approval workflow execution
- signing policy enforcement
- audit logging
- cryptographic verification
- signature validation
- transaction serialization validation
- nonce protection
- replay protection
- signing metrics

---

Supported Signing Methods

- Software Key
- Hardware Security Module (HSM)
- Multi-Party Computation (MPC)
- Threshold Signatures (TSS)
- Multi-Signature Wallets
- Cold Wallet Signing
- Offline Signing
- Air-Gapped Signing
- External Custodian Signing

---

Supported Blockchain Families

UTXO

- Bitcoin
- Litecoin
- Dogecoin

EVM

- Ethereum
- Polygon
- Arbitrum
- Optimism
- Avalanche
- Base
- BNB Chain

Account Based

- Solana
- Tron
- Near
- Aptos
- Sui

---

Signing Flow

Wallet Service

↓

Create Unsigned Transaction

↓

Blockchain Gateway

↓

Signing Engine

↓

Policy Evaluation

↓

Approval Workflow

↓

HSM / MPC

↓

Digital Signature

↓

Signature Validation

↓

Blockchain Gateway

↓

Broadcast

↓

Confirmation

---

Key Management Model

The engine never stores raw private keys in application memory.

Instead it stores:

- key identifiers
- metadata
- algorithm
- blockchain
- policy
- custody provider
- permissions

Actual private keys remain inside:

- HSM
- MPC Cluster
- External Custodian

---

Supported Algorithms

- secp256k1
- Ed25519
- P-256
- Schnorr
- BLS (future)

---

Security Policies

Each signing request is evaluated against policies.

Examples:

- maximum transfer amount
- organization policy
- tenant policy
- country restriction
- destination whitelist
- blockchain restriction
- token restriction
- approval count
- business hours
- geographic restrictions
- velocity limits
- AML result
- Risk score

---

Approval Workflows

Example:

Amount < 10,000 USD

↓

Automatic Approval

↓

Sign

Amount > 100,000 USD

↓

Compliance Approval

↓

Treasury Approval

↓

Executive Approval

↓

Sign

---

Multi-Signature Support

Supported models:

- 2-of-3
- 3-of-5
- 5-of-7
- configurable threshold

Supported technologies:

- Bitcoin Multisig
- Safe Smart Accounts
- MPC Threshold Signing

---

Replay Protection

The Signing Engine validates:

- nonce
- chain ID
- network
- expiration
- transaction hash
- replay domain

before producing a signature.

---

Audit Requirements

Every signing request records:

- Request ID
- Tenant ID
- Wallet ID
- Key ID
- Blockchain
- Asset
- Amount
- Signer
- Policy Result
- Approval Chain
- Timestamp
- Device
- IP Address
- Result

Audit records are immutable.

---

High Availability

The engine supports:

- active-active deployment
- stateless workers
- replicated policy cache
- multiple HSM clusters
- multiple MPC nodes
- automatic failover
- regional redundancy

---

Performance Targets

Metric| Target
Signing Latency| <100 ms (HSM)
MPC Signing| <500 ms
Throughput| >5,000 sign/sec
Availability| 99.99%
Recovery Time| <5 minutes

---

API Responsibilities

The Signing Engine exposes internal APIs only.

Typical operations:

- Create Signing Request
- Validate Request
- Evaluate Policy
- Request Approval
- Produce Signature
- Verify Signature
- Cancel Request
- Query Status

---

Event Publication

The engine emits:

- SigningRequested
- SigningApproved
- SigningRejected
- SigningStarted
- SignatureCreated
- SignatureVerified
- SigningFailed
- KeyRotated
- KeyDisabled
- PolicyViolationDetected

---

Failure Handling

Possible failures include:

- HSM unavailable
- MPC quorum failure
- Invalid nonce
- Policy rejection
- Approval timeout
- Blockchain mismatch
- Unsupported algorithm
- Invalid transaction
- Signature verification failure

No transaction is broadcast unless signature verification succeeds.

---

Compliance

The engine supports:

- PCI DSS
- ISO 27001
- SOC 2
- FATF recommendations
- Travel Rule integrations
- regional custody regulations

---

Consequences

Advantages

- Maximum key security
- Separation of duties
- Centralized policy enforcement
- Multi-chain support
- Hardware-backed signing
- Enterprise custody integration
- Complete auditability
- Regulatory compliance
- Scalable architecture

Trade-offs

- Additional network hop
- Higher operational complexity
- Dependency on HSM/MPC infrastructure
- Increased deployment requirements

---

Related ADRs

- ADR-0002 Wallet Engine
- ADR-0003 Custody Engine
- ADR-0004 Blockchain Gateway
- ADR-0006 Event-Driven Architecture
- ADR-0008 Security Model
- ADR-0010 Multi-Tenant Architecture
- ADR-0013 Settlement Engine