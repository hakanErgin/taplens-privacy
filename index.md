---
layout: default
title: TapLens Privacy Policy
---

# TapLens Privacy Policy

**Version covered:** 0.5.0 (version code 31) and later, until this page says otherwise
**Last updated:** October 25, 2026

This policy describes the TapLens Android app (`app.taplens`) from version 0.5.0 (version code 31). Earlier versions are described in [Earlier versions](#earlier-versions-04x).

## Who we are

TapLens is developed and operated by **Hakan Ergin**, an individual developer based in Turkey. Hakan Ergin is the data controller for personal data processed by TapLens under the Turkish Personal Data Protection Law No. 6698 (KVKK) and, for users in the EEA and UK, under the EU and UK General Data Protection Regulation (GDPR). For questions about this policy or your data, contact **hakan.ergin@gmail.com**.

## Summary

- TapLens works on your device. Screen capture, text recognition, translation and Live mode all run on the phone.
- TapLens sends no screen content, identifiers or usage data to the developer. There is no TapLens server, account, analytics, crash reporting or advertising.
- Google ML Kit, which TapLens uses for recognition and translation, downloads language packs and sends Google its own usage and performance metrics. See [Google ML Kit](#google-ml-kit).
- Live mode is a one-time purchase through Google Play. Google Play processes the payment. See [Purchases](#purchases).

## What TapLens does

TapLens is a system-overlay translator. You tap a floating button while using another app; TapLens captures the current screen, recognizes text on it, translates that text, and draws the translation over the original app. Snapshot mode is free. Live mode, which refreshes translations as you scroll, is unlocked with a one-time purchase.

## Screen capture and recognized text

- **Capture runs only while the translator is on.** You start the translator yourself and grant Android's screen-capture permission, and TapLens shows a persistent notification while it runs. When you stop the translator, capture stops.
- **What starts a translation.** A translation starts when you tap the floating button or use a translation gesture. While translations are shown, TapLens also watches the captured frames on the device and, when the screen changes and then settles (for example after you scroll, or when a new message appears), translates the new screen so the overlay stays correct. In Live mode, scrolling in the underlying app also triggers this refresh after your finger lifts and scrolling pauses.
- **Everything stays on the device.** TapLens passes the captured image to on-device text recognition: Google ML Kit for its supported scripts, or Tesseract with model files bundled in the app for other source languages. Translation uses ML Kit language packs downloaded to the device. TapLens does not upload the captured image, the recognized text or the translation, and does not save the captured image to storage.

## Live mode and the Accessibility service

Live is optional, needs Android 13 or newer, and requires the one-time Live unlock. Before you enable it, TapLens shows an in-app disclosure describing what the service does; choosing **Continue to settings** records your choice and opens Android's Accessibility settings, where you decide whether to turn on the **TapLens Live** service. TapLens uses the Accessibility API only to provide Live; it is not an assistive tool.

While enabled, the TapLens Live service receives:

- **Touch positions and gesture state** while translations are visible, so that a vertical drag clears the translations and continues your drag in the app underneath, and a tap keeps the translations visible.
- **App metadata:** the package name of the app in the foreground, its window identifier when available, and scroll and app-switch events. TapLens uses these to know when to refresh or clear translations.
- **Whether another service is using touch exploration** (for example a screen reader), so Live can step aside and fall back to Snapshot.

Touch positions, app package names, and window identifiers stay only in device memory while the service runs. They are never saved, logged, sent anywhere, or used for analytics or advertising. TapLens keeps no log of Live activity. The service does not read screen elements or screen text, take screenshots, click, type, or create gestures. Translation itself still uses screen capture and on-device recognition and translation as described above.

To turn Live off, choose **Snapshot** in TapLens or disable TapLens Live in Android's Accessibility settings. TapLens stores your Live/Snapshot choice and which version of the disclosure you accepted.

## Google ML Kit

Google ML Kit processes the captured image (for its supported scripts), the recognized text and every translation on the device. Tesseract uses model files bundled with the app and does not download anything.

ML Kit may contact Google to download translation language packs, receive model updates and check device compatibility. It also sends Google utilization and performance metrics. Google's [ML Kit Terms & Privacy](https://developers.google.com/ml-kit/terms) and [ML Kit Android data-disclosure guidance](https://developers.google.com/ml-kit/android-data-disclosure) describe these metrics with categories such as device and application information, device or other identifiers, latency and other performance metrics, API configuration, feature input and output size, feature version, event type, error codes, and for translation, the configured source and target languages. Those published categories do not list the recognized screen text or captured image as a metric field. TapLens does not control the exact metric payload or its retention period.

## Purchases

Live mode is a one-time in-app purchase sold through **Google Play**. Google processes the payment as an independent controller under [Google's Privacy Policy](https://policies.google.com/privacy); TapLens never receives your card or bank details.

To know whether Live is unlocked, the app asks Google Play on your device and checks Google Play's signed answer on the device. It stores one fact on the device, whether Live was last confirmed as unlocked, so Live works offline. If a payment is pending or a confirmation fails, the app retries in the background with Google Play. TapLens sends nothing about your purchase to the developer and keeps no purchase records off the device. Refunds and purchase history are handled by Google Play.

## Data stored on your device

TapLens stores on your device: your language choices and recent language pairs, display preferences, your Live/Snapshot choice and accepted disclosure version, whether Live was last confirmed as unlocked, and downloaded ML Kit language packs. Uninstalling TapLens or clearing its app data removes them; downloaded language packs can also be deleted from the Language Picker.

## Android permissions

- **Display over other apps:** to show the floating button and translation overlay.
- **Screen capture (MediaProjection):** requested through the Android system dialog and used only as described above.
- **Foreground service (media projection):** keeps screen capture running only while the translator is on, with its persistent notification.
- **Notifications:** Android requires a persistent notification while the translator runs.
- **Accessibility service (TapLens Live):** optional and off unless you enable it in Android settings; used only as described in [Live mode and the Accessibility service](#live-mode-and-the-accessibility-service).
- **Network access and network state:** for ML Kit language-pack downloads and for asking Google Play about the Live purchase.
- **Google Play Billing:** added by the Google Play Billing library to sell and restore the Live unlock.
- **Run at startup and keep awake:** added by Android's WorkManager library so a pending purchase confirmation can be retried after a restart.

TapLens does not request the advertising ID, location, contacts or storage permissions.

## Contacting support

The **Contact support** row opens your email app with the support address. An email you send is ordinary correspondence: we use it to answer you and keep it only as long as needed for that.

## Your choices and rights

- Choose **Snapshot**, or disable TapLens Live in Android's Accessibility settings, to turn off the Accessibility service.
- Delete downloaded translation language packs from the Language Picker.
- Uninstall the app or clear its app data to remove everything TapLens stores on your device.
- Request refunds and view your purchase in Google Play.

Because TapLens 0.5.0 and later sends nothing to the developer, we hold no app data about you. Under KVKK Article 11, you have the right to learn whether your personal data is processed and to request information about it; to learn the purpose of processing and whether data is used for that purpose; to know the third parties in Turkey or abroad to whom data is transferred; to request correction, deletion or destruction of data and that these be notified to those third parties; to object to a result against you arising solely from automated analysis; and to claim compensation for damage caused by unlawful processing. If you are in the EEA or UK, you also have rights under the GDPR, including access, rectification, erasure, restriction, objection and data portability, and you may lodge a complaint with your local data protection authority. Send requests to **hakan.ergin@gmail.com**; we will respond within 30 days.

## Children and age

TapLens is designed for adults aged 18 and older and is not directed at children. We do not knowingly collect personal information from children.

## Earlier versions (0.4.x)

Versions 0.4.x (up to 0.4.24, version code 30) were distributed only to closed testing tracks, including the separate closed-test edition `app.taplens.closedtest`. They worked differently: they used an anonymous Firebase identity, Firebase App Check, Crashlytics, Analytics and Remote Config, Google AdMob and User Messaging Platform in the Free tier, Google Play subscriptions verified by the TapLens server, and, for Premium cloud translation, sent recognized text to the TapLens server and its translation providers. The edition of this policy that covered those versions remains available in the history of this policy's repository. To ask about data from those versions, contact **hakan.ergin@gmail.com**.

## Changes to this policy

We may update this policy as TapLens changes. The last-updated date at the top will change when we do. Material changes will be reflected in the Play Store listing or communicated in the app where appropriate.

---

*This policy applies to the TapLens Android app (`app.taplens`) from version 0.5.0 (version code 31).*
