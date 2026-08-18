# ADR-0011 — Unified Asset Model

Status: Accepted

Authors:
- CIBL Architecture Team

Date:
2026-08

---

# Context

CIBL is not only a crypto platform.

It is a complete Digital Asset Infrastructure Platform.

The platform must support every asset that can exist inside a modern financial ecosystem.

Examples include:

- Fiat
- Stablecoins
- CBDCs
- Cryptocurrencies
- Tokenized Securities
- Tokenized Commodities
- Tokenized Real Estate
- Funds
- Bonds
- ETFs
- NFTs
- Loyalty Points
- Carbon Credits
- Energy Credits
- Internal Ledger Assets

All of them must behave consistently inside every service.

Wallet
Ledger
Settlement
Exchange
Compliance
Risk
Liquidity
Payments

must never implement asset-specific logic whenever possible.

Instead, every asset follows one unified model.

---

# Decision

CIBL defines a canonical Asset abstraction.

Every asset is represented using the same core model.

Services interact only with Asset IDs.

Business logic never depends on concrete asset implementations.

---

# Asset Categories

The platform supports multiple asset families.

## Fiat

USD

EUR

GBP

JPY

AED

SAR

CHF

CAD

AUD

...

---

## Stablecoins

USDT

USDC

DAI

FDUSD

PYUSD

TUSD

...

---

## CBDC

Digital Dollar

Digital Euro

Digital Yuan

Digital Dirham

Future CBDCs

---

## Cryptocurrencies

Bitcoin

Ethereum

Solana

Polygon

BNB

Avalanche

Arbitrum

Optimism

TRON

Litecoin

Monero

...

---

## Tokenized Securities

Stocks

Preferred Shares

Depositary Receipts

Private Equity

---

## Bonds

Government Bonds

Corporate Bonds

Municipal Bonds

Treasury Bills

Commercial Papers

---

## Funds

Mutual Funds

Money Market Funds

Index Funds

Private Funds

Hedge Funds

---

## ETFs

Commodity ETFs

Equity ETFs

Bond ETFs

Crypto ETFs

---

## Commodities

Gold

Silver

Copper

Oil

Gas

Platinum

Palladium

Agricultural Products

---

## Tokenized Real Estate

Residential

Commercial

Industrial

Land

REIT-backed Assets

---

## Digital Collectibles

NFT

Gaming Assets

Membership Tokens

Licenses

---

## Loyalty Assets

Reward Points

Airline Miles

Cashback Credits

Coupons

Gift Cards

---

## Carbon Assets

Carbon Credits

Renewable Energy Certificates

Emission Offsets

---

## Internal Assets

Internal Credits

Settlement Units

Escrow Units

Fee Tokens

Reward Tokens



# Asset Identity
Every asset receives a globally unique Asset ID.

Example

asset_usd

asset_btc

asset_eth

asset_gold

asset_real_estate

asset_cbdc_usd

asset_stock_aapl

Human readable codes are never primary identifiers.

# Asset Metadata
Each asset contains metadata.

Asset ID

Symbol

Display Name

Decimals

Category

Issuer

Country

Jurisdiction

Blockchain

Native Network

Settlement Network

Price Source

Precision

Rounding Rules

Compliance Flags

Liquidity Tier

Risk Tier

Tradability

Transferability

Programmability

Token Standard

Contract Address

Status

# Asset Status
Draft

Pending

Active

Paused

Frozen

Deprecated

Archived

# Asset Types
Native Asset

Issued Asset

Wrapped Asset

Synthetic Asset

Tokenized Asset

Internal Ledger Asset

# Asset Precision

Each asset defines:

Display Precision

Accounting Precision

Settlement Precision

Trading Precision

Pricing Precision





Example


BTC

Display:
8

Accounting:
18

Settlement:
8

Pricing:
8




# Asset Lifecycle

Create

↓

Validate

↓

Approve

↓

Activate

↓

Available

↓

Suspended

↓

Retired


# Asset Registry
Asset Registry is the single source of truth.
Responsibilities
register assets
update metadata
activate assets
deactivate assets
manage supported chains
manage issuers
expose APIs





# Multi-chain Assets
Example
USDC
Ethereum
Polygon
Base
Arbitrum
Solana
Avalanche
Optimism
The Asset Registry stores all chain-specific representations under one logical asset.





# Wrapped Assets
Example
WBTC
Underlying Asset:
Bitcoin
Wrapper:
ERC20
Network:
Ethereum
Relationship stored explicitly.





# Synthetic Assets
Example
Synthetic Gold
Synthetic Oil
Synthetic Tesla
Synthetic S&P500
Pricing comes from Oracle Services.





# Tokenized Assets
Supported
Government Bonds
Corporate Bonds
Funds
Gold
Real Estate
Invoices
Trade Finance
Receivables
Carbon Credits
Private Equity
Infrastructure Projects




# Asset Capabilities
Each asset declares supported operations.

Deposit

Withdraw

Transfer

Trade

Swap

Stake

Bridge

Mint

Burn

Freeze

Hold

Escrow

Settlement

Collateral

Borrow

Lend


Unsupported operations are rejected automatically.







# Asset Risk Profile
Each asset contains:



Risk Score

Liquidity Score

Volatility

AML Category

Sanction Exposure

Country Restrictions

Institutional Only Flag

Retail Availability

Maximum Position

Settlement Window





# Asset Pricing
Assets may receive pricing from:


Internal Market

Exchange API

FX API

Oracle

External Provider

Manual Pricing

Reference Index





# Asset Relationships
Examples

USDC

backed_by

USD

------------

Wrapped BTC

wraps

Bitcoin

------------

Digital Dollar

issued_by

Central Bank

------------

Gold Token

represents

Physical Gold






# Asset Versioning
Assets are immutable identifiers.
Metadata evolves through versions.
Old transactions always reference historical metadata.





# Ledger Integration
Ledger stores only
Asset ID
Amount
Precision
Ledger never stores asset metadata.





# Wallet Integration
Wallets contain balances by Asset ID.
No asset-specific wallet implementation exists.






# Settlement Integration
Settlement validates:
Supported Asset
Settlement Window
Liquidity
Compliance
Jurisdiction
Network Availability





# Exchange Integration
Exchange discovers
Tradable Assets
Supported Pairs
Liquidity Pools
Tick Size
Minimum Order
Maximum Order





# Compliance Integration
Each asset defines:
KYC Level
AML Rules
Sanctions Requirements
Travel Rule Requirement
Restricted Countries
PEP Requirement
Source of Funds Requirement






# Risk Integration
Risk Engine consumes:
Asset Volatility
Liquidity
Historical Risk
Issuer Rating
Settlement Risk
Operational Risk
Counterparty Risk





# Design Consequences
•Advantages
•One unified asset model
•Unlimited asset expansion
•Easy onboarding of new assets
•Consistent APIs
•Chain-independent architecture
•Simplified Ledger
•Simplified Wallet
•Better Compliance
•Better Settlement
•Better Exchange Integration
•Future-proof platform


Trade-offs


•Larger metadata registry
•Additional registry management
•More governance requirements
Accepted.



