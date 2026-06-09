# EcoTherapy AI — Privacy Policy

**Effective date:** 8 June 2026
**App:** EcoTherapy AI (Android)
**Developer contact:** tengkuazn@gmail.com

This policy describes what data EcoTherapy AI collects, what it does with that data, and your rights as a user. It is written to satisfy Google Play's Data Safety disclosure and Malaysia's Personal Data Protection Act 2010 (PDPA).

EcoTherapy AI is positioned as a **wellness companion**, not a clinical instrument. It does not collect biometric sensor data or clinical records, and it does not access your camera, microphone, contacts, calendar, or media. The only sensor it ever uses is **coarse location**, and only on demand when you tap *Settings → "Detect my prayer zone"* — see §1.6 below for the full disclosure.

---

## 1. What we collect

| What | When |
| --- | --- |
| **Email address** (from Google Sign-in OR email/password sign-up) | When you sign in or create an account |
| **Password** (only if you sign up with email/password, never if you use Google Sign-in) | When you create an account — stored as a one-way hash by Supabase Auth; we never see the cleartext value |
| **Display name** (you type it at onboarding, optional) | When you complete onboarding |
| **General preferences** (language, age range, content preferences) | When you complete onboarding — all optional |
| **Mood entries** (a 1–5 score with an optional short note) | When you log a mood |
| **Journal entries** | When you write in the journal |
| **Chat messages** (what you type to the wellness companion) | When you use the chat feature |
| **A small set of personal notes the companion remembers about you** | Generated automatically over time so the companion can give consistent replies across sessions |
| **Bug or feedback reports** (your description plus the app version + device platform) | Only when you tap "Report a problem" |
| **Optional screenshot attached to a bug report** | Only the image you explicitly pick or capture when filing a report; never auto-captured |
| **Optional prayer check-ins** (which of the five daily prayers you tapped, and on what date) | Only when you have explicitly enabled **Settings → Daily prayer check-in** AND tapped the home-screen check-in card. **Default off.** No automatic logging, no inference from the clock, no comparison against your local prayer times. |

The prayer check-in feature is **opt-in by design** because prayer is a personal religious act, not a wellness metric. A check-in is a journal-style record that you chose to log; the app never measures, audits, or scores prayer performance. You can disable the feature at any time in Settings and previously-logged check-ins remain only on your account (delete-account removes them with everything else, per §4).

### 1.6 Coarse location — only when you tap "Detect my prayer zone"

Added in v1.0.3+14 (D-034 — JAKIM prayer-zone auto-detection).

Malaysia is divided into **59 prayer-time zones** published by JAKIM (Jabatan Kemajuan Islam Malaysia); the prayer times we display come from JAKIM's own e-Solat API and depend on which of these 59 zones you live in. To save you from picking your zone from a dropdown, the app offers a *Settings → "Detect my prayer zone"* shortcut that uses your phone's coarse location (the same ~3 km accuracy class Play Store uses for app-listing locales — much less precise than the fine GPS that mapping apps use).

What we do with it:

- **Coordinates stay on the device.** When you tap the button, the app asks Android for your approximate position (`ACCESS_COARSE_LOCATION`), runs a haversine-distance lookup against the 59 zone centroids inside the Flutter process, and identifies the closest zone code (e.g. `SGR01`). Your raw coordinates are **never** sent to our servers, **never** logged to the per-user wiki, **never** written to the activity timeline.
- **Only the resolved zone code is persisted.** A six-character administrative-area identifier like `SGR01` (covering ~7 districts in Selangor) is written to `user_profile.prayer_zone`. That's the only piece of information that leaves the device.
- **One-shot, on-demand.** The location read happens once per button tap. The app does **not** subscribe to location updates, does **not** run any background-location service, does **not** access location at startup, and does **not** access location for any feature other than prayer-zone resolution.
- **Opt-in and reversible.** If you decline the permission prompt the app falls back to the default zone (`WLY01` = Kuala Lumpur / Putrajaya) and remains fully functional. You can also pick a zone manually in Settings without ever granting location permission.

---

## 2. What we do not collect

There is no code path in this app that reads, stores, or transmits any of the following:

- Fine-grained location (`ACCESS_FINE_LOCATION`) or any continuous location tracking — only the one-shot coarse fix described in §1.6, and only when you explicitly tap the detect button
- Background location of any kind
- IP-based or cell-tower geolocation
- Camera, microphone, media files **other than** an image you explicitly attach to a bug report (and we never read your full photo library — only the single picture you hand over)
- Contacts, calendar, SMS, call logs
- Health and fitness sensors (heart rate, steps, sleep, etc.)
- Financial information (payment cards, bank accounts, transactions)
- Browser history or installed-app inventory
- Biometric identifiers
- Device advertising identifier or any cross-app tracking ID

Mood entries are **scores you choose to log yourself** — they are not derived from any sensor or background measurement.

Screenshots attached to a bug report are stored privately under your account, accessible only to you and the EcoTherapy AI team. Deleting your account also deletes any attached screenshots.

---

## 3. Who we share data with

We share the minimum data necessary with a small number of clearly-named third parties. **We do not sell your data to anyone, ever.** We do not run advertising or third-party analytics SDKs.

- **Google** — for Google Sign-in. Google sees only that you signed in; we receive only the email address you choose to share. If you sign up with email/password instead, Google is not involved in your authentication path.
- **Supabase Auth** — handles email/password sign-up + sign-in (when you choose that path), email confirmation emails, and password-reset emails. Passwords are hashed by Supabase Auth before storage; cleartext passwords never reach our application code.
- **A cloud database provider** — stores your account, mood entries, journal entries, chat history, and other app data in the Asia-Pacific region.
- **A self-hosted AI model** — runs on developer-controlled infrastructure and generates the replies to your chat messages. Before each message leaves our servers, we **replace your account identifier with a one-way cryptographic hash** so the message the model receives is not linked to your name, email, or any user-stable identifier. (An Anthropic Claude path remains in the app's code as a dormant fallback and is not used on the live service.)
- **Cloudflare** — provides the encrypted network path between our servers and the self-hosted AI model. Cloudflare's proxy checks an access token and streams the request straight through to the model without reading, logging, or storing your message. Your **account identifier is already replaced by a one-way hash** before the request reaches Cloudflare (your message text itself is carried so the AI can read it, but it is not linked to your name, email, or any user-stable identifier).
- **Crash diagnostics** — the app no longer integrates a third-party crash-reporting service (it was removed). If the app crashes, **Google Play's Android vitals** may collect anonymised crash and ANR diagnostics at the platform level under Google's own terms; this happens outside the app and contains no personal identifiers we control.

Each of these providers is bound by their own privacy policies and is contractually limited to processing data on our behalf.

---

## 4. Your rights

### See what we know about you
The **"Export My Data"** option (Settings → Data Management) lets you download everything stored under your account — your profile, mood entries, journal entries, chat history, and the notes the companion remembers about you. Nothing the app knows about you is withheld from this export.

### Correct what we know about you
Many of the notes the companion keeps are generated automatically. To correct or remove a specific piece of information, contact the developer at the email above. You can also delete your account (below), which erases everything the app holds about you.

### Delete everything
**Profile → "Delete Account"** is the right-to-erasure path under PDPA 2010. Tapping it (and typing `DELETE` to confirm) removes your account and all data tied to it in a single server-side operation. The deletion is irreversible.

### Export your data
**Settings → "Export My Data"** lets you download everything as a JSON archive or a CSV (mood-only).

### Withdraw consent
You can sign out, uninstall, or delete your account at any time. No notice, no waiting period, no cost.

---

## 5. Children

The app is **intended for adults 18 and over**. It is not designed for, marketed to, or appropriate for children. We do not knowingly collect data from anyone under 18.

---

## 6. Crisis and safety

EcoTherapy AI is a **wellness companion, not a clinical instrument**. If you are in crisis, please contact a professional helpline:

- **Befrienders KL:** 03-7627 2929 (24 hours)
- **Talian HEAL (KKM):** 15555 (daily, 8 am – midnight)
- **Talian Kasih (KPWKM):** 15999 (24 hours)
- **Malaysian Mental Health Association:** 03-2780 6803
- **Emergency:** 999

The app surfaces these helplines from a dedicated Crisis Resources page and may surface a helpline card alongside (not instead of) the companion's reply if your message contains distress-adjacent language.

---

## 7. Security

All requests between the app and our servers use TLS encryption. Your account data is isolated by per-user access controls on the server side so other users cannot read your records.

If you sign in with Google, we never see or store a password — sign-in is delegated to Google entirely. If you sign up with email/password (the alternative we restored in v1.0.57), Supabase Auth stores a one-way hash of your password, not the password itself; we cannot recover the cleartext, even on request.

---

## 8. Changes to this policy

If we change this policy in a way that materially affects what we collect or who we share it with, the change will be announced inside the app and the **effective date** at the top of this document will be updated. Continued use of the app after a material change constitutes acceptance of the new policy.

---

## 9. Contact

For any privacy question, data-subject access request, or complaint:

**Email:** tengkuazn@gmail.com

Under PDPA 2010 §43 you have the right to refer an unresolved complaint to the **Personal Data Protection Commissioner Malaysia** at https://www.pdp.gov.my.
