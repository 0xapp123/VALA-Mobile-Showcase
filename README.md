# 📱 VALA — Mobile App (React Native / Expo)

> **Role:** Lead Mobile Engineer  
> **Company:** [VALA Labs Ltd](https://www.vala.social/)  
> **Platforms:** iOS & Android  
> **Stack:** React Native 0.79 · Expo SDK 53 · TypeScript · Solana · Stream Chat

[![VALA Social](https://img.shields.io/badge/🌐-vala.social-black?style=for-the-badge)](https://www.vala.social/)
[![VALA Finance](https://img.shields.io/badge/💰-vala.finance-black?style=for-the-badge)](https://vala.finance/)
[![Download](https://img.shields.io/badge/📲-Download_App-green?style=for-the-badge)](https://www.vala.social/)

---

## 🧠 What Is VALA?

**VALA** is a short-form video social app with integrated crypto finance — think **TikTok meets DeFi**.

- **[vala.social](https://www.vala.social/)** — Vibe-first content discovery powered by **VISION**, a multimodal AI recommendation engine that understands mood, energy, and aesthetics — not just views and keywords.
- **[vala.finance](https://vala.finance/)** — Built-in Solana wallet with token swaps, tipping, and fiat onramp so creators can earn directly inside the app.

Users scroll a feed that "gets" them, discover content across 30+ languages, earn Sparks (in-app rewards) for engagement, and trade crypto — all without leaving the app.

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

## 🎯 Key Contributions & Technical Highlights

### 🎬 Custom TikTok-Style Video Player System
> The most complex subsystem — a production-grade vertical video feed.

- **Player Pool Architecture (`PlayerPoolV2`)** — Recycling pool that manages `expo-video` player instances to minimize memory usage. Only a few players exist at any time; they're bound/unbound as cells enter/leave the viewport.
- **Playback Gate** — Controls when videos are allowed to play based on visibility, user interaction, and app state (foreground/background).
- **HLS Prefetch** — Pre-fetches upcoming video segments for instant playback on swipe.
- **Background Upload** — Users can record/select a video and navigate away while it uploads via native background upload task.
- **TTFF (Time To First Frame) Tracking** — End-to-end performance pipeline: player creation → status ready → first frame rendered → play called. Logged to Sentry in production.
- **Stall Detection** — Monitors loading stalls (>3s) and playback freezes, auto-reports to Sentry.
- **Conditional Logging** — ~70-80% log reduction in production. Critical metrics (TTFF, stalls, errors) always ship; verbose lifecycle logs are dev-only.
- **Watch Event Tracking** — Analytics for watch duration, completion rate, and engagement signals.

### 🔮 VISION AI Integration
> Multimodal recommendation engine powering the "vibe-first" experience.

- **Vibe Search** — Natural language search like *"cozy rainy day anime vibes"* that queries VISION's multimodal embeddings.
- **Vibe Sections** — Feed sections organized by mood/energy (hype, calm, nostalgic, chaotic).
- **Trending Velocity** — Real-time trending algorithm factoring in engagement acceleration, not just raw counts.
- **Comment Quality Analysis** — AI-powered comment scoring to surface meaningful conversations.
- **Auto-Translation** — Comments translated across 30+ languages via VISION API.
- **Discover Feed** — CMS-driven discover page with dynamic sections powered by VISION recommendations.

### 💰 Crypto / DeFi Features (Solana)
> Full wallet and trading experience embedded inside a social app.

- **Solana Wallet** — Non-custodial wallet with private key reveal, powered by Privy embedded wallets.
- **Token Swap** — Real-time swap interface with WebSocket-based price feeds, quote previews, and slippage controls.
- **Send Transactions** — SOL and SPL token transfers with address validation and fee estimation.
- **Tip Distribution** — On-chain tipping system where viewers tip creators directly in-feed. Includes platform fee splits.
- **MoonPay Integration** — Fiat-to-crypto onramp for users who don't have existing wallets.
- **Token Portfolio** — Real-time portfolio view with animated price charts (react-native-wagmi-charts + Skia).
- **Transaction History** — Filterable transaction history with grouped-by-date display.
- **Solana dApp Store** — Published to Solana Mobile dApp Store for Saga/Seeker devices.

### 🔐 Authentication System
- **Privy Auth** — Primary auth with email OTP, Apple Sign-In, and passkey support.
- **Magic SDK Migration** — Built a seamless migration flow from Magic SDK to Privy without users losing their wallets.
- **Token Refresh Service** — Background token refresh with retry logic and session persistence via `expo-secure-store`.

### 💬 Real-Time Chat
- **Stream Chat (GetStream)** — Full-featured chat with channels, DMs, group conversations, and user search.
- **Deep Linking** — Chat deep links that resolve to the correct channel/conversation.
- **Push Notifications** — Firebase Cloud Messaging with unread badge sync.

### 📊 Performance Engineering
- **FlashList v2** — Optimized vertical and nested horizontal lists with proper `getItemType`, `keyExtractor`, and memoized props.
- **Memory Pressure Management** — Custom `useMemoryPressure` hook that monitors and responds to device memory warnings.
- **Tab Memory Optimization** — Releases resources (video players, cached data) from inactive tabs to reduce memory footprint.
- **Baseline Probes** — Built-in performance probes measuring TTI (~250ms), active player count, and mounted row count.
- **Bundle Monitoring** — Automated bundle size tracking (~17.9 MB Android Hermes bytecode).

### 🚀 CI/CD & DevOps
- **EAS Build Pipeline** — Multi-environment builds: `staging`, `preview`, `production`, `dapp-store-production`.
- **OTA Updates** — EAS Update with channel-based deployment (development → preview → production).
- **Sentry Integration** — Error tracking with source maps, breadcrumbs, and custom performance monitoring.
- **Automated Build Scripts** — Shell scripts for build + submit workflows across iOS and Android.

---

## 📐 Project Scale

| Metric | Count |
|--------|-------|
| **Source Files** | 800+ |
| **API Modules** | 140 |
| **UI Components** | 293 |
| **Custom Hooks** | 20+ |
| **State Stores** | 21 (Zustand) |
| **Screens/Routes** | 125 |
| **Utility Functions** | 55 |
| **Custom Expo Plugins** | 4 |
| **Native Patches** | 3 |

---

## 🛠️ Tech Stack

| Category | Technology |
|----------|-----------|
| **Framework** | React Native 0.79 + Expo SDK 53 (New Architecture) |
| **Language** | TypeScript (strict mode) |
| **Navigation** | Expo Router (file-based, typed routes) |
| **State** | Zustand 5 + TanStack React Query 5 |
| **Video** | expo-video with custom player pool |
| **Lists** | @shopify/flash-list v2 |
| **Graphics** | React Native Skia (charts, animations) |
| **Chat** | Stream Chat (GetStream) |
| **Auth** | Privy + Apple Authentication |
| **Blockchain** | Solana Web3.js + SPL Token |
| **Fiat Onramp** | MoonPay SDK |
| **Push** | Firebase Cloud Messaging |
| **Monitoring** | Sentry |
| **CI/CD** | EAS Build + EAS Update |
| **Forms** | React Hook Form + Zod |
| **Animations** | React Native Reanimated 3 + Lottie |
| **Styling** | Custom design system (Gilroy + Fira Sans Condensed) |
| **React** | React 19 |

---

## 📸 The App

> Visit **[vala.social](https://www.vala.social/)** to see the app in action, or download it from the App Store / Google Play.

<!-- Add screenshots here if you have permission -->
<!-- ![Feed](./screenshots/feed.png) -->
<!-- ![Discover](./screenshots/discover.png) -->
<!-- ![Wallet](./screenshots/wallet.png) -->

---

## 📬 Get In Touch

I'd love to chat about this project, my approach to mobile engineering, or potential opportunities.

<!-- 👇 Replace with YOUR actual links -->
- 🐦 **X / Twitter:** [@0xapp123](https://x.com/0xapp123)
- 📧 **Email:** apollo1030109@gmail.com
- 🌐 **Portfolio:** [Portfolio](https://oura-kano.vercel.app)
- 💻 **GitHub:** [@0xapp123](https://github.com/0xapp123)

---

## ⚠️ Disclaimer

This repository is a **portfolio showcase** of my work as a Lead Mobile Engineer at VALA Labs Ltd. No proprietary source code, API keys, or sensitive business logic is included. All information presented is based on publicly available details about the [VALA](https://www.vala.social/) product.


