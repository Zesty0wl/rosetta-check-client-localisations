# Rosetta Check — Community Translations

This repo holds the translation files for [**Rosetta Check**](https://rosettacheck.com), a macOS utility that tells you which of your apps still depend on Rosetta 2 so you can plan your move to Apple silicon.

Download the app: <https://rosettacheck.com/app>

The app itself is closed-source, but the translations are open. **Anyone can submit a fix or a brand-new language** via pull request.

## Languages

| Code | Language    | Status                         |
|------|-------------|--------------------------------|
| `en` | English     | ✅ Source language (100%)      |
| `fr` | French      | 🌍 Community-maintained        |
| `de` | German      | 🌍 Community-maintained        |
| `ja` | Japanese    | 🌍 Community-maintained        |

Want to add a language or fix a wrong string? See [CONTRIBUTING.md](CONTRIBUTING.md). For practical tips on editors, machine translation, plurals, and per-language style references, see [TRANSLATION-TIPS.md](TRANSLATION-TIPS.md).

## How translations reach the app

1. You open a PR against this repo.
2. A maintainer reviews and merges.
3. The next Rosetta Check release pulls in this repo's `main` branch at build time and ships your translation in the App Store / direct download.

There is **no separate sign-off** beyond the PR review — once merged, your translation is live in the next release (typically within 2–4 weeks).

## Files

- [`Localizable.xcstrings`](Localizable.xcstrings) — every user-facing string in the app. **This is the file most contributors edit.**
- [`InfoPlist.xcstrings`](InfoPlist.xcstrings) — app name and macOS-level usage descriptions (Spotlight, Notifications, etc.). Rarely changes.
- [`metadata/`](metadata/) — App Store Connect copy (description, keywords, promotional text) per locale.

## Licence

All translations in this repo are released under the [MIT licence](LICENSE). By submitting a PR you agree your contribution is MIT-licensed and may be redistributed inside the closed-source Rosetta Check application. See [CONTRIBUTING.md](CONTRIBUTING.md#licence--contributor-agreement) for the full text.

## Credits

Translators are listed in [CONTRIBUTORS.md](CONTRIBUTORS.md). Thank you 🙏
