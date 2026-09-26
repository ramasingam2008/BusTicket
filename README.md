# Tap Bus 🚌

**Contactless bus ticketing for Chennai, powered by Bluetooth Low Energy.**

Tap Bus lets a commuter buy a bus ticket just by having their phone nearby when a bus arrives — no NFC tap, no manual entry, no queue. The app listens for a BLE beacon broadcast by the bus, detects which route it is, lets the rider pick their drop stop, and pays instantly from an in-app wallet.

## Features

- **Automatic bus detection** — scans for BLE beacons named `BUS-<route>` and picks the closest one using a smoothed, multi-reading signal check (filters out noisy single readings so it doesn't misfire).
- **One-tap booking** — once a bus is detected, a bottom sheet shows the route, auto-fills the boarding stop, and lets the rider choose where they're getting off with live fare calculation.
- **In-app wallet** — pay fares instantly from a stored balance; top up via any UPI app (PhonePe, GPay, Paytm, etc.) or in demo mode with no real payment.
- **Payment & top-up animations** — a processing → success (animated checkmark) sequence gives clear visual feedback on every payment, matching the UX of major payment apps.
- **Digital ticket** — a receipt-style ticket screen (torn-paper visual, route, stops, fare, timestamp) that's screenshot-blocked to prevent ticket fraud.
- **Ticket history** — every past ticket is saved and can be reopened anytime from the Tickets tab to show a conductor, not just right after purchase.
- **Passes screen** — daily/weekly/monthly pass options (UI ready, purchase flow coming soon).

## Tech stack

- **Kotlin** + **Jetpack Compose** (Material 3) — 100% declarative UI, no XML layouts
- **Kotlin Coroutines** — async BLE scanning loop and timed animation sequencing
- **Android Bluetooth LE APIs** (`BluetoothManager`, `ScanCallback`) — beacon detection
- **UPI Intent-based payments** — standard Android intents hand off to any installed UPI app; no proprietary payment SDK, no vendor lock-in

All dependencies are open source (Apache 2.0 via AndroidX/Compose/Kotlin) — no third-party closed libraries are used.

## How it works

1. The app continuously scans for BLE devices named `BUS-<routeNumber>` (e.g. `BUS-77A`).
2. Signal strength (RSSI) is smoothed over several readings; once a bus reads consistently "close," a booking sheet appears automatically.
3. The rider selects their drop stop; fare is calculated from a per-route stop list.
4. Payment is deducted from the wallet, with a processing/success animation, and a ticket is issued.
5. The ticket is stored in history and can be reopened anytime to show during inspection.

## Running the project

1. Clone this repo.
2. Open in **Android Studio** (Giraffe or newer recommended).
3. Let Gradle sync.
4. Run on a physical device (BLE scanning requires real Bluetooth hardware — it won't work on the emulator).
5. Grant Bluetooth/location permissions when prompted.
6. To test without a real bus beacon, set `DEMO_MODE = true` in `MainActivity.kt` to skip real payments, or use another BLE device advertising as `BUS-<route>`.

## Configuration

Key constants in `MainActivity.kt`:

| Constant | Purpose |
|---|---|
| `DEMO_MODE` | Skips real UPI payment when `true` |
| `BLOCK_SCREENSHOTS` | Prevents screenshots on the ticket screen |
| `TAP_RSSI` | Signal strength threshold counted as "close" |
| `TAP_CONFIRM_READINGS` | Consecutive strong readings required before triggering a tap |
| `SHOP_UPI_ID` | UPI ID payments are sent to (replace before real use) |

## License

MIT — see [LICENSE](./LICENSE).

## Built for

SFD-2026 Mini Hackathon — Code. Collaborate. Create.
