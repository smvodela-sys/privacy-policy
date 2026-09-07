# Legal pages

Three static pages — `privacy.html`, `terms.html`, `eula.html` — plus `legal.css`.
Zero build, zero cost. Host on GitHub Pages (or any static host).

## Status: content is READY, hosting is the last step

All three pages are current as of 6 Sep 2026: no car photo (the app takes a
selfie plus a free-text description), and all five AI providers named with links
to their own policies.

**The app links them at:**

```
https://smvodela-sys.github.io/privacy-policy/myothercar/privacy.html
https://smvodela-sys.github.io/privacy-policy/myothercar/terms.html
https://smvodela-sys.github.io/privacy-policy/myothercar/eula.html
```

That base lives in one place — `AppCopy.legalBase` in `lib/core/app_copy.dart` —
and is overridable at build time without a code change:

```
flutter build ... --dart-define=LEGAL_BASE_URL=https://your-host/path
```

## Publish (the one remaining manual step)

Copy the four files into a `myothercar/` folder in the `privacy-policy` Pages
repo and push:

```bash
cp web/legal/*.html web/legal/legal.css /path/to/privacy-policy/myothercar/
```

Then verify — this is also enforced by the build:

```bash
make _guard-legal-urls
```

`make release-android` and `make release-ios` both run that guard and **refuse
to build** until all three return 200. A dead Terms link on the paywall is an
App Store 3.1.2 rejection, and the EULA link sits behind the onboarding consent
gate — asking the user to agree to a page that 404s.

## Do NOT use the account-wide policy

`https://smvodela-sys.github.io/privacy-policy/` (the root page) covers every
app on the developer account and is **wrong for this one**. It states, under
"Data We Do Not Collect", that photos are never uploaded, and again in a callout
that "we never transmit your photos, images…". This app uploads the user's
selfie to a third-party model on every generation. Shipping that link is an App
Store 5.1.1 data-accuracy problem and a Play Data Safety mismatch. It also
declares AppLovin advertising and IDFA collection, which this app does not do.

Use `myothercar/privacy.html` from this folder. `test/unit/copy_audit_test.dart`
asserts the app never points at the root policy.

## Still to confirm

- **Support / Marketing URLs** in `web/store/APP_STORE.txt` point at
  `rvodela.github.io/myothercar/` — confirm that Pages site is live, or swap in
  the support email (`r_vodela@yahoo.com`).

## Review before submission

The wording tracks spec §11: no-servers positioning, EXIF stripping, crash-only
Crashlytics with opt-out, "subscription buys the app as-is, not generation costs,
not future content, not support", provider-quality disclaimer, AI-content
labelling. Have a lawyer review if the app is monetised at scale — these are a
solid honest starting point, not legal advice.
