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
│   └── seeds/
├── docs/
│   ├── api/
│   ├── architecture/
│   ├── blockchain/
│   ├── decisions/
│   ├── deployment/
│   ├── security/
│   └── ui-ux/
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
│   │   │   └── index.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tsup.config.ts
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
│   │   │   └── index.ts
│   │   ├── package.json
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
│   ├── blockchain/
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
│   ├── exchange/
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
