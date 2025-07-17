# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

React Native Firebase is a monorepo containing official React Native modules for Firebase services. The project uses:

- Lerna for monorepo management
- Yarn workspaces (Yarn 4.9.1)
- Jest for unit testing
- Detox for end-to-end testing
- TypeScript for type definitions

## Architecture

### Monorepo Structure

- `/packages/*` - Individual Firebase service modules (analytics, auth, firestore, etc.)
- `/tests` - End-to-end test application
- Each package has:
  - `/lib` - JavaScript implementation
  - `/android` - Android native code
  - `/ios` - iOS native code
  - `/e2e` - End-to-end tests
  - `/__tests__` - Unit tests

### Key Principles

- All implementations must mirror the official Firebase JS SDK
- Platform-specific methods must be annotated with `@platform`
- Native method names must match JS method names

## Common Development Commands

### Build & Setup

```bash
# Install dependencies (run from root)
yarn
yarn lerna:prepare

# Build all packages
yarn build:all:build

# Clean build artifacts
yarn build:all:clean
yarn lerna:clean
```

### Linting & Type Checking

```bash
# Run all linting
yarn lint:all

# JavaScript/TypeScript linting
yarn lint:js

# Android linting (requires google-java-format)
yarn lint:android

# iOS linting (requires clang-format)
yarn lint:ios:check
yarn lint:ios:fix

# TypeScript compilation check
yarn tsc:compile
```

### Testing

#### Unit Tests (Jest)

```bash
yarn tests:jest                 # Run once
yarn tests:jest-watch          # Watch mode
yarn tests:jest-coverage       # With coverage
```

#### End-to-End Tests (Detox)

```bash
# Android
yarn tests:android:build       # Build test app
yarn tests:packager:jet-reset-cache  # Start packager
yarn tests:emulator:start      # Start Firebase emulator
yarn tests:android:test        # Run tests

# iOS
yarn tests:ios:pod:install     # Install CocoaPods
yarn tests:ios:build          # Build test app
yarn tests:packager:jet-reset-cache  # Start packager
yarn tests:emulator:start     # Start Firebase emulator
yarn tests:ios:test           # Run tests
```

### Running a Single Test

To run a specific test file during development:

```bash
# For a specific e2e test
cd tests && yarn detox test --configuration [platform] path/to/test.e2e.js

# For Jest tests
yarn jest path/to/test.test.ts
```

## Package-Specific Development

When working on a specific package:

1. Make changes in `/packages/[module-name]/lib`
2. Update TypeScript definitions in `index.d.ts`
3. Update native code in `/android/src` and `/ios`
4. Add/update tests in `/__tests__` and `/e2e`
5. Run `yarn prepare` in the package directory to build

## Important Configuration Files

- `firebase.json` - Firebase project configuration (searched up to 3 directories from project root)
- `lerna.json` - Monorepo configuration
- `tsconfig.json` - TypeScript configuration
- `eslint.config.mjs` - ESLint configuration

## Code Style Requirements

- Use ESLint configuration (includes Prettier)
- Android: google-java-format
- iOS: clang-format with Google style
- Follow existing patterns in the codebase
- TypeScript strict mode is enabled

## Release Process

The project uses conventional commits with automatic versioning:

- feat: New features
- fix: Bug fixes
- perf: Performance improvements
- chore: Maintenance (hidden in changelog)
- test: Tests (hidden in changelog)
- docs: Documentation (hidden in changelog)

## Debugging Tips

- Check `firebase-debug.log` for Firebase-related issues
- Use `--verbose` flag with Detox for detailed test output
- Android logs: `yarn tests:android:test:cover`
- iOS logs: `yarn tests:ios:test-cover`

## Platform-Specific Notes

### iOS

- Requires Xcode 16.4+
- Install Apple Sim Utils: `brew install wix/brew/applesimutils`
- CocoaPods must be installed

### Android

- Gradle configuration in `/android/build.gradle`
- Firebase configuration auto-discovered from `firebase.json`

### Windows Development

- Use `yarn tests:android:build:windows` for Android builds
- Use `yarn tests:emulator:start:windows` for emulator

