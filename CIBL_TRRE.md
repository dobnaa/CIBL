```



CIBL/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   ├── workflows/
│   │   ├── cd.yml
│   │   ├── ci.yml
│   │   ├── dependency-review.yml
│   │   ├── release.yml
│   │   └── security.yml
│   ├── CODE_OF_CONDUCT.md
│   ├── CODEOWNERS
│   └── PULL_REQUEST_TEMPLATE.md
├── .husky/
│   ├── commit-msg
│   ├── pre-commit
│   └── pre-push
├── .vscode/
│   ├── extensions.json
│   ├── launch.json
│   ├── settings.json
│   └── tasks.json
├── apps/
│   ├── admin/
│   ├── checkout/
│   ├── mobile/
│   │   ├── android/
│   │   ├── ios/
│   │   ├── src/
│   │   ├── app.json
│   │   ├── babel.config.js
│   │   ├── metro.config.js
│   │   ├── package.json
│   │   └── tsconfig.json
│   └── web/
│       ├── public/
│       ├── src/
│       └── tsconfig.json
├── assets/
│   ├── animations/
│   ├── avatars/
│   ├── backgrounds/
│   ├── badges/
│   ├── crypto/
│   │   ├── networks/
│   │   ├── nft/
│   │   └── tokens/
│   ├── flags/
│   ├── fonts/
│   │   ├── cairo/
│   │   ├── inter/
│   │   ├── jetbrains-mono/
│   │   ├── orbitron/
│   │   └── vazirmatn/
│   ├── icons/
│   ├── illustrations/
│   ├── images/
│   ├── logos/
│   ├── placeholders/
│   ├── sounds/
│   └── videos/
├── blockchain/
│   ├── contracts/
│   ├── deployments/
│   ├── networks/
│   ├── scripts/
│   └── tests/
├── database/
│   ├── backups/
│   │   └── .gitkeep
│   ├── migrations/
│   ├── schema/
│   │   └── ledger.sql
│   └── seeds/
├── docs/
│   ├── api/
│   │   ├── openapi/
│   │   │   └── openapi.yaml
│   │   ├── authentication.md
│   │   ├── authorization.md
│   │   ├── error-format.md
│   │   ├── idempotency.md
│   │   ├── pagination.md
│   │   ├── rate-limits.md
│   │   ├── versioning.md
│   │   └── webhooks.md
│   ├── architecture/
│   │   ├── blockchain-flow.md
│   │   ├── deployment-architecture.md
│   │   ├── domain-model.md
│   │   ├── event-catalog.md
│   │   ├── glossary.md
│   │   ├── infrastructure-overview.md
│   │   ├── ledger-booking-flow.md
│   │   ├── ledger-reconciliation.md
│   │   ├── payment-flow.md
│   │   ├── platform-overview.md
│   │   ├── README.md
│   │   ├── sequence-diagrams.md
│   │   ├── service-map.md
│   │   ├── settlement-flow.md
│   │   ├── system-overview.md
│   │   └── wallet-lifecycle.md
│   ├── assets/
│   │   ├── diagrams/
│   │   │   └── .gitkeep
│   │   ├── erd/
│   │   │   └── .gitkeep
│   │   ├── icons/
│   │   │   └── .gitkeep
│   │   ├── images/
│   │   │   └── .gitkeep
│   │   └── sequence/
│   │       └── .gitkeep
│   ├── blockchain/
│   │   ├── address-format.md
│   │   ├── confirmations.md
│   │   ├── deployment-process.md
│   │   ├── fee-policy.md
│   │   ├── smart-contract-development.md
│   │   ├── supported-networks.md
│   │   ├── token-standards.md
│   │   ├── transaction-lifecycle.md
│   │   ├── upgrade-strategy.md
│   │   └── wallet-derivation.md
│   ├── compliance/
│   │   ├── aml.md
│   │   ├── audit-requirements.md
│   │   ├── kyc.md
│   │   ├── sanctions.md
│   │   ├── transaction-monitoring.md
│   │   └── travel-rule.md
│   ├── database/
│   │   ├── backup.md
│   │   ├── indexing.md
│   │   ├── migrations.md
│   │   ├── partitioning.md
│   │   ├── recovery.md
│   │   └── schema.md
│   ├── decisions/
│   │   ├── 0000-cibl-core-architecture-principles.md
│   │   ├── 0001-double-entry-ledger-engine.md
│   │   ├── 0002-wallet-engine.md
│   │   ├── 0003-custody-engine.md
│   │   ├── 0004-blockchain-gateway.md
│   │   ├── 0005-signing-engine.md
│   │   ├── 0006-event-driven-architecture.md
│   │   ├── 0007-service-boundaries.md
│   │   ├── 0008-security-model.md
│   │   ├── 0009-api-design-principles.md
│   │   ├── 0010-multi-tenant-architecture.md
│   │   ├── 0011-asset-model.md
│   │   ├── 0012-liquidity-engine.md
│   │   ├── 0013-settlement-engine.md
│   │   ├── 0014-compliance-engine.md
│   │   ├── 0015-risk-engine.md
│   │   ├── 0016-notification-architecture.md
│   │   ├── 0017-observability.md
│   │   ├── 0018-disaster-recovery.md
│   │   ├── 0019-smart-contract-platform.md
│   │   └── README.md
│   ├── deployment/
│   │   ├── docker.md
│   │   ├── kubernetes.md
│   │   ├── local-development.md
│   │   ├── production.md
│   │   ├── release-process.md
│   │   ├── rollback.md
│   │   ├── scaling.md
│   │   └── staging.md
│   ├── operations/
│   │   ├── alerting.md
│   │   ├── incident-management.md
│   │   ├── logging.md
│   │   ├── monitoring.md
│   │   ├── runbooks.md
│   │   └── sla-slo.md
│   ├── security/
│   │   ├── audit-logging.md
│   │   ├── authentication.md
│   │   ├── authorization.md
│   │   ├── encryption.md
│   │   ├── hsm.md
│   │   ├── incident-response.md
│   │   ├── key-management.md
│   │   ├── mpc.md
│   │   ├── secrets-management.md
│   │   ├── threat-model.md
│   │   └── vulnerability-management.md
│   ├── templates/
│   │   ├── adr-template.md
│   │   ├── api-template.md
│   │   ├── architecture-template.md
│   │   ├── incident-template.md
│   │   └── runbook-template.md
│   ├── testing/
│   │   ├── chaos-testing.md
│   │   ├── e2e-tests.md
│   │   ├── integration-tests.md
│   │   ├── performance-tests.md
│   │   ├── security-tests.md
│   │   ├── strategy.md
│   │   └── unit-tests.md
│   ├── ui-ux/
│   │   ├── accessibility.md
│   │   ├── branding.md
│   │   ├── design-system.md
│   │   ├── localization.md
│   │   └── responsive-design.md
│   ├── CONTRIBUTING.md
│   └── README.md
├── infrastructure/
│   ├── docker/
│   ├── github-actions/
│   ├── kubernetes/
│   ├── monitoring/
│   ├── nginx/
│   ├── observability/
│   └── terraform/
├── packages/
│   ├── api-client/
│   │   ├── src/
│   │   │   ├── assets/
│   │   │   │   ├── cbdc/
│   │   │   │   │   ├── digital-dirham.ts
│   │   │   │   │   ├── digital-dollar.ts
│   │   │   │   │   ├── digital-euro.ts
│   │   │   │   │   ├── digital-yuan.ts
│   │   │   │   │   └── index.ts
│   │   │   │   ├── crypto/
│   │   │   │   │   ├── bridge.ts
│   │   │   │   │   ├── coins.ts
│   │   │   │   │   ├── index.ts
│   │   │   │   │   ├── nft.ts
│   │   │   │   │   ├── staking.ts
│   │   │   │   │   └── tokens.ts
│   │   │   │   ├── fiat/
│   │   │   │   │   ├── currencies/
│   │   │   │   │   │   ├── aed.ts
│   │   │   │   │   │   ├── ars.ts
│   │   │   │   │   │   ├── aud.ts
│   │   │   │   │   │   ├── bhd.ts
│   │   │   │   │   │   ├── brl.ts
│   │   │   │   │   │   ├── cad.ts
│   │   │   │   │   │   ├── chf.ts
│   │   │   │   │   │   ├── clp.ts
│   │   │   │   │   │   ├── cny.ts
│   │   │   │   │   │   ├── cop.ts
│   │   │   │   │   │   ├── czk.ts
│   │   │   │   │   │   ├── dkk.ts
│   │   │   │   │   │   ├── egp.ts
│   │   │   │   │   │   ├── eur.ts
│   │   │   │   │   │   ├── gbp.ts
│   │   │   │   │   │   ├── hkd.ts
│   │   │   │   │   │   ├── huf.ts
│   │   │   │   │   │   ├── idr.ts
│   │   │   │   │   │   ├── index.ts
│   │   │   │   │   │   ├── inr.ts
│   │   │   │   │   │   ├── irr.ts
│   │   │   │   │   │   ├── jpy.ts
│   │   │   │   │   │   ├── kes.ts
│   │   │   │   │   │   ├── krw.ts
│   │   │   │   │   │   ├── kwd.ts
│   │   │   │   │   │   ├── mad.ts
│   │   │   │   │   │   ├── mxn.ts
│   │   │   │   │   │   ├── myr.ts
│   │   │   │   │   │   ├── ngn.ts
│   │   │   │   │   │   ├── nok.ts
│   │   │   │   │   │   ├── nzd.ts
│   │   │   │   │   │   ├── omr.ts
│   │   │   │   │   │   ├── pen.ts
│   │   │   │   │   │   ├── php.ts
│   │   │   │   │   │   ├── pln.ts
│   │   │   │   │   │   ├── qar.ts
│   │   │   │   │   │   ├── ron.ts
│   │   │   │   │   │   ├── rub.ts
│   │   │   │   │   │   ├── sar.ts
│   │   │   │   │   │   ├── sek.ts
│   │   │   │   │   │   ├── sgd.ts
│   │   │   │   │   │   ├── thb.ts
│   │   │   │   │   │   ├── twd.ts
│   │   │   │   │   │   ├── usd.ts
│   │   │   │   │   │   ├── vnd.ts
│   │   │   │   │   │   └── zar.ts
│   │   │   │   │   ├── services/
│   │   │   │   │   │   ├── exchange.ts
│   │   │   │   │   │   ├── iban.ts
│   │   │   │   │   │   ├── index.ts
│   │   │   │   │   │   ├── liquidity.ts
│   │   │   │   │   │   ├── routing.ts
│   │   │   │   │   │   ├── settlement.ts
│   │   │   │   │   │   └── swift.ts
│   │   │   │   │   ├── accounts.ts
│   │   │   │   │   ├── constants.ts
│   │   │   │   │   ├── index.ts
│   │   │   │   │   └── types.ts
│   │   │   │   ├── rwa/
│   │   │   │   │   ├── index.ts
│   │   │   │   │   ├── tokenized-bonds.ts
│   │   │   │   │   ├── tokenized-funds.ts
│   │   │   │   │   ├── tokenized-gold.ts
│   │   │   │   │   └── tokenized-real-estate.ts
│   │   │   │   ├── stablecoins/
│   │   │   │   │   ├── dai.ts
│   │   │   │   │   ├── fdusd.ts
│   │   │   │   │   ├── index.ts
│   │   │   │   │   ├── pyusd.ts
│   │   │   │   │   ├── tusd.ts
│   │   │   │   │   ├── usdc.ts
│   │   │   │   │   └── usdt.ts
│   │   │   │   ├── asset-catalog.ts
│   │   │   │   ├── asset-types.ts
│   │   │   │   ├── exchange-rates.ts
│   │   │   │   └── index.ts
│   │   │   ├── auth/
│   │   │   │   ├── change-password.ts
│   │   │   │   ├── forgot-password.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── login.ts
│   │   │   │   ├── logout.ts
│   │   │   │   ├── refresh-token.ts
│   │   │   │   ├── register.ts
│   │   │   │   ├── reset-password.ts
│   │   │   │   ├── sessions.ts
│   │   │   │   └── two-factor.ts
│   │   │   ├── blockchain/
│   │   │   │   ├── arbitrum.ts
│   │   │   │   ├── avalanche.ts
│   │   │   │   ├── base.ts
│   │   │   │   ├── bitcoin.ts
│   │   │   │   ├── bsc.ts
│   │   │   │   ├── ethereum.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── optimism.ts
│   │   │   │   ├── polygon.ts
│   │   │   │   ├── solana.ts
│   │   │   │   └── tron.ts
│   │   │   ├── client/
│   │   │   │   ├── deserializer.ts
│   │   │   │   ├── http-client.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── interceptors.ts
│   │   │   │   ├── middleware.ts
│   │   │   │   ├── request.ts
│   │   │   │   ├── response.ts
│   │   │   │   ├── rest-client.ts
│   │   │   │   ├── retry.ts
│   │   │   │   ├── serializer.ts
│   │   │   │   └── websocket-client.ts
│   │   │   ├── compliance/
│   │   │   │   ├── aml.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── kyc.ts
│   │   │   │   ├── risk.ts
│   │   │   │   └── sanctions.ts
│   │   │   ├── config/
│   │   │   │   ├── client-config.ts
│   │   │   │   ├── environments.ts
│   │   │   │   └── index.ts
│   │   │   ├── constants/
│   │   │   │   ├── endpoints.ts
│   │   │   │   ├── headers.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── timeouts.ts
│   │   │   │   └── versions.ts
│   │   │   ├── errors/
│   │   │   │   ├── api-error.ts
│   │   │   │   ├── authentication-error.ts
│   │   │   │   ├── authorization-error.ts
│   │   │   │   ├── blockchain-error.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── network-error.ts
│   │   │   │   ├── timeout-error.ts
│   │   │   │   └── validation-error.ts
│   │   │   ├── exchange/
│   │   │   │   ├── index.ts
│   │   │   │   ├── market.ts
│   │   │   │   ├── orderbook.ts
│   │   │   │   ├── quotes.ts
│   │   │   │   └── swap.ts
│   │   │   ├── payments/
│   │   │   │   ├── capture.ts
│   │   │   │   ├── create-payment.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── invoices.ts
│   │   │   │   ├── payment-status.ts
│   │   │   │   └── refund.ts
│   │   │   ├── settlements/
│   │   │   │   ├── batches.ts
│   │   │   │   ├── index.ts
│   │   │   │   └── settlements.ts
│   │   │   ├── telemetry/
│   │   │   │   ├── index.ts
│   │   │   │   ├── logging.ts
│   │   │   │   ├── metrics.ts
│   │   │   │   └── tracing.ts
│   │   │   ├── types/
│   │   │   │   ├── auth.ts
│   │   │   │   ├── blockchain.ts
│   │   │   │   ├── common.ts
│   │   │   │   ├── exchange.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── payment.ts
│   │   │   │   ├── settlement.ts
│   │   │   │   └── wallet.ts
│   │   │   ├── users/
│   │   │   │   ├── index.ts
│   │   │   │   ├── permissions.ts
│   │   │   │   ├── preferences.ts
│   │   │   │   ├── profile.ts
│   │   │   │   └── verification.ts
│   │   │   ├── utils/
│   │   │   │   ├── formatter.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── pagination.ts
│   │   │   │   ├── parser.ts
│   │   │   │   ├── query-builder.ts
│   │   │   │   └── validator.ts
│   │   │   ├── wallets/
│   │   │   │   ├── addresses.ts
│   │   │   │   ├── balances.ts
│   │   │   │   ├── broadcast.ts
│   │   │   │   ├── create-wallet.ts
│   │   │   │   ├── estimate-fee.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── sign.ts
│   │   │   │   ├── transactions.ts
│   │   │   │   ├── transfer.ts
│   │   │   │   └── wallet-details.ts
│   │   │   ├── webhooks/
│   │   │   │   ├── events.ts
│   │   │   │   ├── index.ts
│   │   │   │   ├── subscribe.ts
│   │   │   │   └── unsubscribe.ts
│   │   │   └── index.ts
│   │   ├── tests/
│   │   │   ├── fixtures/
│   │   │   │   └── .gitkeep
│   │   │   ├── integration/
│   │   │   │   └── .gitkeep
│   │   │   ├── mocks/
│   │   │   │   └── .gitkeep
│   │   │   ├── unit/
│   │   │   │   └── .gitkeep
│   │   │   └── {unit,integration,mocks,fixtures}/
│   │   ├── CHANGELOG.md
│   │   ├── LICENSE
│   │   ├── package.json
│   │   ├── README.md
│   │   ├── tsconfig.json
│   │   ├── tsup.config.ts
│   │   └── vitest.config.ts
│   ├── auth/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── blockchain/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── build-config/
│   │   ├── eslint.config.js
│   │   ├── package.json
│   │   ├── prettier.config.js
│   │   ├── README.md
│   │   ├── tsconfig.package.json
│   │   ├── tsup.config.base.ts
│   │   └── vitest.config.ts
│   ├── config/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── constants/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── crypto/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── design-tokens/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── env/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── errors/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── hooks/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── i18n/
│   │   ├── src/
│   │   │   ├── locales/
│   │   │   │   ├── ar/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── de/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── en/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── es/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── fa/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── fr/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── hi/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── id/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── it/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── ja/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── ko/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── nl/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── pt/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── ru/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── tr/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── uk/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── ur/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── vi/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   ├── zh-CN/
│   │   │   │   │   ├── admin.json
│   │   │   │   │   ├── api.json
│   │   │   │   │   ├── auth.json
│   │   │   │   │   ├── blockchain.json
│   │   │   │   │   ├── common.json
│   │   │   │   │   ├── compliance.json
│   │   │   │   │   ├── dashboard.json
│   │   │   │   │   ├── errors.json
│   │   │   │   │   ├── ledger.json
│   │   │   │   │   ├── metadata.json
│   │   │   │   │   ├── notification.json
│   │   │   │   │   ├── payment.json
│   │   │   │   │   ├── profile.json
│   │   │   │   │   ├── risk.json
│   │   │   │   │   ├── security.json
│   │   │   │   │   ├── settings.json
│   │   │   │   │   ├── settlement.json
│   │   │   │   │   ├── validation.json
│   │   │   │   │   └── wallet.json
│   │   │   │   └── zh-TW/
│   │   │   │       ├── admin.json
│   │   │   │       ├── api.json
│   │   │   │       ├── auth.json
│   │   │   │       ├── blockchain.json
│   │   │   │       ├── common.json
│   │   │   │       ├── compliance.json
│   │   │   │       ├── dashboard.json
│   │   │   │       ├── errors.json
│   │   │   │       ├── ledger.json
│   │   │   │       ├── metadata.json
│   │   │   │       ├── notification.json
│   │   │   │       ├── payment.json
│   │   │   │       ├── profile.json
│   │   │   │       ├── risk.json
│   │   │   │       ├── security.json
│   │   │   │       ├── settings.json
│   │   │   │       ├── settlement.json
│   │   │   │       ├── validation.json
│   │   │   │       └── wallet.json
│   │   │   ├── config.ts
│   │   │   ├── currency.ts
│   │   │   ├── dates.ts
│   │   │   ├── detector.ts
│   │   │   ├── formatter.ts
│   │   │   ├── index.ts
│   │   │   ├── interpolation.ts
│   │   │   ├── numbers.ts
│   │   │   ├── pluralization.ts
│   │   │   ├── resources.ts
│   │   │   ├── types.ts
│   │   │   └── validation.ts
│   │   ├── package.json
│   │   ├── README.md
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── logger/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── sdk/
│   │   ├── src/
│   │   │   ├── auth/
│   │   │   │   └── index.ts
│   │   │   ├── blockchain/
│   │   │   │   └── index.ts
│   │   │   ├── fiat/
│   │   │   │   └── index.ts
│   │   │   ├── payment/
│   │   │   │   └── index.ts
│   │   │   ├── wallet/
│   │   │   │   └── index.ts
│   │   │   ├── webhook/
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── shared/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── telemetry/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── types/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── ui/
│   │   ├── src/
│   │   │   ├── components/
│   │   │   │   └── index.ts
│   │   │   ├── icons/
│   │   │   │   └── index.ts
│   │   │   ├── layouts/
│   │   │   │   └── index.ts
│   │   │   ├── themes/
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   ├── utils/
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
│   └── validators/
│       ├── src/
│       │   └── index.ts
│       ├── package.json
│       ├── tsconfig.json
│       └── tsup.config.ts
├── scripts/
│   ├── blockchain/
│   ├── bootstrap/
│   ├── build/
│   ├── database/
│   ├── docker/
│   ├── release/
│   └── utils/
├── services/
│   ├── address/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── analytics/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── api/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── audit/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── auth/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── block-scanner/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── blockchain/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── broadcaster/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── compliance/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── custody/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── exchange/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── fiat/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── fx/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── ledger/
│   │   ├── src/
│   │   │   ├── accounts/
│   │   │   │   └── index.ts
│   │   │   ├── audit/
│   │   │   │   └── index.ts
│   │   │   ├── balances/
│   │   │   │   └── index.ts
│   │   │   ├── idempotency/
│   │   │   │   └── index.ts
│   │   │   ├── journals/
│   │   │   │   └── index.ts
│   │   │   ├── postings/
│   │   │   │   └── index.ts
│   │   │   ├── reconciliation/
│   │   │   │   └── index.ts
│   │   │   ├── reversals/
│   │   │   │   └── index.ts
│   │   │   ├── ledger.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── liquidity/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── notification/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── payment/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── risk/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── settlement/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── signing/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── vault/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   ├── wallet/
│   │   ├── src/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── test/
│   │   │   └── .gitkeep
│   │   ├── nest-cli.json
│   │   ├── package.json
│   │   └── tsconfig.json
│   └── webhook/
│       ├── src/
│       │   ├── app.module.ts
│       │   └── main.ts
│       ├── test/
│       │   └── .gitkeep
│       ├── nest-cli.json
│       ├── package.json
│       └── tsconfig.json
├── templates/
│   ├── emails/
│   ├── notifications/
│   ├── pdf/
│   └── reports/
├── tests/
│   ├── e2e/
│   ├── fixtures/
│   ├── integration/
│   ├── performance/
│   └── security/
├── .editorconfig
├── .env.example
├── .env.production.example
├── .env.staging.example
├── .env.test.example
├── .gitignore
├── .npmrc
├── .prettierignore
├── .prettierrc
├── CHANGELOG.md
├── commitlint.config.js
├── CONTRIBUTING.md
├── eslint.config.js
├── lint-staged.config.js
├── package.json
├── pnpm-workspace.yaml
├── README.md
├── SECURITY.md
├── SUPPORT.md
├── tsconfig.base.json
├── tsconfig.node.json
├── tsconfig.paths.json
└── turbo.json









```