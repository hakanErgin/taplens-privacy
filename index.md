# TapLens Privacy Policy

**Version covered:** 0.4.0 closed test
**Last updated:** September 8, 2026

This policy describes the TapLens Android app as configured for version 0.4.0 during its closed test. Earlier versions may have used different translation and provider behavior, so this page does not describe those versions.

## Who we are

TapLens is developed and operated by **Hakan Ergin**, an individual developer based in Turkey. For questions about this policy or your data, contact **hakan.ergin@gmail.com**.

## What TapLens does

TapLens is a system-overlay translator. You tap a floating button while using another app; TapLens captures the current screen, recognizes text on it, translates that text, and draws the translation over the original app.

TapLens has two tiers:

- **Free** translates on the device using Google ML Kit for the 18 directions where English is the source or target, across English, Spanish, French, German, Italian, Portuguese, Vietnamese, Japanese, Korean, and Chinese. The Free tier shows Google's test ad banner on the Home and Language Picker screens.
- **Premium** translates online through the TapLens proxy for 90 language directions, supports automatic source-language detection, and has no ads.

## Screen capture and recognized text

Screen capture is the most sensitive operation TapLens performs:

- **Capture happens only when you act.** TapLens requests a screen capture when you tap its floating button or use its translation gestures while the translator is enabled. It does not record your screen in the background.
- **The captured image stays on the device.** Google ML Kit recognizes text on the device. The captured image is processed in memory and discarded after recognition; it is not uploaded by TapLens.
- **Free translation stays on the device.** Recognized text is translated with the downloaded ML Kit language model. The recognized text and translation do not leave the device for this translation path.
- **Premium translation uses the TapLens proxy.** Recognized text, the source and target language codes, your anonymous UID, your client IP address, and the App Check token are sent over HTTPS to the proxy. The UID and token are used for authentication and abuse prevention; the IP address is used for rate limiting. For the 0.4.0 closed test, the proxy's configured fallback chain is Groq's 120B model, Cerebras's 120B model, and Google's Gemini 2.5 Flash-Lite. A request is sent to the provider that handles that step of the chain. DeepSeek is not part of this version's configured route.

The proxy uses Upstash managed Redis for this cache and for operational state. Its translated-result cache uses a hash-derived key so identical phrases can be reused; the key does not contain the original request text, the cache is not linked to your Firebase UID, and it is configured to expire after 30 days. Separately, the proxy stores a short-lived response/idempotency record keyed by your anonymous UID and request key so a retry can safely replay a response; that record is distinct from the 30-day result cache. Provider logging and retention are separate from both records; see [Third parties](#third-parties).

Please avoid translating screens containing passwords, payment details, or other secrets. Premium recognized text is transmitted for translation as described above, even though TapLens does not intentionally send the captured image.

## ML Kit processing and metrics

Google ML Kit processes the captured image, recognized text, and on-device translation inputs and outputs on the device. ML Kit may contact Google to download language packs, receive model updates, and check device compatibility. It also sends utilization and performance metrics to Google. Google's [ML Kit Terms & Privacy](https://developers.google.com/ml-kit/terms) and [ML Kit Android data-disclosure guidance](https://developers.google.com/ml-kit/android-data-disclosure) describe categories such as device and application information, device or other identifiers, latency and other performance metrics, API configuration, feature input and output size, feature version, event type, error codes, and for translation, configured source and target languages. Those published categories do not list the recognized screen text or captured image as a metric field. TapLens does not control the exact metric payload or its retention period.

## Identifiers and account data

TapLens has no user accounts. It uses:

- **An anonymous Firebase UID.** On first launch, Firebase Authentication creates a random identifier that is not linked to your name, email, or Google account. The proxy uses it to enforce a per-install daily translation limit and prevent abuse.
- **App Check and Play Integrity attestation.** Firebase App Check obtains a short-lived token to help verify that requests come from a genuine copy of the app on a genuine device.
- **An advertising ID.** Google AdMob may read the Android advertising ID for ad serving and measurement. You can reset or opt out of this identifier in your Android settings.

Your selected source and target languages and render-mode preference are stored locally. For Premium translations, the language codes accompany the request so the proxy can produce the requested direction.

## Crash reports

The release build uses Google Firebase Crashlytics to receive crash reports when the app fails unexpectedly. Reports may include device model, operating system version, app version, and state associated with the crash. Crash reports do not intentionally include the text you translated or the captured image. Google's [Firebase privacy and security guidance](https://firebase.google.com/support/privacy) says Crashlytics keeps crash stack traces, extracted minidump data, and associated identifiers for 90 days before starting removal from live and backup systems. NDK minidump data is kept only while the crash is processed; the 90-day period is not a promise that every copy disappears exactly on that day.

## Advertising and consent

The Free tier shows Google's test AdMob banner on the Home and Language Picker screens. AdMob may receive the advertising ID, IP address, device information, and consent status to serve and measure ads. Where required, Google User Messaging Platform (UMP) presents a consent form before analytics or personalized-ad data is sent. You can decline or withdraw consent.

If UMP reports that a privacy-options entry point is required, you can reopen it in the app at **Home → Privacy and cookie settings**. The row may not be shown when UMP does not require a privacy-options entry point. You can also reset or opt out of your advertising ID in Android settings.

See [Google's advertising policies](https://policies.google.com/technologies/ads).

## Analytics

Release versions use Google Firebase Analytics for app-usage events such as feature usage, error rates, timing, and selected source and target language codes. Analytics events are sent only after the UMP consent flow when consent is required, or when consent is not required. They do not include screen images, recognized screen text, or translated text.

## Purchases

Pro subscriptions are handled by **Google Play Billing**. Google processes the payment; we never receive your card or bank details. We receive only what is needed to verify and honor your subscription.

## Android permissions

- **Display over other apps** — to show the floating button and translation overlay.
- **Screen capture (MediaProjection)** — requested through the Android system dialog and used only as described above.
- **Notifications** — Android requires a persistent notification while the translator service is running; TapLens also uses notifications for app and quota-related notices that you can disable.

## Third parties

TapLens shares data with the following processors only to provide, secure, and measure the app's functionality. We do not sell your data or share it with anyone else unless required by law.

- **Google Firebase** (Authentication, App Check, Crashlytics, and Analytics) receives the anonymous UID, attestation data, crash diagnostics, and consent-gated analytics events.
- **Google ML Kit** processes screen images, recognized text, and on-device translations locally. It may receive language-pack requests and the utilization metrics described above; TapLens does not upload the captured image or recognized text to ML Kit for translation.
- **Google AdMob and UMP** receive advertising and device information and consent status to serve the test banner and obtain consent where required.
- **Google Play Billing** receives and returns purchase information. TapLens does not receive payment details.
- **Google Cloud Run and Cloud Logging** host the TapLens proxy and receive hosting and request metadata, which can include the client IP address, route, status, latency, and operational log records. The current Cloud Logging `_Default` bucket is configured to retain its records for 30 days.
- **Upstash managed Redis** receives the Premium translated-result hash cache, anonymous-UID-linked quota and short-lived retry/idempotency records, and IP-based rate-limit state.
- **Sentry** receives backend error and sampled performance diagnostics. Translation request bodies are reduced to language codes and counts before error events are sent; no Sentry retention period is promised here.
- **The TapLens proxy** receives recognized text, source and target language codes, your anonymous UID, client IP address, and App Check token for Premium requests. It may retain the translated result in the hash-derived cache described above.
- **Groq, Cerebras, and Google Gemini**, through the TapLens proxy, receive the recognized text and language codes for the Premium request routed to each provider. TapLens does not send these providers your Firebase UID.

The proxy's application logs use structured request summaries. They redact the authorization and App Check headers and the raw request blocks and response results, while recording validation outcomes, languages, counts, status, timing, and provider outcome for operations. This application-level redaction does not mean that every Cloud Run or Cloud Logging record strips all provider-managed request metadata.

For this closed test, the current account state is a Groq Developer organization, a Cerebras account with purchased credits, and a Google Gemini API project on Paid Tier 1 with prepaid credits. These billing facts do not by themselves establish a provider's zero-data-retention setting, logging choice, training use, or storage region.

### Provider privacy terms

- **Groq.** Groq's [Your Data](https://console.groq.com/docs/your-data) page says inference requests are not retained by default, but temporary reliability or suspected-abuse logs may retain inputs and outputs for up to 30 days, unless law requires longer. Groq describes [Zero Data Retention (ZDR)](https://console.groq.com/docs/your-data) as a control that disables that reliability and abuse retention for eligible customers, and its [Services Agreement](https://console.groq.com/docs/legal/services-agreement) says inputs and outputs are not used for training or fine-tuning unless the customer explicitly grants permission or instructs Groq. This policy does not represent ZDR as enabled for TapLens and does not guarantee zero retention.
- **Cerebras.** Cerebras's [retention explanation](https://support.cerebras.net/articles/1811589793-does-cerebras-retain-my-data) says it does not retain prompt content, API requests and responses, chat or transaction logs, or user input and model output, while retaining operational account and usage metrics. Its [privacy policy](https://www.cerebras.ai/privacy-policy) and [Terms of Use](https://www.cerebras.ai/terms-of-service) provide additional qualifications. These public statements do not establish every setting, route, or storage region of the TapLens account.
- **Google Gemini Developer API.** The current Gemini project uses Google's paid service with active billing and prepaid credits. Google's [Gemini API Terms](https://ai.google.dev/gemini-api/terms) say paid services do not use prompts or responses to improve Google products, but may log them for prohibited-use detection, safety, security, and legal or regulatory disclosures. Such data may be transiently stored or cached in countries where Google or its agents maintain facilities. Google's [ZDR guidance](https://ai.google.dev/gemini-api/docs/zdr) describes an approved project control that sanitizes content before abuse-monitoring logs; it does not remove feature-specific storage. Google's [logs policy](https://ai.google.dev/gemini-api/docs/logs-policy) describes a configurable maximum retention for billing-enabled project logs. TapLens does not represent ZDR as approved or enabled and does not promise a particular Gemini retention period or region.

## Data retention

- The TapLens proxy's translated-result cache is configured to expire after 30 days. Its hash-derived cache key does not contain the original request text, and the cache is not linked to your Firebase UID.
- Separately, Upstash stores a short-lived response/idempotency record keyed by the anonymous UID and request key for retry safety. This record is distinct from the 30-day translated-result cache; quota and IP-based rate-limit state are also stored there under the proxy's operational TTLs.
- Premium source text is sent to the provider selected by the proxy. Provider retention follows the terms and account settings described above; TapLens does not establish one provider-retention period for every request.
- The anonymous UID and usage counters are kept while needed for per-install rate limiting and abuse prevention. Daily usage counters reset each day.
- Cloud Logging's current `_Default` bucket is configured for 30-day retention. This is hosting-log retention and does not establish a retention period for provider, Upstash, or Sentry data.
- Sentry retention follows its service and account settings; TapLens does not promise a retention period for Sentry diagnostics.
- Crashlytics starts removal of crash stack traces, extracted minidump data, and associated identifiers after its published 90-day period; see [Firebase's privacy and security guidance](https://firebase.google.com/support/privacy).
- Language settings, render preferences, and downloaded language packs remain on your device until you delete them, clear app data, or uninstall TapLens.

## Your choices and rights

- Where UMP requires it, reopen consent choices from **Home → Privacy and cookie settings**. You can decline or withdraw analytics and personalized-ad consent.
- Delete downloaded language packs from the Language Picker screen.
- Reset or opt out of your advertising ID in Android settings.
- Uninstall the app or clear its app data to remove data stored on your device.
- Contact **hakan.ergin@gmail.com** to ask what data is associated with your anonymous UID or to request deletion from TapLens records. Because the UID is anonymous, TapLens may be unable to link a request to you or fulfill it in every case; we will explain if that applies.

If you are in the EEA or UK, you may have rights under applicable data protection law, including access, rectification, erasure, restriction, and objection. You may also lodge a complaint with your local data protection authority.

## Age eligibility

The TapLens 0.4.0 closed test is restricted to adults aged 18 or older. It is not intended for people under 18.

## Changes to this policy

We may update this policy as TapLens changes. The last-updated date at the top will change when we do. Material changes will be reflected in the Play Store listing or otherwise communicated in the app where appropriate.

---

*This policy applies to the TapLens Android app (package `app.taplens`) and describes version 0.4.0's closed-test configuration.*
