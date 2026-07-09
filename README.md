<div align="center">

# 🌱 LifeLayer

### Build habits. Track progress.

An all-in-one productivity & fitness app for iOS — routines, workouts, nutrition, and a social layer, backed by a security-first cloud architecture.

![Expo](https://img.shields.io/badge/Expo-SDK%2054-000020?logo=expo&logoColor=white)
![React Native](https://img.shields.io/badge/React%20Native-0.81-61DAFB?logo=react&logoColor=black)
![Supabase](https://img.shields.io/badge/Supabase-Postgres%20%7C%20Auth%20%7C%20RLS-3ECF8E?logo=supabase&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-gpt--4o--mini-412991?logo=openai&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-iOS-lightgrey?logo=apple)

</div>

---

## 📱 Demo

<!--
  ADD YOUR DEMO HERE. Best options:
  1) Record a 30–60s screen recording on your iPhone, then either:
       • drag the .mp4 straight into this README on github.com (GitHub hosts it inline), or
       • upload to YouTube (Unlisted) and paste the link below, or
       • convert to a .gif and put it in screenshots/ and reference it here.
-->

> ▶️ **[Watch the demo video](#)** &nbsp;·&nbsp; *(replace this link with your YouTube/Loom URL)*

<p align="center">
  <img src="screenshots/dashboard.png"  width="24%" />
  <img src="screenshots/routine.png"    width="24%" />
  <img src="screenshots/workout.png"    width="24%" />
  <img src="screenshots/nutrition.png"  width="24%" />
</p>

---

## Overview

LifeLayer brings a person's whole daily system into one place: their **morning/night routines**, their **gym program and workout logging**, their **nutrition**, and a lightweight **social layer** to stay accountable with friends. It's a production-grade React Native app with a fully-featured Supabase backend — auth, row-level security, storage, database triggers, and serverless edge functions — plus AI features powered by OpenAI.

I built it end-to-end: mobile UI, data modeling, backend security policies, the AI/moderation pipeline, and the release/CI setup.

---

## ✨ Features

### 🔁 Routines
- **Timed, step-by-step runner** with per-task time goals, sub-steps, and a checklist mode
- **Quick-check** — tick tasks off without starting the timer; the calendar records the exact time each was completed
- **Drag-to-reorder**, **rename**, and per-day **scheduling** (with an option to auto-shift later routines when a task's duration changes)
- **Streaks**, completion history, and a calendar day view

### 💪 Fitness
- **1,300+ exercise library** with animated GIF demos, target/secondary muscles, and instructions
- **Interactive muscle-map** (SVG) that highlights primary vs. secondary muscles worked
- **Gym splits** (PPL, Arnold, Upper/Lower, custom) and per-muscle-group workout plans
- Workout logging with a weekly calendar and progress photos (stored privately on-device)

### 🥗 Nutrition
- Daily meal tracking with **macros** (calories / protein / carbs / fat)
- **Food search** (USDA database) and **barcode scanning**
- Saved meals and reusable meal sections; goal-based macro targets from onboarding

### 👥 Social
- Add friends by **friend code**; view a friend's activity on a **calendar** (routines, workouts, deep-work, meals)
- **Copy** a friend's saved routine or workout into your own
- Granular **privacy controls** — show full data to *everyone*, *friends only*, or *no one*
- A community **Explore** feed with reporting/moderation

### 🤖 AI
- **AI routine coach** — generates and critiques routines with gpt-4o-mini
- **AI skin & hair analysis** — snap a selfie for a personalized skincare/haircare routine (vision model)
- Every AI call is proxied server-side (keys never touch the device)

### ⏱️ Productivity
- Deep-work focus sessions with notes
- **Phone Downtime** — one tap triggers an iOS Focus via the Shortcuts app
- Habit tracking, weekly goals, journal, and a schedule/calendar

---

## 🖼️ Screenshots

| Dashboard | Routine runner | Workout + muscle map | Nutrition |
|---|---|---|---|
| ![](screenshots/dashboard.png) | ![](screenshots/routine.png) | ![](screenshots/workout.png) | ![](screenshots/nutrition.png) |

| Friend profile | Explore feed | AI coach | Onboarding |
|---|---|---|---|
| ![](screenshots/friend.png) | ![](screenshots/explore.png) | ![](screenshots/ai.png) | ![](screenshots/onboarding.png) |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **App** | React Native 0.81, React 19, Expo SDK 54, Expo Router (file-based navigation) |
| **UI/UX** | Reanimated, Gesture Handler, react-native-svg (custom muscle map), draggable lists, light/dark theming |
| **Backend** | Supabase — Postgres, Auth, Storage, Row-Level Security, triggers, `SECURITY DEFINER` functions |
| **Serverless** | Supabase Edge Functions (Deno) as a secrets proxy |
| **AI** | OpenAI gpt-4o-mini (text + vision), Moderation API |
| **Auth** | Email/password with OTP email confirmation, Google Sign-In, Apple Sign-In, OTP password reset |
| **Native** | Secure Store (session tokens), Camera (barcode), Image Picker, Notifications |
| **Release** | EAS Build + **EAS Update** (over-the-air JS updates) |

---

## 🏗️ Architecture & Engineering Highlights

The parts I'm most proud of are the ones you *don't* see on screen:

- **Secrets never reach the client.** The OpenAI key and the Supabase service-role key live only inside a Supabase **Edge Function** that the app calls with the user's JWT. Only the public anon key is bundled into the app — so decompiling the binary reveals nothing sensitive.

- **Row-Level Security everywhere.** Every table enforces per-user isolation at the database level. Friend and profile-visibility reads are gated by composable `SECURITY DEFINER` helpers (`is_friend()`, `can_view_data()`), so a user can only ever read data they're actually allowed to — enforced by Postgres, not by trusting the client.

- **Defense-in-depth content moderation.** User-generated content passes a client-side blocklist → the OpenAI Moderation API → a context-aware gpt-4o-mini check → and finally a **database trigger** that re-validates on insert. The trigger also **binds author identity** from the real profile server-side, so posts can't be spoofed or impersonated.

- **Rate limiting on both sides.** Client-side counters for snappy UX, backed by hard server-side limits (RLS insert policies + per-user daily caps in the edge function) so limits can't be bypassed by hitting the API directly.

- **Ship without the App Store.** EAS Update pushes JavaScript changes over-the-air, so day-to-day iteration doesn't require a new build or review.

---

## 🔒 About the source code

The full source lives in a **private repository**. This showcase intentionally omits it to protect API keys, the security-policy design, and the backend configuration. **Happy to walk through the code or grant read access on request** during an interview.

---

<div align="center">

*Built with React Native, Expo, and Supabase.*

</div>
