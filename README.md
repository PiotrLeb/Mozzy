# Mozzy

Mozzy is an app for personal trainers that brings session scheduling, client records (weight, height, progress), and messaging into one place.

## Features

- Sign up, email verification, and login
- Daily/weekly session schedule with confirmed / new / pending status
- Client roster with search and filters (Active, New, Pending, Paused)
- Per-client profile: height, weight, a weight-progress chart, and payment status
- One-tap payment reminders
- In-app messaging per client
- Editable trainer profile

## Tech Stack

- React Native

## Getting Started

### Prerequisites

- Node.js >= 18
- npm or yarn
- Watchman (macOS only)
- Xcode (for iOS) / Android Studio (for Android)
- A configured React Native environment — see the
  [official setup guide](https://reactnative.dev/docs/environment-setup) if
  you haven't run a bare RN project before

### Installation

```bash
git clone <repo-url>
cd mozzy
npm install
```

### Run on iOS

```bash
npx pod-install ios
npx react-native run-ios
```

### Run on Android

```bash
npx react-native run-android
```

> Using Expo instead of the bare workflow? Run `npx expo start` and skip the
> platform-specific commands above.

## Project Structure

```
src/
├── app/                    # app initialization, routing, providers
├── features/
│   └── <feature-name>/
│       ├── components/     # UI components for this feature
│       ├── hooks/          # hooks specific to this feature
│       ├── services/       # business logic, API calls
│       ├── types/          # types and interfaces
│       ├── utils/          # helpers used only by this feature
│       └── index.ts        # public API of the module
├── shared/
│   ├── components/         # shared UI components
│   ├── hooks/              # shared hooks
│   ├── utils/              # shared helper functions
│   ├── types/              # shared types
│   └── constants/          # shared constants
├── config/                 # configuration, environment variables
└── main.ts
```

## Scripts

| Command | Description |
|---|---|
| `npm start` | Start the Metro bundler |
| `npm run android` | Build and run on Android |
| `npm run ios` | Build and run on iOS |
| `npm test` | Run the test suite |
| `npm run lint` | Run the linter |
