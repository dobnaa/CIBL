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