```

CIBL/
│
├── assets/                                    ✅ ریشه (Shared Static Assets)
│   ├── animations/                            (Lottie/JSON animations)
│   ├── avatars/                               (Default user/business avatars)
│   ├── backgrounds/                           (Hero images, patterns)
│   ├── badges/                                (New, Beta, Verified badges)
│   ├── crypto/                                (دارایی‌های دیجیتال)
│   │   ├── networks/                          (Eth, Sol, Btc logos)
│   │   ├── tokens/                            (USDC, USDT, DAI)
│   │   └── nft/                               (Placeholder NFTs)
│   ├── flags/                                 (کشورها برای فیات)
│   ├── fonts/                                 (Inter, Vazirmatn, Orbitron)
│   ├── icons/                                 (سیستم، منو، بستن)
│   ├── illustrations/                         (Empty states, success, error)
│   ├── images/                                (عمومی - مثل تصاویر بلاگ)
│   ├── logos/                                 (لوگوهای برند CIBL)
│   ├── placeholders/                          (تصاویر شاخص برای لودینگ)
│   ├── sounds/                                (Notif sounds, click feedback)
│   ├── videos/                                (Tutorials, landing promos)
│   ├── themes/                                (SVG Gradients, Glass Effects, Patterns)
│   └── certificates/                          (Visa, Mastercard, PayPal, SEPA, Swift)
│
├── apps/                                      ✅ اپلیکیشن‌های نهایی
│   ├── web/                                   (React + Vite + Tailwind)
│   ├── mobile/                                (React Native + Expo)
│   ├── admin/                                 (پنل داخلی مدیریت)
│   └── checkout/                              (درگاه پرداخت سبک)
│
├── services/                                  ✅ Domain-Driven Design (Bounded Contexts)
│   ├── api/                                   (Gateway - Routing, Rate Limit)
│   ├── auth/                                  (Passkey, 2FA, JWT, Sessions)
│   ├── wallet/                                (ایجاد کیف‌پول، آدرس‌دهی، گس)
│   ├── payments/                              (Intent, Checkout, Refunds)
│   ├── ledger/                                (دفتر کل دوبل)
│   ├── settlement/                            (تسویه رمز و فیات)
│   ├── compliance/                            (Sanctions, Travel Rule)
│   ├── risk/                                  (Fraud Scoring, Rule Engine)
│   ├── notifications/                         (Email, SMS, Push)
│   ├── webhook/                               (مدیریت و تحویل رویدادها)
│   ├── kyc/                                   (احراز هویت افراد)
│   ├── fiat/                                  (SEPA, ACH, Wire)
│   ├── exchange/                              (OTC, Liquidity, Swap)
│   ├── blockchain/                            (RPC, Indexer, Reorg Handler)
│   ├── audit/                                 (ثبت لاگ‌های امنیتی و انطباق)
│   └── merchant/                              (مدیریت کسب‌وکارها، تیم‌ها)
│
├── packages/                                  ✅ کتابخانه‌های مشترک (Shared Libraries)
│   ├── config/                                (Runtime configs: env, networks, fees)
│   ├── crypto/                                (Encryption, Signing, MPC helpers)
│   ├── eslint-config/                         (Presets برای تمام پروژه‌ها)
│   ├── i18n/                                  (فایل‌های ترجمه و تشخیص RTL)
│   ├── logger/                                (Structured logging with context)
│   ├── types/                                 (Global TS interfaces)
│   ├── ui/                                    (کامپوننت‌های مشترک React)
│   ├── utils/                                 (توابع خالص Helper)
│   ├── validators/                            (اسکیماهای Zod)
│   ├── api-client/                            (HTTP Clients for Frontend)
│   └── hooks/                                 (React Hooks مشترک)
│
├── blockchain/                                ✅ قراردادها و اسکریپت‌های زنجیره‌ای
│   ├── adapters/                              (لایه انتزاعی برای اتصال به زنجیره‌ها)
│   ├── contracts/                             (سورس Solidity / Rust Anchor)
│   ├── keys/                                  (اسکریپت‌های تولید کلید)
│   ├── networks/                              (تنظیمات Mainnet/Testnet)
│   ├── scripts/                               (Deploy, Verify, Upgrade)
│   ├── shared/                                (یوتیلیتی‌های مشترک بلاکچین)
│   └── testnet/                               (Faucet, Local Validator)
│
├── database/
│   ├── migrations/                            (اسکریپت‌های تغییر ساختار)
│   ├── seeds/                                 (داده‌های اولیه)
│   └── schemas/                               (تعاریف Prisma/TypeORM)
│
├── infrastructure/                            ✅ (Docker, K8s, Terraform, Monitoring)
│   ├── docker/
│   ├── kubernetes/
│   ├── terraform/
│   └── monitoring/                            (Prometheus, Grafana, Loki)
│
├── config/                                    ✅ (ابزارهای توسعه - Development Tooling)
│   ├── nginx/
│   ├── eslint/
│   ├── prettier/
│   ├── commitlint/
│   └── semantic-release/
│
├── docs/                                      ✅ مستندات کامل
│   ├── api/                                   (OpenAPI, Postman collections)
│   ├── architecture/                          (System Design, Data Flow)
│   ├── deployment/                            (CI/CD, Environment setup)
│   ├── developer/                             (SDK, Webhooks guide)
│   ├── security/                              (Key Management, Threat Model)
│   ├── user/                                  (راهنمای کاربران و کسب‌وکارها)
│   └── adr/                                   (Architecture Decision Records) ⭐
│
├── scripts/                                   (Setup, Build, Release, Sync)
├── tests/                                     (Unit, Integration, E2E, Load)
├── .github/                                   (Workflows, Issue/PR templates)
│
├── tsconfig.base.json                         (تنظیمات پایه TypeScript)
├── tsconfig.paths.json                        (⭐ تعریف متمرکز Aliasها)
├── tsconfig.node.json                         (برای اسکریپت‌های Node.js)
├── tsconfig.json                              (Reference به فایل‌های بالا)
│
├── pnpm-workspace.yaml                        (packages: apps/*, services/*, packages/*, blockchain/*)
├── turbo.json                                 (Pipelineهای ساخت و تست)
├── package.json
├── docker-compose.yml
├── .env.example
└── README.md



```
