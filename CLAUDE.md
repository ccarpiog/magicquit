# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

MagicQuit is a macOS menu bar utility (Swift/SwiftUI) that automatically terminates applications after a user-configurable idle period (1–72 hours, default 8). It runs as a status-bar-only app (`LSUIElement = YES` — no Dock icon).

## Build & Run

```bash
# Build (Release)
xcodebuild -scheme MagicQuit -configuration Release build

# Build (Debug)
xcodebuild -scheme MagicQuit -configuration Debug build

# Run tests
xcodebuild -scheme MagicQuit -destination 'platform=macOS' test

# Open in Xcode
open MagicQuit.xcodeproj
```

Deployment target: macOS 13.3. Swift 5.0. Single external dependency: [LaunchAtLogin-Modern](https://github.com/sindresorhus/LaunchAtLogin-Modern) (Swift Package).

## Architecture

Almost all logic lives in two files:

- **MagicQuitApp.swift** — `@main` entry point; creates the `MenuBarExtra` with icon and instantiates `RunningAppsManager`.
- **ContentView.swift** — contains everything else:
  - `RunningAppsManager` (ObservableObject) — central state: tracks running apps via `NSWorkspace` notifications, runs a 1-second timer to check idle times, terminates apps past threshold. Persists per-app toggle state as JSON in `UserDefaults`.
  - `ContentView` — main menu bar popup listing running apps.
  - `AppRow` — per-app row: icon, name, countdown, toggle, reset/close buttons.
  - `SettingsWindowController` — singleton managing the settings `NSWindow`.
  - `SettingsView` — idle hours slider, launch-at-login toggle, show/hide close button.

### Key design details

- **Blocked apps:** `isBlockedApp()` filters system processes (Finder, Dock, Spotlight, etc.) and background-only apps (`activationPolicy != .regular`).
- **Persistence:** `@AppStorage` for scalar settings; manual JSON encode/decode in `UserDefaults` for the per-app `toggleStatus` dictionary.
- **App termination:** Uses `NSRunningApplication.terminate()`. The app needs the `NSAppleEventsUsageDescription` entitlement to close other apps.
- **Countdown display:** Shows bold text when an app is < 1 hour from termination.
