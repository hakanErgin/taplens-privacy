# TapLens Privacy Policy

**Effective date:** July 4, 2026

TapLens ("the app") is developed and operated by **Hakan Ergin**, an individual developer based in Turkey ("we", "us"). This policy explains what data TapLens processes, why, and what your choices are.

**Contact:** hakan.ergin@gmail.com

## What TapLens does

TapLens is an Android screen translator. When you tap its floating button, it captures the current screen, recognizes the text on it, translates that text, and shows the translation as an overlay.

## Screen content and translations

This is the most sensitive data the app touches, so here is exactly what happens:

- **Screen capture happens only when you act.** A screenshot is taken only when you tap the TapLens floating button (or use its translate gestures) while the translator is enabled. TapLens does not record your screen in the background.
- **Screenshots never leave your device.** Text recognition (OCR) runs entirely on your device using Google ML Kit. The captured image is processed in memory and discarded.
- **Only recognized text is sent for translation.** The text extracted from the screen is sent over an encrypted connection (TLS) to our translation server, which forwards it to a third-party AI translation provider — currently one of **Cerebras**, **Groq**, **Google (Gemini)**, or **DeepSeek** — to produce the translation. These providers process the text in order to return a translation, under their own API terms and privacy policies, which you can review at their respective sites.
- **We do not store your text.** Our server does not persist the text you translate or the translations returned. Server logs are deliberately restricted to counts and technical identifiers — never the content of what you translate.
- **Translations are cached on your device only**, so repeated text does not need to be re-sent. Uninstalling the app deletes this cache.

**Please avoid translating screens containing passwords, payment details, or other secrets.** Although we do not store your text, it is transmitted for translation as described above.

## Identifiers and account data

TapLens has no user accounts. To enforce fair-use limits and prevent abuse, the app uses:

- **An anonymous user ID** (Firebase Anonymous Authentication) — a random identifier not linked to your name, email, or Google account.
- **App integrity tokens** (Firebase App Check / Play Integrity) — used to verify requests come from a genuine copy of the app.
- **Usage counters** — how many translations you have used today, stored on your device and on our server against your anonymous ID, to enforce the daily free quota and ad-based bonuses.

## Advertising (Google AdMob)

The free version of TapLens shows ads via **Google AdMob**, including optional rewarded ads you can choose to watch for bonus translations. The AdMob SDK may collect your device's **advertising ID**, IP address, and device information to serve and measure ads. Where required (e.g. in the EEA/UK), you will be shown a consent form (Google User Messaging Platform) and can choose whether ads may be personalized. You can change or withdraw your consent at any time from the app's settings ("Reset ad consent") and via your device's ad settings (reset/delete advertising ID).

See Google's policies: https://policies.google.com/technologies/ads

When you complete a rewarded ad, Google sends our server a signed confirmation containing your anonymous ID so we can credit the bonus translations. No screen content is involved in this.

## Analytics

Release versions of TapLens use **Google Firebase Analytics** to collect aggregated usage events (e.g. feature usage, error rates) that help us improve the app. Analytics events are held until the consent flow completes and are subject to the same consent choices as advertising. They never include screen content or translated text.

## Purchases

Pro subscriptions are handled by **Google Play Billing**. Google processes the payment; we never receive your card or bank details. We receive only what is needed to verify and honor your subscription.

## Android permissions TapLens uses

- **Display over other apps** — to show the floating button and translation overlay.
- **Screen capture (MediaProjection)** — requested each session via the Android system dialog; used only as described above.
- **Notifications** — Android requires a persistent notification while the translator service is running; also used for quota-related notices you can disable.

## Data sharing

We share data only with the processors named above (Google Firebase / AdMob / Play, and the translation providers Cerebras, Groq, Google Gemini, and DeepSeek), only to provide the app's functionality. We do **not** sell your data, and we do not share it with anyone else unless required by law.

## Data retention

- Screen text: not stored by us (processed transiently to return a translation).
- Anonymous ID and usage counters: kept while needed for quota enforcement and abuse prevention; daily counters reset every day.
- On-device data (settings, translation cache, counters): deleted when you clear the app's data or uninstall it.

## Your rights

If you are in the EEA/UK, you have rights under the GDPR — including access, rectification, erasure, restriction, and objection — regarding personal data we process (in practice, the anonymous identifiers and usage data described above). Contact us at the email above to exercise them. You also have the right to lodge a complaint with your local data protection authority.

Because TapLens uses an anonymous ID, we may be unable to link data to you unless you contact us from within the app context; some requests may therefore be technically impossible to fulfil, which we will explain if it applies.

## Children

TapLens is not directed at children under 13 (or the applicable age of digital consent in your country), and we do not knowingly collect data from them.

## Changes to this policy

We may update this policy as the app evolves. Material changes will be reflected here with an updated effective date, and where appropriate announced in the app.

---

*This policy applies to the TapLens Android app (package `app.taplens`).*
