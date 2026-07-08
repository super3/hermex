<div align="center">

<img src="docs/assets/readme/hermex-icon.png" alt="Hermex app icon" width="96" />

# Hermex

**Control your self-hosted [Hermes](https://github.com/nesquena/hermes-webui) agent from your phone.**

Your server. Your phone. No middleman.

[![iOS 18+](https://img.shields.io/badge/iOS-18%2B-000000?logo=apple&logoColor=white)](https://apps.apple.com/app/hermex/id6767006319)
[![Swift](https://img.shields.io/badge/Swift-5.9%2B-F05138?logo=swift&logoColor=white)](https://swift.org)
[![Android: in development](https://img.shields.io/badge/Android-in%20development-3DDC84?logo=android&logoColor=white)](docs/ANDROID_PORT_PLAN.md)
[![Kotlin](https://img.shields.io/badge/Kotlin-2.0-7F52FF?logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg)](LICENSE)
[![Follow on X](https://img.shields.io/badge/Follow-%40uzairansar-000000?logo=x&logoColor=white)](https://x.com/uzairansar)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-support-FFDD00?logo=buymeacoffee&logoColor=black)](https://buymeacoffee.com/callmeuzi)

<a href="https://apps.apple.com/app/hermex/id6767006319">
  <img src="https://toolbox.marketingtools.apple.com/api/v2/badges/download-on-the-app-store/black/en-us" alt="Download on the App Store" height="50" />
</a>

[Website](https://hermexapp.com) · [App Store](https://apps.apple.com/app/hermex/id6767006319) · [Report a bug](https://github.com/uzairansaruzi/hermex/issues) · [Contributing](CONTRIBUTING.md)

<img src="docs/assets/readme/hero-devices.png" alt="Hermex running on two iPhones: a streaming chat session and the home screen with Tasks, Skills, Memory, Insights, and Sessions" width="720" />

</div>

Hermex is a set of native mobile clients for driving a self-hosted [hermes-webui](https://github.com/nesquena/hermes-webui) server — a mobile cockpit for an AI agent that lives on a machine **you** control. The phone is the control plane, not the compute plane: the agent, its tools, and your data stay on your own hardware.

This repository is a monorepo with two clients:

- **iPhone** (`ios/`) — a native SwiftUI app for iOS 18+, shipping on the [App Store](https://apps.apple.com/app/hermex/id6767006319).
- **Android** (`android/`) — a native Kotlin + Jetpack Compose app targeting the same feature set. It is in active development in this repo and not yet published to Google Play; build it from source (see below).

Both clients share one philosophy:

- **Free.** No subscriptions, no in-app purchases.
- **Private.** No analytics, no tracking, no third-party relay — the app talks only to your server.
- **Native.** Real SwiftUI on iOS and real Jetpack Compose on Android — not a web wrapper, and no cross-platform UI runtime.

## Features

The iPhone app is the reference client; the Android app targets parity with it.

- **Chat with your agent** — send messages with model, reasoning-effort, workspace, and profile options; attach files and images; watch responses stream in real time with thinking and tool-call detail.
- **Steer or stop a run** mid-flight.
- **Sessions** — browse, search, and resume every conversation on your server; cached sessions stay readable offline.
- **Pick your models** — switch between any model or provider your server is configured for, with recents and favorites.
- **Profiles & projects** — switch agent profiles and organize sessions into projects.
- **Tasks** — view and edit your agent's scheduled cron jobs from your phone.
- **Skills** — browse and search the agent's installed skills.
- **Workspace browser** — explore your server's file system from the app.
- **Memory & Insights** — read-only panels for agent memory and usage analytics.

<div align="center">
<table>
  <tr>
    <td align="center"><img src="docs/assets/readme/screenshot-chat.png" alt="Streaming chat with code blocks and markdown tables" width="240" /><br /><sub><b>Stream responses in real time</b></sub></td>
    <td align="center"><img src="docs/assets/readme/screenshot-tasks.png" alt="Tasks screen listing scheduled cron jobs" width="240" /><br /><sub><b>Manage scheduled tasks</b></sub></td>
    <td align="center"><img src="docs/assets/readme/screenshot-skills.png" alt="Skills screen with searchable agent skills" width="240" /><br /><sub><b>Browse agent skills</b></sub></td>
  </tr>
</table>

Screenshots show the iPhone app; the Android app mirrors the same design. More at [hermexapp.com](https://hermexapp.com).
</div>

## Getting started

Hermex is a client only — it does not ship with, host, or provision a backend. You bring your own [hermes-webui](https://github.com/nesquena/hermes-webui) server (a third-party, MIT-licensed open-source project) running on a machine you control. Setup takes about 15 minutes:

1. **Run the server.** Install and start `hermes-webui` on macOS, Linux, or Windows/WSL2 (Python 3.11+). Set `HERMES_WEBUI_PASSWORD`.
2. **Make it reachable from your phone** (see options below).
3. **Connect.** On iPhone, [download Hermex](https://apps.apple.com/app/hermex/id6767006319) from the App Store; on Android, build the app from source (see [Building from source](#building-from-source)). Enter your server URL (e.g. `https://hermes.yourdomain.com`) and password, and you're in.

Self-hosting the server, securing it, and keeping it reachable are your responsibility.

### Making the server reachable

- **HTTPS via a tunnel or reverse proxy (recommended).** Expose the server through Cloudflare Tunnel or any reverse proxy that terminates real TLS at a hostname you own. Real HTTPS keeps both platforms' transport-security policies (iOS App Transport Security, Android Network Security Config) happy with no exceptions. On a publicly reachable hostname the password is your only app-level defense — set a strong one.
- **Tailscale.** Run the server bound to all interfaces with a password, install Tailscale on both the server and your phone, and connect to `http://<tailnet-ip>:8787`. Both apps allow plain HTTP only for Tailscale's `100.64.0.0/10` device range.
- **Local testing** can use `http://localhost:8787` on the iOS simulator (when the server runs on the same Mac), or `http://10.0.2.2:8787` from the Android emulator to reach the host machine.

### Troubleshooting the connection

If connection testing fails, check these first:

1. The machine hosting `hermes-webui` is awake.
2. `hermes-webui` is running and serving `/health` (`curl https://<your-server>/health`).
3. The tunnel, reverse proxy, or Tailscale route is connected.
4. The server URL and password are correct.

## Building from source

### iOS

Prefer the [App Store build](https://apps.apple.com/app/hermex/id6767006319) unless you're developing. To build yourself you need Xcode 26 or newer (iOS 18 SDK) and an iPhone or simulator on iOS 18+.

Clone the repo, open `ios/HermesMobile.xcodeproj`, and run the `HermesMobile` scheme on an iPhone simulator (the Xcode target is `HermesMobile`; the app's display name is `Hermex`). Dependencies are resolved automatically via Swift Package Manager.

From the command line:

```zsh
xcodebuild -project ios/HermesMobile.xcodeproj -scheme HermesMobile -destination 'platform=iOS Simulator,name=iPhone 17' build
```

```zsh
xcodebuild test -project ios/HermesMobile.xcodeproj -scheme HermesMobile -destination 'platform=iOS Simulator,name=iPhone 17'
```

If that simulator is not installed, list available devices and choose a nearby iPhone simulator:

```zsh
xcrun simctl list devices available
```

Local validation defaults for XcodeBuildMCP users live in `.xcodebuildmcp/config.yaml`; the standard post-change flow is in [`DEVELOPMENT.md`](ios/DEVELOPMENT.md).

### Android

The Android app lives in `android/` and is built with the Gradle wrapper. You need JDK 17+ and the Android SDK (API 26+; the app's `minSdk` is 26).

```zsh
cd android && ./gradlew build          # assemble + unit tests + lint
```

```zsh
cd android && ./gradlew installDebug   # install on a connected device or emulator
```

Or open the `android/` directory in Android Studio. The locked dependency list lives in `android/gradle/libs.versions.toml`, and the Android working agreement is in [`android/AGENTS.md`](android/AGENTS.md). The port roadmap and status are tracked in [`docs/ANDROID_PORT_PLAN.md`](docs/ANDROID_PORT_PLAN.md).

## Server compatibility

The apps are developed and tested against the `hermes-webui` commit pinned in [`UPSTREAM_TESTED_SHA`](UPSTREAM_TESTED_SHA). Upstream does not yet guarantee API stability (its README declares version skew unsupported pending their stable-API work), so newer or older server versions may break individual features — please include your server version in bug reports. Both apps decode tolerantly (unknown fields never crash them) and endpoint shapes are verified against upstream source, never invented; see [`CONTRACT_TESTS.md`](CONTRACT_TESTS.md) for the contract-testing approach.

## Documentation map

- [`PROJECT_SPEC.md`](PROJECT_SPEC.md): source of truth for product scope, API behavior, dependencies, and architecture decisions.
- [`PROJECT_INTENT.md`](PROJECT_INTENT.md): short orientation; useful for product tradeoffs, not implementation details.
- [`AGENTS.md`](AGENTS.md): the platform-neutral working agreement; [`ios/AGENTS.md`](ios/AGENTS.md) and [`android/AGENTS.md`](android/AGENTS.md) add the per-platform rules.
- [`docs/ANDROID_PORT_PLAN.md`](docs/ANDROID_PORT_PLAN.md): the Android port plan, tech-stack mapping, and phased build status.
- [`DEVELOPMENT.md`](ios/DEVELOPMENT.md): iOS local development workflow, server setup notes, and the maintainer release runbook.
- [`TESTFLIGHT.md`](ios/TESTFLIGHT.md): maintainer-only TestFlight/App Store Connect operations.
- [`CONTRACT_TESTS.md`](CONTRACT_TESTS.md): upstream contract-test readiness and the pin-advance policy.
- [`SECURITY.md`](SECURITY.md): how to report a vulnerability.
- [`docs/agents/`](docs/agents): repo-local agent workflow conventions (issues, triage labels, domain notes).
- [GitHub Issues](https://github.com/uzairansaruzi/hermex/issues): source of truth for active bugs, polish notes, and feature requests.

## Contributing

Contributions are welcome — see [`CONTRIBUTING.md`](CONTRIBUTING.md) for how to pick up work and open a PR, [`AGENTS.md`](AGENTS.md) for the working agreement coding agents follow in this repo, and the [Code of Conduct](CODE_OF_CONDUCT.md). The short version:

- Do not invent API endpoints or JSON shapes; verify against the upstream server source or a running server.
- Every decoded model (Swift `Codable`, Kotlin serialization) decodes tolerantly — never crash on unknown fields.
- Add no third-party dependencies beyond each platform's locked list (`PROJECT_SPEC.md` for iOS, `android/gradle/libs.versions.toml` for Android) without explicit approval.
- Do not modify the upstream `hermes-webui` server from this repo.

## Support the project

Hermex is free and built in the open. If it's useful to you:

- ⭐ **Star this repo** — it helps others find the project.
- 🐦 **Follow [@uzairansar on X](https://x.com/uzairansar)** for updates and dev logs.
- ☕ **[Buy me a coffee](https://buymeacoffee.com/callmeuzi)** to support development.

<a href="https://buymeacoffee.com/callmeuzi"><img src="https://img.shields.io/badge/Buy%20Me%20a%20Coffee-callmeuzi-FFDD00?style=for-the-badge&logo=buymeacoffee&logoColor=black" alt="Buy Me a Coffee" height="40" /></a>

## License

MIT — see [LICENSE](LICENSE).

Hermex is an independent client and is not affiliated with the upstream [hermes-webui](https://github.com/nesquena/hermes-webui) project. Apple, the Apple logo, and App Store are trademarks of Apple Inc. Android and Google Play are trademarks of Google LLC.
