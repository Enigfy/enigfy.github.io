# Privacy Policy

**Effective Date:** April 27, 2026 · Enigfy

## 1. Introduction

Monte Carlo ("the App") is a strategy game developed and published by Enigfy ("we," "our," or "us"). This Privacy Policy explains what information the App collects, how it is used, and your rights as a user.

We take your privacy seriously. Monte Carlo does not require an account, does not collect personal data, and does not use advertising or analytics SDKs. The App communicates with an Enigfy-operated game server only when you use the Versus multiplayer mode, and only to facilitate gameplay — never for tracking, profiling, or advertising.

This policy covers the three components of Monte Carlo: the iOS app (the "Alex" client), the game server operated by Enigfy, and the browser-based companion page (the "Bob" client) that an opponent uses to join a Versus match without installing anything. The server is a Ruby on Rails application hosted on Render.com in the United States, with a PostgreSQL database managed by Render. This policy is accessible within the App (via the Help screen) and at [www.enigfy.com](https://www.enigfy.com), as required by Apple's App Store Review Guidelines (Guideline 5.1).

## 2. Who We Are

- **Developer / Publisher:** Enigfy
- **Website:** [www.enigfy.com](https://www.enigfy.com)
- **Contact:** [info@enigfy.com](mailto:info@enigfy.com)

## 3. What Data the App Stores Locally

The following information is saved on your device only, using Apple's standard UserDefaults storage. It is never transmitted to Enigfy or any third party unless explicitly noted in section 4.

### 3.1 Game preferences

- Sound on/off
- Preferred starter player (human or computer)
- Casino mode haptic strength (standard or strong)
- Tutorial completion and "don't show again" flags

### 3.2 Game counters

Monte Carlo uses three local counters to manage free-trial limits and purchase gating. These are simple integer values stored on your device:

- **2-Star games played** (`advancedGamesUsed`) — Counts the number of Solo games completed in 2-Star (colour-hint) mode. After 10 free games, a paywall is shown offering the Pro upgrade. This counter is never sent to the server.
- **Versus sessions created** (`versusTrialCount`) — Counts the number of Versus multiplayer sessions you have created. After 6 free sessions, a paywall is shown offering the Versus subscription. Stored locally on your device and additionally synced to Apple's iCloud Key-Value Store so it survives app reinstalls and device transfers (see section 3.8). This counter is never sent to the Enigfy server.
- **Casino balance** (`cheatBalance`) — The number of Casino Pack games remaining. Stored locally on your device and additionally synced to Apple's iCloud Key-Value Store so it survives app reinstalls and device transfers (see section 3.8). Decremented by one on the first Casino activation per Versus session. This counter is never sent to the Enigfy server.

### 3.3 Player name

You may set a custom display name (default: "- -"). This name is sent to the server when you create a Versus session (see section 4.1) so that your opponent's web browser can display it instead of a generic label. The server stores it alongside the game session for the duration of the session. No real name is required — you can use any text or leave the default.

### 3.4 Purchase status

- **Pro unlock** — A boolean flag indicating whether the Monte Carlo Pro non-consumable has been purchased. Stored locally and verified against Apple's StoreKit entitlements on each app launch.
- **Versus subscription** — Whether the 3-month auto-renewable subscription is active, determined at runtime from Apple's `Transaction.currentEntitlements`. No subscription details are persisted locally beyond an active/inactive flag and the expiration date.
- **Casino Pack** — The remaining game balance is stored locally on your device via UserDefaults.

### 3.5 Statistics and win celebration state

- Total games played and total wins (Solo mode only — simple integer counters)
- Win celebration image deck order and position (determines which cat photo is shown next on a win — purely cosmetic)

### 3.6 Upgrade milestone tracking

A list of total-game milestones at which the Pro upgrade sheet has already been shown (to avoid showing it repeatedly at the same milestone). Contains only integer values.

### 3.7 What is NOT stored

Monte Carlo does not store or access any of the following:

- Your real name, email address, or any contact information
- Your location
- Device identifiers or advertising identifiers (IDFA)
- Health data, contacts, photos, camera, or microphone access
- Any information from other apps or websites
- Crash reports or usage analytics — no analytics SDK is included

### 3.8 iCloud Key-Value Store sync

The Casino balance and Versus trial count (sections 3.2) are synced to Apple's iCloud Key-Value Store using your Apple ID's iCloud account. This allows both values to survive app reinstalls and transfer between your devices signed into the same Apple ID. The synced values are two integers (the remaining Casino game count and the number of Versus sessions created). No other App data is synced to iCloud. This sync is handled entirely by Apple's iCloud infrastructure; the data is not sent to Enigfy's server. Apple's handling of iCloud data is governed by [Apple's Privacy Policy](https://www.apple.com/legal/privacy/).

## 4. Data Sent to and Stored by the Server

When you use Versus mode (two-player multiplayer), the App communicates with an Enigfy-operated game server over HTTPS and WebSocket (WSS). The server is a Ruby on Rails 8 API hosted on Render.com (Standard plan) in the United States, backed by a PostgreSQL database that Render manages. Apart from the lightweight server health check described in section 4.7 below, no server communication occurs during Solo mode.

### 4.1 Game session data

When you create a Versus session, the App sends:

- Your chosen display name (or an empty string if you use the default "- -")
- The App's version number (e.g. "1.0.0") — used by the server to enforce a minimum client version and reject outdated clients

The server returns:

- A randomly generated 8-character invite code (e.g. "ABCD1234")
- Two randomly generated authentication tokens (one for each player)
- The initial board state

For the lifetime of a session, the server stores a single database row containing only: the invite code, the four column heights, whose turn it is, the game status, the two authentication tokens, your display name, a count of live WebSocket subscribers, and Casino-mode flags. It stores no identifier linking this row to your Apple ID, your device, or any other Monte Carlo game you have played or will play in the future. Session rows are never aggregated, exported, or used for analytics.

### 4.2 Data relayed through the server during gameplay

The server acts as a relay between the two players. All gameplay data sent by the App is forwarded (broadcast) by the server to your opponent's web browser, and vice versa. The data exchanged during a game consists of:

- **Moves** — which column and how many pieces to remove, authenticated by your session token. The server validates each move and broadcasts the updated board state to both players.
- **Who goes first** — before the first move, you choose which player starts. This choice is sent to the server, which broadcasts it to your opponent.
- **Rematch** — after a game ends, you can start a new game on the same session. The rematch signal is sent to the server, which resets the board and broadcasts the fresh state to your opponent.
- **Casino mode toggle** — when you activate or deactivate Casino mode (see section 4.3), the toggle is sent to the server, which broadcasts it to your opponent's web client so it can show or hide the corresponding visual effect.

None of this data contains personal information — it is purely game state (board positions, turn order, visual effect flags) authenticated by randomly generated session tokens.

### 4.3 Casino mode

Casino mode is an optional visual enhancement for Versus games (animated column pedestals + haptic feedback showing the winning move).

- **Toggle relay**: When you activate or deactivate Casino mode, the App sends the toggle to the server, which broadcasts it to your opponent's web browser. This is the same relay pattern described in section 4.2.
- **Casino Pack purchases**: Casino Packs are consumable (20 games per pack). After purchase, the balance is credited locally on your device. No purchase data or balance information is sent to the server.

### 4.4 What the server does NOT receive

- No Apple ID, no email, no real name (unless you voluntarily enter your real name as your display name)
- No device identifier, no IDFA, no advertising identifier
- No analytics events, no usage telemetry, no crash reports
- No data from Solo mode — Solo games are entirely on-device

The Render platform that hosts the server produces standard web-server access logs which may include the IP address of incoming requests. These logs are used only for infrastructure operations, debugging, and abuse prevention. They are never joined to session rows, display names, or gameplay, and they are not used for analytics, profiling, advertising, or geolocation. Authentication tokens are redacted from the Rails application log by the framework's standard parameter filter.

### 4.5 Session lifecycle and retention

Game session data (board state, invite code, tokens, display name, Casino toggle state) exists on the server only for the duration of the game session. Sessions are not associated with any user account or persistent identifier. There is no user registration, no login, and no way to link sessions played at different times to the same person.

A scheduled cleanup job deletes every game session row older than 24 hours, regardless of whether the game finished, was abandoned, or was never started. After deletion the invite code, tokens, display name, and board state are no longer retrievable through any Enigfy-operated interface. The managed-PostgreSQL backups that Render retains for disaster recovery are the only residual copy and are used solely for infrastructure resilience — not for analytics, profiling, or any secondary purpose.

### 4.6 The companion web client ("Bob")

Your opponent joins Versus mode by scanning a QR code (or opening an invite link) which points to a single HTML page served directly by the Enigfy game server from the same Render.com host. This page — the Bob client — is self-contained: no third-party scripts, no fonts or images loaded from content-delivery networks, no cookies, no `localStorage` or `sessionStorage`, no analytics, no advertising code.

The invite code and a randomly generated session token are passed to the page through the URL's query string (for example, `?code=ABCD1234&token=…`). The page reads those values into memory, uses them to authenticate with the Enigfy server over HTTPS and WebSocket, and renders the live game state. Because the token appears in the URL, it may be retained in the opponent's browser history; however, it is valid only for that single session and becomes inert at most 24 hours later when the session row is deleted (section 4.5). Closing the tab ends the opponent's participation; nothing is written to the browser's persistent storage.

Bob's client communicates only with the Enigfy server — never with any third party. From the server's perspective, it is handled identically to the iOS app: only moves, session tokens, and WebSocket routing data are received, and all of the guarantees in sections 4.1–4.5 apply.

### 4.7 Server health check and web client pre-warm

Each time the App is launched or returns to the foreground, it sends a single lightweight HTTPS GET request to the server's health-check endpoint (`/up`). This request wakes the server from idle sleep (the hosting provider suspends idle instances) and triggers routine cleanup of stale game sessions. The request contains no authentication token, no user identifier, and no payload — it is an anonymous HTTP GET. The server responds with a simple 200 OK status.

When you open the Versus setup screen, the App also sends a single anonymous GET request to the server's root URL (`/`) to pre-warm the web page that your opponent will load when scanning the QR code. This request likewise contains no token, no identifier, and no payload.

Both requests are subject to the same standard Render platform access logs described in section 4.5 (IP address only, used for operations and abuse prevention, never joined to session data).

## 5. In-App Purchases

Monte Carlo offers three optional In-App Purchase products, all processed entirely by Apple through the App Store:

| Product | Type | What it unlocks |
|---------|------|-----------------|
| Monte Carlo Pro | One-time (non-consumable) | Unlimited 2-Star colour hints in Solo mode |
| Versus | 3-month auto-renewable subscription | Unlimited Versus multiplayer sessions beyond the free trial |
| Casino Pack | Consumable (20 games) | Casino mode visual effects in Versus games |

Enigfy does not receive, process, or store any payment card details or billing information. Apple handles all payment processing.

When you complete a purchase, Apple passes a verified transaction receipt to the App. The App reads this receipt to confirm the purchase status:

- **Pro**: stores a simple "unlocked: yes/no" flag on your device.
- **Versus**: checks Apple's `Transaction.currentEntitlements` at runtime — no subscription details are persisted beyond active/inactive and expiration date.
- **Casino Pack**: the balance is credited locally on your device. No transaction data is sent to the server.

No Apple ID information is retained by the App or the server.

For information on how Apple handles payment data, please refer to [Apple's Privacy Policy](https://www.apple.com/legal/privacy/).

## 6. Third-Party Services and SDKs

Monte Carlo does not integrate any third-party analytics, advertising, crash reporting, or social networking SDKs. No personal data is shared with or transmitted to any third party by the App.

The frameworks used are:

- **Apple StoreKit** — for In-App Purchase processing. Governed by Apple's privacy policy.
- **Apple URLSession** — for HTTPS and WebSocket communication with the Enigfy game server (Versus mode and health check).
- **Apple NSUbiquitousKeyValueStore (iCloud Key-Value Store)** — for syncing the Casino balance and Versus trial count across the user's devices (section 3.8). Governed by Apple's privacy policy.

No other third-party code is included. Monte Carlo does not use any AI or machine-learning services, and no user data is shared with any third-party AI system.

## 7. Data Collected by the App Store

When you download, install, or make a purchase within Monte Carlo, Apple may collect certain information as part of operating the App Store — for example, purchase history associated with your Apple ID, or crash logs submitted via Apple's opt-in diagnostics programme. This data is collected and controlled by Apple, not by Enigfy. Please refer to [Apple's Privacy Policy](https://www.apple.com/legal/privacy/) for details.

## 8. App Store Privacy Details (Nutrition Label)

Apple requires all apps to submit a Privacy Details declaration in App Store Connect, which appears on the App's product page. Monte Carlo's declaration reflects the contents of this policy:

- **Data Not Collected:** Monte Carlo does not collect any data linked to the user's identity as defined by Apple's App Privacy Details guidelines.
- **No Tracking:** The App does not track users across apps or websites owned by other companies.

If there is any discrepancy between the App Store privacy label and this policy, this policy controls and we will update the label promptly.

## 9. Children's Privacy

Monte Carlo does not knowingly collect any personal information from any user, including children. The App does not require account registration and collects no personal data.

The applicable minimum age for digital consent varies by jurisdiction — for example, 13 under the United States' COPPA, and 13–16 under the European Union's GDPR (depending on the member state). Regardless of age, no personal data is collected by this App, so no age-based consent is required. Parental controls and family-sharing settings are managed through Apple's Screen Time and Family Sharing features in iOS Settings.

## 10. Data Retention and Deletion

### 10.1 On-device data

All locally stored data (preferences, counters, statistics) lives exclusively on your device and is entirely under your control.

**On-device data is retained indefinitely until you delete it.** You can remove all App data at any time by:

- **Deleting the App** from your device (iOS Settings → General → iPhone Storage → Monte Carlo → Delete App). This immediately and permanently removes all locally stored preferences and game counters.
- **Resetting individual settings** within the App's Settings screen.

### 10.2 Server-side data

Game session data on the server is transient and not linked to any user account or persistent identifier. A scheduled cleanup job removes every session row older than 24 hours (section 4.4). Rails' standard parameter filter redacts authentication tokens from application logs; Render's platform-level access logs may contain IP addresses but are used only for operations and abuse prevention (section 4.3). Managed-PostgreSQL backups retained by Render for disaster recovery are the only residual copy and are not used for analytics, profiling, or any secondary purpose.

If you would like to delete an in-progress session before the 24-hour window expires, email [info@enigfy.com](mailto:info@enigfy.com) with the invite code and we will remove the row manually.

## 11. Consent and Your Privacy Rights

Monte Carlo does not collect personal data. No consent to data collection is sought or required. There is accordingly no consent to revoke.

Standard data-subject rights — including the right of access, rectification, portability, and erasure — are satisfied automatically: the only data that exists on your device is under your control via the deletion methods in section 10.1. Server-side rights are addressed in the server privacy policy.

If you have any questions or concerns about privacy, please contact us at [info@enigfy.com](mailto:info@enigfy.com). We will respond within 30 days.

## 12. International Users

Monte Carlo is available worldwide through the Apple App Store. The game server — and the web page served to Versus opponents — is hosted on Render.com in the United States, using Render's managed PostgreSQL service, also located in the United States. When you use Versus mode, game session data (board moves, display name, session tokens) is transmitted to this infrastructure. No personal data as defined by applicable privacy laws is transmitted — session tokens are randomly generated and not linked to any individual.

Users in the European Economic Area (EEA) and the United Kingdom are covered by this policy. The data transmitted during Versus gameplay (board state, randomly generated tokens) does not constitute personal data within the meaning of the EU General Data Protection Regulation (GDPR) or the UK GDPR, as it cannot be used to identify a natural person.

Users in Switzerland are similarly covered under the Swiss Federal Act on Data Protection (revDSG).

Users in California and other US jurisdictions with state privacy laws (including the California Consumer Privacy Act, CCPA) are equally covered. Monte Carlo does not collect, sell, share, or disclose personal information as defined by those laws.

## 13. Apple Privacy Manifest

In compliance with Apple's Privacy Manifest requirements (effective May 2024), Monte Carlo includes a `PrivacyInfo.xcprivacy` file that declares the App's use of Apple's UserDefaults API (category `NSPrivacyAccessedAPICategoryUserDefaults`, reason code `CA92.1` — data directly tied to app functionality). The App also uses Apple's iCloud Key-Value Store (`NSUbiquitousKeyValueStore`) to sync the Casino balance and Versus trial count (section 3.8); this is covered by the same iCloud entitlement. No personal data types are declared as collected because no personal data is collected.

## 14. Changes to This Privacy Policy

We may update this Privacy Policy from time to time. When we do, we will revise the "Effective Date" at the top of this page. For material changes, we will also update the App Store listing. We encourage you to review this policy periodically. Continued use of the App after any changes constitutes acceptance of the updated policy.

## 15. Contact Us

If you have any questions about this Privacy Policy or the App's data practices, please contact us:

- **Email:** [info@enigfy.com](mailto:info@enigfy.com)
- **Website:** [www.enigfy.com](https://www.enigfy.com)
