---
layout: default
title: TapLens Privacy Policy
---

# TapLens Privacy Policy

**Version covered:** 0.4.7 (version code 13) and later, until this page says otherwise
**Last updated:** [set to the publication date]

This policy describes the TapLens Android app from version 0.4.7 (version code 13): the release app (`app.taplens`) and its closed-test edition (`app.taplens.closedtest`). Earlier versions used different translation, subscription, and ad behavior, so this page does not describe them. The TapLens server configuration (translation providers, caches, and log retention) can change independently of the app; the server details below were last verified on September 27, 2026.

## Who we are

TapLens is developed and operated by **Hakan Ergin**, an individual developer based in Turkey. For questions about this policy or your data, contact **hakan.ergin@gmail.com**.

## What TapLens does

TapLens is a system-overlay translator. You tap a floating button while using another app; TapLens captures the current screen, recognizes text on it, translates that text, and draws the translation over the original app.

TapLens has two tiers:

- **Free** recognizes and translates text on your device with Google ML Kit. On-device translation has no daily quota. Free may show ads: a native ad on the Home screen and an ad when you return to the app, only after the consent step described in [Advertising and consent](#advertising-and-consent).
- **Premium** is a Google Play subscription (weekly, monthly, or yearly). It has no ads. For supported languages, Premium sends recognized text to the TapLens translation service for cloud translation, with a daily allowance of cloud requests; which languages use the cloud can differ between app versions. Other language pairs, and requests made when cloud translation is unavailable or the allowance is used up, are translated on your device instead.

Some features in this policy are switched on by server or app configuration: Premium subscriptions and purchase verification, cloud translation, and ads. They may not be active in every build or at every time. When a feature is off, the data flows this policy describes for it do not happen, with one exception: the app checks your consent status with Google User Messaging Platform on each start even while ads are off (see [Advertising and consent](#advertising-and-consent)).

## Screen capture and recognized text

Screen capture is the most sensitive operation TapLens performs:

- **Capture happens only when you act.** TapLens requests a screen capture when you tap its floating button or use its translation gestures while the translator is enabled. It does not record your screen in the background.
- **The captured image stays on the device.** TapLens passes it to on-device OCR: Google ML Kit for its supported scripts, or Tesseract with model files bundled in the app for 17 other source languages. TapLens does not upload the captured image and does not save it to storage.
- **On-device translation stays on the device.** Free translation, and Premium translation that runs on the device, uses downloaded ML Kit language packs. The recognized text and translation do not leave the device on this path.
- **Premium cloud translation uses the TapLens server.** Recognized text, the source and target language codes, your anonymous Firebase ID token, an App Check token, the app version, a random request key, and your IP address are sent over HTTPS to the TapLens server. The ID token and App Check token are used for authentication and abuse prevention; the IP address is used for rate limiting. The server sends the recognized text and language codes to one translation provider at a time, in a fixed fallback order: Groq (model `openai/gpt-oss-120b`), then Cerebras (`gpt-oss-120b`), then Google Gemini (`gemini-2.5-flash-lite`). The provider does not receive your Firebase UID.

The server uses Upstash managed Redis for a translated-result cache and for operational state. The cache key is a hash of the language pair and recognized text, so it does not contain the original text; the cached value is the translation only. The cache is not linked to your Firebase UID and expires after 30 days. Separately, the server keeps a retry record keyed by your anonymous UID and request key for 60 seconds so a retried request can safely reuse the same response.

Please avoid translating screens containing passwords, payment details, or other secrets. Premium cloud translation transmits recognized text as described above, even though TapLens never sends the captured image.

## ML Kit processing and metrics

For source languages routed to ML Kit OCR, Google ML Kit processes the captured image and recognized text on the device. For the other 17 source languages, Tesseract uses model files bundled with the app for on-device OCR; it does not download those models. ML Kit processes all on-device translation inputs and outputs. ML Kit may contact Google to download translation language packs, receive model updates, and check device compatibility. It also sends utilization and performance metrics to Google. Google's [ML Kit Terms & Privacy](https://developers.google.com/ml-kit/terms) and [ML Kit Android data-disclosure guidance](https://developers.google.com/ml-kit/android-data-disclosure) describe categories such as device and application information, device or other identifiers, latency and other performance metrics, API configuration, feature input and output size, feature version, event type, error codes, and for translation, configured source and target languages. Those published categories do not list the recognized screen text or captured image as a metric field. TapLens does not control the exact metric payload or its retention period.

## Subscriptions and purchase verification

Premium subscriptions are sold and billed by **Google Play**. Google processes the payment; we never receive your card or bank details.

When you subscribe, or when the app restores an existing subscription, the app sends the Google Play purchase token, the subscription product, and a random operation ID to the TapLens server, together with your anonymous Firebase ID token and an App Check token. The server checks the purchase with Google Play and stores a subscription record containing:

- a one-way hash (SHA-256) of the purchase token, not the token itself;
- your anonymous Firebase UID, so your install can use Premium;
- the product, subscription state (for example active, in grace period, canceled, expired, refunded, or revoked), and expiry time.

Google Play also sends the server real-time notifications when a subscription renews, is canceled, refunded, or otherwise changes, so the record stays current. These notifications travel through Google Cloud Pub/Sub.

To keep your daily cloud allowance attached to one subscription across renewals and plan changes, the server also keeps a subscription-lineage record that links purchase-token hashes to their original subscription. It contains hashes only, not purchase tokens or your UID.

## Identifiers and account data

TapLens has no user accounts. It uses:

- **An anonymous Firebase UID.** On first launch, Firebase Authentication creates a random identifier that is not linked to your name, email, or Google account. The server uses it to authenticate requests, link your install to a verified subscription, apply rate limits, and prevent abuse.
- **App Check and Play Integrity attestation.** Firebase App Check obtains a short-lived token to help verify that requests come from a genuine copy of the app on a genuine device.
- **An advertising ID.** When ads are enabled, the Google Mobile Ads SDK can start after the consent step and may read the Android advertising ID for ad serving and measurement. Ads are shown only in the Free tier. You can reset or delete this identifier in your Android settings.

Your selected source and target languages and display preferences are stored on your device. For Premium cloud translations, the language codes accompany the request so the server can produce the requested direction.

## Crash reports

The release app uses Google Firebase Crashlytics to receive crash reports when the app fails unexpectedly. Reports may include device model, operating system version, app version, and state associated with the crash. Crash reports do not intentionally include the text you translated or the captured image. Google's [Firebase privacy and security guidance](https://firebase.google.com/support/privacy) says Crashlytics keeps crash stack traces, extracted minidump data, and associated identifiers for 90 days before starting removal from live and backup systems. NDK minidump data is kept only while the crash is processed; the 90-day period is not a promise that every copy disappears exactly on that day.

## Advertising and consent

Ads appear only in the Free tier. TapLens may show a native ad on the Home screen and an app-open ad when you return to the app. The Premium tier, and the app before it knows your tier, does not show ads.

On each app start, TapLens checks your consent status with Google User Messaging Platform (UMP), which asks for consent where required. This check happens even while ads are off. The Google Mobile Ads SDK starts only when that step allows ads and an ad format is enabled. The consent check and the SDK start do not currently depend on your tier, so they can also happen in Premium, which still shows no ads. AdMob may receive the advertising ID, IP address, device information, and consent status to serve and measure ads. You can decline or withdraw consent.

If UMP reports that a privacy-options entry point is required, you can reopen it in the app at **Home → Privacy and cookie settings**. The row may not be shown when UMP does not require it. You can also reset or delete your advertising ID in Android settings.

See [Google's advertising policies](https://policies.google.com/technologies/ads).

## Analytics

The release app uses Google Firebase Analytics for app-usage events such as feature usage, error rates, timing, and selected source and target language codes. Analytics storage stays off until the consent step allows it, or until consent is determined not to be required. Events do not include screen images, recognized screen text, or translated text.

## Android permissions

- **Display over other apps** — to show the floating button and translation overlay.
- **Screen capture (MediaProjection)** — requested through the Android system dialog and used only as described above.
- **Notifications** — Android requires a persistent notification while the translator service is running; TapLens also uses notifications for app notices that you can disable.
- **Network access** — to download language packs, reach the TapLens server for Premium, verify subscriptions, and load ads in the Free tier.

## Third parties

TapLens shares data with the following processors only to provide, secure, and measure the app's functionality. We do not sell your data or share it with anyone else unless required by law.

- **Google Firebase** (Authentication, App Check, Crashlytics, Analytics, and Remote Config) receives the anonymous UID, attestation data, crash diagnostics, consent-gated analytics events, and configuration requests.
- **Google ML Kit** processes screen images for its OCR scripts and all on-device translations. It may receive language-pack requests and the utilization metrics described above. Tesseract processes the other 17 OCR source languages on the device.
- **Google AdMob and UMP** receive advertising and device information and consent status to show Free-tier ads and obtain consent where required.
- **Google Play** processes subscription purchases and, through the Google Play Developer API and real-time notifications (delivered with Google Cloud Pub/Sub), confirms subscription status to the TapLens server.
- **Google Cloud Run and Cloud Logging** host the TapLens server and receive hosting and request metadata, which can include the client IP address, route, status, and latency. Cloud Logging keeps these records for 30 days.
- **Upstash managed Redis** stores the translated-result cache, retry records, IP-based rate-limit state, daily-usage counters, subscription records, and subscription-lineage records described in this policy.
- **Sentry** receives server error and sampled performance diagnostics. Translation request bodies are reduced to language codes and counts before error events are sent, and Sentry's default personal-data collection is disabled; no Sentry retention period is promised here.
- **Groq, Cerebras, and Google Gemini**, through the TapLens server, receive the recognized text and language codes for Premium cloud requests routed to each provider. They do not receive your Firebase UID.

The server's request logs record counts, language codes, timing, status, cache and provider outcomes, and a hash of the request key. They do not record recognized text, translations, purchase tokens, or the authorization and App Check headers. This application-level redaction does not mean that every Cloud Run or Cloud Logging record strips all platform-managed request metadata.

### Provider privacy terms

Provider account settings (paid tiers, retention controls, and regions) were last verified on September 15, 2026 and can change. At that verification, the account state was a Groq Developer organization, a Cerebras account with purchased credits, and a Google Gemini API project on Paid Tier 1 with prepaid credits. These facts do not establish a provider's zero-data-retention setting, logging choice, training use, or storage region.

- **Groq.** Groq's [Your Data](https://console.groq.com/docs/your-data) page says inference requests are not retained by default, but temporary reliability or suspected-abuse logs may retain inputs and outputs for up to 30 days, unless law requires longer. Groq describes [Zero Data Retention (ZDR)](https://console.groq.com/docs/your-data) as a control that disables that retention for eligible customers, and its [Services Agreement](https://console.groq.com/docs/legal/services-agreement) says inputs and outputs are not used for training or fine-tuning unless the customer explicitly grants permission or instructs Groq. This policy does not represent ZDR as enabled for TapLens and does not guarantee zero retention.
- **Cerebras.** Cerebras's [retention explanation](https://support.cerebras.net/articles/1811589793-does-cerebras-retain-my-data) says it does not retain prompt content, API requests and responses, chat or transaction logs, or user input and model output, while retaining operational account and usage metrics. Its [privacy policy](https://www.cerebras.ai/privacy-policy) and [Terms of Use](https://www.cerebras.ai/terms-of-service) provide additional qualifications. These public statements do not establish every setting, route, or storage region of the TapLens account.
- **Google Gemini Developer API.** Google's [Gemini API Terms](https://ai.google.dev/gemini-api/terms) say paid services do not use prompts or responses to improve Google products, but may log them for prohibited-use detection, safety, security, and legal or regulatory disclosures. Such data may be transiently stored or cached in countries where Google or its agents maintain facilities. Google's [ZDR guidance](https://ai.google.dev/gemini-api/docs/zdr) describes an approved project control that sanitizes content before abuse-monitoring logs; it does not remove feature-specific storage. Google's [logs policy](https://ai.google.dev/gemini-api/docs/logs-policy) describes a configurable maximum retention for billing-enabled project logs. TapLens does not represent ZDR as approved or enabled and does not promise a particular Gemini retention period or region.

## Data retention

- **Translated-result cache:** expires after 30 days; hash-derived key, translation only, not linked to your Firebase UID.
- **Retry records:** 60 seconds, keyed by your anonymous UID and request key.
- **Daily cloud-usage counters:** linked to your subscription lineage, reset each UTC day, and deleted about 48 hours after that day ends. Per-request accounting records use hashes of your UID and request key and expire after 48 hours. IP-based rate-limit state follows the server's short operational windows. Aggregate service-level usage and cost counters, which contain no user identifiers, are kept for 90 days.
- **Subscription records:** kept while the subscription is active or recoverable, and for up to 400 days after it reaches a final state (such as expired, refunded, or revoked), so renewals, refunds, and restores can be handled correctly.
- **Subscription-lineage records:** kept without a fixed expiry so a subscription's daily allowance cannot be reset by replacing or renewing it. They contain purchase-token hashes only. You can ask us to delete them; see below.
- **Hosting logs:** Cloud Logging keeps request metadata for 30 days.
- **Provider data:** Premium text sent to a provider follows that provider's terms and account settings described above.
- **Sentry:** retention follows its service and account settings; TapLens does not promise a retention period.
- **Crashlytics:** removal starts after Google's published 90-day period; see [Firebase's privacy and security guidance](https://firebase.google.com/support/privacy).
- **On your device:** language settings, display preferences, and downloaded translation language packs remain until you delete them, clear app data, or uninstall TapLens. The bundled Tesseract OCR models ship inside the app and are removed when you uninstall it; clearing app data removes any extracted copies.

## Your choices and rights

- Manage or cancel your subscription in Google Play. Canceling stops renewal; it does not delete the server records described above.
- Where UMP requires it, reopen consent choices from **Home → Privacy and cookie settings**. You can decline or withdraw analytics and personalized-ad consent.
- Delete downloaded translation language packs from the Language Picker screen. Bundled OCR models are part of the app and cannot be deleted there.
- Reset or delete your advertising ID in Android settings.
- Uninstall the app or clear its app data to remove data stored on your device.
- Contact **hakan.ergin@gmail.com** to ask what data is associated with your anonymous UID or subscription, or to request deletion from TapLens records. Because the UID is anonymous, TapLens may be unable to link a request to you or fulfill it in every case; we will explain if that applies. Deleting subscription records can end Premium access on your install until the subscription is restored.

If you are in the EEA or UK, you may have rights under applicable data protection law, including access, rectification, erasure, restriction, and objection. You may also lodge a complaint with your local data protection authority.

## Age eligibility

TapLens is intended for adults aged 18 or older. It is not intended for people under 18.

## Changes to this policy

We may update this policy as TapLens changes. The last-updated date at the top will change when we do. Material changes will be reflected in the Play Store listing or otherwise communicated in the app where appropriate.

---

*This policy applies to the TapLens Android app (`app.taplens`, and its closed-test edition `app.taplens.closedtest`) from version 0.4.7 (version code 13).*
