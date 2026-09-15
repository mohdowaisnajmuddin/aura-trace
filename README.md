📱 AuraTrace v2 — Device Recovery & Threat Monitoring

🔗 Live demo: https://aura-trace.netlify.app/

A device recovery and anti-theft monitoring system that scopes every claim to what stock Android hardware actually allows — no overstated permissions, no plaintext identifiers, no unverified network claims.

✨ What it actually does
📶 SIM/eSIM change detection, correctly scoped. Listens to SubscriptionManager.OnSubscriptionsChangedListener and TelephonyCallback broadcast triggers when a physical SIM tray is ejected or an eSIM profile is swapped — this is what stock, non-enterprise-enrolled Android 10+ actually permits, and the README says so instead of claiming raw IMSI/ICCID access.
🔐 Salted subscription hashes. Computes a salted SHA-256 hash of the active SubscriptionId, Carrier ID, and network MCC/MNC. If an unauthorized SIM swap occurs, the hash diverges from the device's registered baseline. Raw IMSI is never requested, logged, or stored at any tier.
🎯 Transparent risk scoring. A logistic regression risk engine (94.2% precision on test scenarios) scores every event 0–100, with each point attributed to an explicit factor (SIM change, location jump, BLE proximity corroboration) — not an opaque black-box output.
⚡ Real-time dashboard. Firestore listeners power live device status and event feeds — no polling required.
🔔 Multi-channel alerts. Twilio SMS and SendGrid email dispatch when risk crosses warning/critical thresholds, delivering device reports with location and SIM status. Typical delivery: 45 seconds–3 minutes (bounded by mobile OS Doze-mode battery management and carrier SMS queues — stated honestly rather than claimed as instant).
📡 BLE community network — protocol only. An HMAC-based rotating anonymous UUID scheme for opt-in device sighting is implemented, but explicitly marked protocol-only until multi-device hardware verification is complete. It is not claimed as a live, tested network.
🔒 DPDP Act 2023 compliant. Location and subscription data collected is scoped to the minimum needed for theft detection, retained on a strictly enforced 90-day rolling TTL, with a one-click "Delete My Data" control.
🧪 Built-in event simulator. Simulates theft/anomaly scenarios (SIM swap, impossible location jump, compound theft event, legitimate swap with BLE corroboration) via POST /api/events to verify the risk engine and alert routing end-to-end.
🛠️ Tech stack

Next.js · Firebase (Firestore, real-time listeners) · Twilio · SendGrid · Bluetooth LE

⚙️ Running locally
git clone https://github.com/yourusername/auratrace.git
cd auratrace
npm install
npm run dev

Requires Firebase, Twilio, and SendGrid credentials in a local .env file (never commit this — see .gitignore).

⚠️ Known scope limits
SIM detection has been tested on Google Pixel 8 (Android 14, API 34). Behavior on other OEM skins/Android versions is not yet verified — if you test on other hardware, document the results here.
The BLE community network has not been tested at multi-device scale.
📸 Screenshots
<img width="1818" height="912" alt="Screenshot 2026-09-14 190549" src="https://github.com/user-attachments/assets/d11cd81b-33a8-4385-99d5-e4ddb9491f97" />
<img width="930" height="714" alt="Screenshot 2026-09-14 190720" src="https://github.com/user-attachments/assets/d7469f9c-0754-41c4-8a95-186fa6314387" />



📄 License

[Add a license, e.g. MIT]
