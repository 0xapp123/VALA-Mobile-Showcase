# VALA — Short-Form Video Social App with Crypto Wallet (React Native / Expo SDK 53)

<!-- SEO: react native tiktok clone, expo video app, solana mobile wallet, short form video app, react native social media app, expo sdk 53 project, typescript mobile app, defi mobile app, react native video player, tiktok style feed react native -->

> **A production-grade TikTok-style short-form video platform with built-in Solana DeFi wallet, AI-powered content discovery, and creator rewards — built with React Native, Expo SDK 53, and TypeScript.**

[![VALA Social](https://img.shields.io/badge/🌐_Website-vala.social-black?style=for-the-badge)](https://www.vala.social/)
[![VALA Finance](https://img.shields.io/badge/💰_Finance-vala.finance-black?style=for-the-badge)](https://vala.finance/)
[![Download App](https://img.shields.io/badge/📲-Download_App-brightgreen?style=for-the-badge)](https://www.vala.social/)

[![React Native](https://img.shields.io/badge/React_Native-0.79-61DAFB?style=flat-square&logo=react&logoColor=white)](https://reactnative.dev/)
[![Expo SDK](https://img.shields.io/badge/Expo_SDK-53-000020?style=flat-square&logo=expo&logoColor=white)](https://expo.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.8-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=white)](https://react.dev/)
[![Solana](https://img.shields.io/badge/Solana-Web3-9945FF?style=flat-square&logo=solana&logoColor=white)](https://solana.com/)
[![Platform](https://img.shields.io/badge/Platform-iOS_|_Android-lightgrey?style=flat-square&logo=apple&logoColor=white)](https://www.vala.social/)

**Role:** Lead Mobile Engineer &nbsp;|&nbsp; **Company:** [VALA Labs Ltd](https://www.vala.social/) &nbsp;|&nbsp; **Platforms:** iOS & Android

---

## 📑 Table of Contents

- [What Is VALA?](#-what-is-vala--short-form-video-meets-defi)
- [Architecture Overview](#️-architecture-overview)
- [Video Player System](#-tiktok-style-video-player-system--react-native)
- [AI-Powered Content Discovery (VISION)](#-ai-powered-content-discovery-vision)
- [Solana Crypto Wallet & DeFi](#-solana-crypto-wallet--defi-integration)
- [Authentication & Security](#-authentication--security)
- [Real-Time Chat System](#-real-time-chat--messaging)
- [Performance Optimization](#-react-native-performance-optimization)
- [CI/CD & Deployment](#-cicd--deployment-pipeline)
- [Project Scale](#-project-scale--metrics)
- [Full Tech Stack](#️-full-tech-stack)
- [Screenshots](#-app-screenshots)
- [FAQ](#-frequently-asked-questions)
- [Contact & Hire Me](#-contact--hire-me)

---

## 🧠 What Is VALA — Short-Form Video Meets DeFi

**VALA** is a cross-platform mobile app (iOS & Android) that combines **short-form video social media** with **decentralized finance (DeFi)** — built entirely with **React Native** and **Expo SDK 53**.

Think **TikTok + crypto wallet** in one app:

- **[vala.social](https://www.vala.social/)** — A vibe-first content discovery platform powered by **VISION**, a multimodal AI recommendation engine. It understands mood, energy, and aesthetics — not just views and keywords. Content travels across 30+ languages with auto-captions and translations.
- **[vala.finance](https://vala.finance/)** — A built-in **Solana wallet** with token swaps, peer-to-peer tipping, and fiat-to-crypto onramp (MoonPay) so creators earn directly inside the app.

Users scroll a feed that "gets" them, discover content by vibe (hype, cozy, chaotic, calm, nostalgic), earn **Sparks** (in-app rewards) for engagement, and trade crypto — all without leaving the app.

---

## 🏗️ Architecture Overview

```
┌──────────────────────────────────────────────────────┐
│                    VALA Mobile App                   │
├──────────┬──────────┬───────────┬──────────┬─────────┤
│   Feed   │ Discover │  Create   │   Chat   │ Profile │
│ (Video)  │ (VISION) │ (Upload)  │ (Stream) │ (Wallet)│
├──────────┴──────────┴───────────┴──────────┴─────────┤
│                    Shared Layer                      │
│  ┌──────────┐ ┌──────────┐ ┌──────┐ ┌──────────────┐ │
│  │  Video   │ │   Auth   │ │ API  │ │    State     │ │
│  │ Player   │ │  (Privy/ │ │Layer │ │  (Zustand +  │ │
│  │  Pool    │ │  Magic)  │ │      │ │ React Query) │ │
│  └──────────┘ └──────────┘ └──────┘ └──────────────┘ │
├──────────────────────────────────────────────────────┤
│              Platform / Native Layer                 │
│  expo-video · Solana Web3 · Firebase · Sentry · EAS  │
└──────────────────────────────────────────────────────┘
```

---

## 🎬 TikTok-Style Video Player System — React Native

> Production-grade vertical video feed with player pooling, HLS prefetch, and real-time performance monitoring.

Building a smooth TikTok-style video feed in React Native is one of the hardest mobile engineering challenges. Here's how this system works:

- **Player Pool Architecture (`PlayerPoolV2`)** — A recycling pool that manages `expo-video` player instances to minimize memory. Only a few players exist at a time; they bind/unbind as cells enter/leave the viewport — similar to how RecyclerView works on Android.
- **Playback Gate** — Controls when videos play based on viewport visibility, user interaction state, and app lifecycle (foreground/background). Prevents multiple videos from playing simultaneously.
- **HLS Prefetch** — Pre-fetches upcoming video HLS segments so playback starts instantly on swipe. Zero-delay transitions between videos.
- **Background Video Upload** — Users record or select a video, then navigate away while it uploads via a native background upload task. Upload progress is tracked via Zustand store.
- **TTFF (Time To First Frame) Tracking** — End-to-end performance pipeline measuring: player creation → status ready → first frame rendered → play called. All TTFF metrics are logged to Sentry in production.
- **Stall Detection** — Monitors loading stalls (>3s) and playback freezes in real-time. Auto-reports to Sentry with video ID and playback position context.
- **Conditional Production Logging** — ~70-80% log volume reduction in production. Critical metrics (TTFF, stalls, errors) always ship to Sentry; verbose lifecycle logs are development-only.
- **Watch Event Tracking** — Analytics tracking watch duration, completion rate, replay count, and engagement signals for the recommendation engine.
- **Video Diagnostics** — Built-in diagnostic tools for debugging playback issues on real devices.

---

## 🔮 AI-Powered Content Discovery (VISION)

> Multimodal AI recommendation engine — content discovery by mood, not just metrics.

- **Vibe Search** — Natural language search powered by multimodal embeddings. Users search by feeling: *"cozy rainy day anime vibes"*, *"hype gym motivation"*, *"nostalgic 90s aesthetic"*.
- **Vibe Sections** — Discover feed organized by mood/energy categories (hype, calm, nostalgic, chaotic, cozy) — not just "trending" or "for you".
- **Trending Velocity Algorithm** — Real-time trending that factors engagement acceleration and velocity, not just raw view counts. Surfaces genuinely emerging content.
- **AI Comment Quality Analysis** — Comment scoring powered by VISION API to surface meaningful conversations and filter noise.
- **Auto-Translation (30+ Languages)** — Comments and captions auto-translated across 30+ languages so content travels globally.
- **CMS-Driven Discover Feed** — Dynamic discover page with configurable sections, banners, and curated collections powered by VISION recommendations.

---

## 💰 Solana Crypto Wallet & DeFi Integration

> Full non-custodial wallet and trading experience embedded inside a social app.

- **Solana Wallet (Privy)** — Non-custodial embedded wallet with private key reveal, powered by Privy. Users get a wallet automatically on sign-up.
- **Token Swap with WebSocket Price Feed** — Real-time swap interface with live WebSocket-based price updates, quote previews, slippage controls, and transaction simulation.
- **SOL & SPL Token Transfers** — Send SOL and any SPL token with address validation, fee estimation, and transaction confirmation flow.
- **In-Feed Creator Tipping** — On-chain tipping where viewers tip creators directly while watching. Includes platform fee splits via custom Solana transaction instructions.
- **MoonPay Fiat Onramp** — Buy crypto with credit card/bank transfer directly inside the app via MoonPay SDK integration.
- **Real-Time Token Portfolio** — Portfolio view with animated price charts built with react-native-wagmi-charts and React Native Skia.
- **Filterable Transaction History** — Complete transaction history with date grouping, type filters, and transaction detail views.
- **Solana dApp Store** — Published and listed on the Solana Mobile dApp Store for Saga and Seeker devices.

---

## 🔐 Authentication & Security

- **Privy Auth** — Primary authentication with email OTP, Apple Sign-In, and WebAuthn passkey support.
- **Magic SDK → Privy Migration** — Engineered a seamless migration from Magic SDK to Privy without users losing access to their wallets or accounts.
- **Token Refresh Service** — Background JWT token refresh with exponential backoff retry logic and session persistence via `expo-secure-store`.
- **Secure Key Storage** — Private keys and sensitive tokens stored using `expo-secure-store` (Keychain on iOS, EncryptedSharedPreferences on Android).

---

## 💬 Real-Time Chat & Messaging

- **Stream Chat (GetStream) Integration** — Full-featured real-time chat with 1:1 DMs, group conversations, channels, user search, and typing indicators.
- **Deep Link Routing** — Chat deep links (`vala.fyi/chat/...`) that resolve to the correct channel or conversation from push notifications or shared links.
- **Push Notifications** — Firebase Cloud Messaging (FCM) on both iOS and Android with unread count badge sync and notification routing.

---

## 📊 React Native Performance Optimization

> Techniques used to keep a video-heavy social app smooth at 60fps on mid-range devices.

- **FlashList v2** — Replaced FlatList with @shopify/flash-list v2 for all lists. Proper `getItemType` for heterogeneous lists, `keyExtractor` for stable recycling, and memoized render item props.
- **Memory Pressure Hook** — Custom `useMemoryPressure` hook that monitors device memory warnings and proactively releases resources (video players, image caches).
- **Tab Memory Optimization** — Inactive tabs release their video players and cached data to reduce memory footprint. Resources re-acquire when the tab becomes active.
- **TTI Baseline Probes** — Built-in performance probes measuring Time To Interactive (~250ms baseline), active video player count, and mounted feed row count.
- **Bundle Size Monitoring** — Automated Hermes bytecode bundle size tracking (~17.9 MB Android production bundle). Size regressions are caught before merge.
- **React 19 + New Architecture** — Running on React Native's New Architecture (Fabric + TurboModules) with React 19 for improved rendering performance.

---

## 🚀 CI/CD & Deployment Pipeline

- **EAS Build (Multi-Environment)** — Expo Application Services builds for `staging`, `preview`, `production`, and `dapp-store-production` profiles.
- **OTA Updates (EAS Update)** — Over-the-air JavaScript bundle updates with channel-based deployment: `development` → `preview` → `production`.
- **Sentry Error Monitoring** — Full error tracking with source maps, breadcrumbs, custom performance transactions, and video playback metrics.
- **Automated Build + Submit Scripts** — One-command build and App Store / Google Play submission scripts for both platforms.
- **Solana dApp Store Deployment** — Separate APK build profile and on-chain publishing for the Solana Mobile dApp Store.

---

## 📐 Project Scale & Metrics

| Metric | Count |
|--------|-------|
| **Total Source Files** | 800+ |
| **API Modules** | 140 |
| **React Native Components** | 293 |
| **Custom React Hooks** | 20+ |
| **Zustand State Stores** | 21 |
| **App Screens / Routes** | 125 |
| **Utility Functions** | 55 |
| **Custom Expo Config Plugins** | 4 |
| **Native Patches** | 3 |

---

## 🛠️ Full Tech Stack

| Category | Technology |
|----------|-----------|
| **Mobile Framework** | React Native 0.79 (New Architecture) |
| **Development Platform** | Expo SDK 53 (Managed Workflow) |
| **Language** | TypeScript 5.8 (strict mode) |
| **UI Library** | React 19 |
| **Navigation** | Expo Router v5 (file-based, typed routes) |
| **State Management** | Zustand 5 + TanStack React Query 5 |
| **Video Playback** | expo-video with custom player pool |
| **List Virtualization** | @shopify/flash-list v2 |
| **2D Graphics & Charts** | React Native Skia + wagmi-charts |
| **Real-Time Chat** | Stream Chat SDK (GetStream) |
| **Authentication** | Privy + Apple Authentication + Passkeys |
| **Blockchain** | Solana Web3.js + SPL Token |
| **Fiat-to-Crypto** | MoonPay React Native SDK |
| **Push Notifications** | Firebase Cloud Messaging (FCM) |
| **Error Monitoring** | Sentry React Native |
| **CI/CD** | EAS Build + EAS Update + EAS Submit |
| **Form Handling** | React Hook Form + Zod validation |
| **Animations** | React Native Reanimated 3 + Lottie |
| **Design System** | Custom (Gilroy + Fira Sans Condensed) |
| **Package Manager** | pnpm 9 |

---

## 📸 App Screenshots

> Visit **[vala.social](https://www.vala.social/)** to see the app in action, or download it from the [App Store](https://apps.apple.com/us/app/vala/id6630372839) / [Google Play](https://play.google.com/store/apps/details?id=com.valalabsltd.vala).

<img width="190"  alt="feed" src="https://github.com/user-attachments/assets/ed59b254-8504-4b64-8773-d367e4506c82" />
<img width="190"  alt="discover" src="https://github.com/user-attachments/assets/660d95fe-bd23-445e-9f1e-603178d1bc94" />
<img width="190"  alt="wallet" src="https://github.com/user-attachments/assets/e742a82e-a92e-42bc-b912-7f252df120ef" />
<img width="190"  alt="chat" src="https://github.com/user-attachments/assets/78ba7ad5-4c9f-4652-bb38-e967b3259fa4" />
<img width="190"  alt="create" src="https://github.com/user-attachments/assets/13156786-9e13-4d03-a147-cb5749b8a959" />

---

## ❓ Frequently Asked Questions

<details>
<summary><strong>What tech stack is VALA built with?</strong></summary>

VALA is built with **React Native 0.79**, **Expo SDK 53**, **TypeScript 5.8**, and **React 19**. It uses the **New Architecture** (Fabric renderer + TurboModules) and runs on both iOS and Android from a single codebase. The crypto features use **Solana Web3.js** and the video player uses **expo-video** with a custom player pool.
</details>

<details>
<summary><strong>How does the TikTok-style video feed work in React Native?</strong></summary>

The vertical video feed uses a custom **Player Pool** architecture. Instead of creating a new video player for every video, a small pool of `expo-video` players is recycled as the user scrolls. Players bind to visible cells and unbind when they scroll off-screen. Combined with **HLS prefetch** and a **playback gate**, this achieves smooth 60fps scrolling with instant playback on swipe.
</details>

<details>
<summary><strong>How does VALA's AI recommendation engine work?</strong></summary>

VALA uses **VISION**, a multimodal AI engine that analyzes what's *inside* each video — visuals, audio, mood, energy, aesthetic. It builds a real-time taste profile for each user based on their engagement patterns, not just follower graphs. Users can also search by vibe using natural language queries like "cozy rainy day anime vibes".
</details>

<details>
<summary><strong>What blockchain does VALA use?</strong></summary>

VALA uses the **Solana** blockchain for its crypto features. Users get a non-custodial embedded wallet (powered by **Privy**) on sign-up. They can swap tokens, send SOL/SPL tokens, tip creators on-chain, and buy crypto with fiat via **MoonPay**. The app is also published on the **Solana Mobile dApp Store**.
</details>

<details>
<summary><strong>How is performance optimized for a video-heavy app?</strong></summary>

Key optimizations include: **FlashList v2** for list virtualization, a **player pool** that limits concurrent video instances, **memory pressure monitoring** that proactively releases resources, **tab-level memory optimization** that cleans up inactive tabs, **HLS segment prefetching**, and **Hermes bytecode** compilation. The app targets ~250ms TTI and maintains a ~17.9 MB production bundle.
</details>

<details>
<summary><strong>Can I hire you for React Native / mobile development?</strong></summary>

Absolutely! I'm open to freelance, contract, and full-time opportunities. Check the [Contact](#-contact--hire-me) section below for my LinkedIn, email, and other ways to reach me.
</details>

---

## 🏷️ Built With

`React Native` · `Expo` · `TypeScript` · `Solana` · `Web3` · `DeFi` · `TikTok Clone` · `Short-Form Video` · `AI Recommendations` · `expo-video` · `FlashList` · `Zustand` · `React Query` · `Stream Chat` · `Firebase` · `Sentry` · `MoonPay` · `Privy` · `React Native Reanimated` · `React Native Skia` · `Lottie` · `EAS Build` · `Expo Router` · `Hermes` · `New Architecture` · `React 19` · `iOS` · `Android` · `Mobile App` · `Cross-Platform`

---

## 📬 Contact & Hire Me

I'm a mobile engineer specializing in **React Native**, **Expo**, and **Web3 mobile apps**. I'd love to discuss this project, share technical insights, or explore opportunities.

<!-- 👇 REPLACE with your real links -->
| Channel | Link |
|---------|------|
| 🐦 **X / Twitter** | [@0xapp123](https://x.com/0xapp123) |
| 📧 **Email** | [apollo1030109@gmail.com](mailto:apollo1030109@gmail.com) |
| 🌐 **Portfolio** | [Portfolio](https://oura-kano.vercel.app) |
| 💻 **GitHub** | [@0xapp123](https://github.com/0xapp123) |

> ⭐ **If you found this interesting, please star this repo!** Stars help others discover this project.

---

## ⚠️ Disclaimer

This repository is a **portfolio showcase** of my work as a Lead Mobile Engineer at [VALA Labs Ltd](https://www.vala.social/). No proprietary source code, API keys, or sensitive business logic is included. All information presented is based on publicly available details about the [VALA](https://www.vala.social/) product.

