# Translation style guide

Rosetta Check's voice in English is: **calm, plain-spoken, slightly informal, never alarming.** It's a utility — not a security product — so it should sound helpful, not scary. Try to keep that register in your language.

## Tone

- **Plain words over jargon.** Prefer the everyday equivalent. The app is read by IT admins *and* curious end-users.
- **Active voice.** "Rosetta Check found 3 apps" not "3 apps were found".
- **Direct, second person.** "You can…" not "The user can…".
- **No exclamation marks** except in success states ("100% Ready!"). Never in warnings.
- **No emoji in body text.** Some are used as decorative status icons in the source — keep those, don't add new ones.

## Terms that must NOT be translated

| Term              | Why                                              |
|-------------------|--------------------------------------------------|
| Rosetta Check     | Product name                                     |
| Rosetta / Rosetta 2 | Apple technology name                          |
| Apple silicon     | Apple's official term (lowercase 's' is correct) |
| Intel             | Brand name                                       |
| arm64 / x86_64    | Architecture identifiers                         |
| Mach-O            | Binary format name                               |
| Spotlight         | macOS feature name                               |
| App Store         | Apple service name                               |
| Finder            | macOS feature name                               |

## Terms with preferred translations

When the source string says **"Intel app"**, translate "app" but keep "Intel". Examples:

- 🇫🇷 *application Intel*, not *application Intel-native*
- 🇩🇪 *Intel-App*
- 🇯🇵 *Intel アプリ*

When the source says **"Apple silicon app"** or **"native app"**, translate the same way:

- 🇫🇷 *application Apple silicon* / *application native*
- 🇩🇪 *Apple-silicon-App* / *native App*
- 🇯🇵 *Apple silicon ネイティブアプリ* / *ネイティブアプリ*

## Numbers, dates, units

- Use the locale's standard decimal separator (`,` in fr/de, `.` in en/ja).
- Dates should follow the locale's `medium` format — don't hard-code `dd/mm/yyyy`.
- File sizes: keep the unit as shown in source (`MB`, `GB`). Don't translate.

## Length

macOS dialogs and menus have limited width. Try to keep translations within **±30%** of the English source length. If a translation is much longer:

- Check for an idiomatic shorter form.
- If unavoidable, flag it in the PR and a maintainer will check the UI.

## Punctuation per locale

- 🇫🇷 French uses a non-breaking space before `: ; ! ?` — please include it.
- 🇯🇵 Japanese uses full-width punctuation (`、 。 ：`) inside Japanese sentences but ASCII (`, . :`) inside English brand names.
- 🇩🇪 German keeps quotation marks as „…" (lower-opening, upper-closing).

## Capitalisation in UI strings

The source English uses **Sentence case** for buttons and menu items ("Open settings", not "Open Settings"). Match your locale's UI convention — German nouns are always capitalised, French sentence case is fine.

## Notifications

Notification titles are short (≤ 40 chars) and never end with punctuation. Bodies are 1–2 short sentences, plain.

Source example:

> **Title:** Intel app installed
> **Body:** Rosetta Check spotted a new app that still needs Rosetta 2.

Avoid making notification copy more dramatic than the English — users get one of these per Intel app installed and we don't want notification fatigue.

## When in doubt

Open a draft PR and ask. Better to ship a slightly-imperfect translation that real users can react to than to block on perfection.
