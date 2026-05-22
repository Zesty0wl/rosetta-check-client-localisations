# Contributing translations

Thanks for helping make Rosetta Check usable in more languages. This guide covers the two common cases:

1. **Fix or improve an existing translation** (a string sounds wrong or is missing).
2. **Add a brand-new language** that isn't in the table yet.

Please also read [STYLE-GUIDE.md](STYLE-GUIDE.md) before you start — it lists terms that **must not** be translated (e.g. "Rosetta Check", "Apple silicon", "Intel") and explains the tone the app uses.

---

## Before you start

- You need a GitHub account.
- You **do not** need Xcode, a Mac, or any Apple developer tools. You can edit everything in the GitHub web UI.
- Open an issue first if you want to claim a language so two people don't duplicate work. Use the [Translation request](.github/ISSUE_TEMPLATE/translation-request.yml) template.

---

## Case 1: Fix an existing translation

1. Find the string in [`Localizable.xcstrings`](Localizable.xcstrings). The file is JSON; the key is the English source string.
2. Edit the `value` for your language under `localizations.<code>.stringUnit.value`.
3. Change `state` to `translated` if it was anything else.
4. Open a PR. The title should be `[fr] Fix translation of "..."` (replace `fr` with your language code).

### Example

Before:

```json
"Scan complete" : {
  "localizations" : {
    "fr" : {
      "stringUnit" : { "state" : "translated", "value" : "Scan terminé" }
    }
  }
}
```

After (correcting "Scan" to "Analyse"):

```json
"Scan complete" : {
  "localizations" : {
    "fr" : {
      "stringUnit" : { "state" : "translated", "value" : "Analyse terminée" }
    }
  }
}
```

---

## Case 2: Add a brand-new language

1. Open an issue using the [Translation request](.github/ISSUE_TEMPLATE/translation-request.yml) template so we can confirm the locale code and tag you in the credits.
2. In `Localizable.xcstrings`, for **every** string key, add a new entry under `localizations` using your ISO 639-1 code (e.g. `es` for Spanish, `pt-BR` for Brazilian Portuguese):

   ```json
   "Scan complete" : {
     "localizations" : {
       "en" : { "stringUnit" : { "state" : "translated", "value" : "Scan complete" } },
       "es" : { "stringUnit" : { "state" : "translated", "value" : "Análisis completado" } }
     }
   }
   ```
3. Do the same in `InfoPlist.xcstrings` (only ~5 strings).
4. Create `metadata/<locale>/` with `description.txt`, `keywords.txt`, `promotional_text.txt` translated from `metadata/en-US/`.
5. Open the PR. The title should be `Add <Language> (<code>) translation`.

There are currently **439** strings in `Localizable.xcstrings`. You don't have to do them all in one PR — partial translations are welcome. Untranslated strings fall back to English at runtime, so nothing breaks.

---

## The `state` field — important

Every string has a `state` per language. The values that matter:

| `state`          | Meaning                                                          |
|------------------|------------------------------------------------------------------|
| `translated`     | ✅ The translation is current and will be shown to users.        |
| `needs_review`   | ⚠️ Will be shown but is flagged for a maintainer to check.       |
| `new`            | ❌ Falls back to English at runtime.                             |
| `stale`          | ❌ English source changed; translation is outdated.              |

**If you edit a `value` you must also set `state` to `translated`** or your work won't appear in the app. This is the #1 mistake first-time contributors make.

---

## Testing your translation (optional)

If you have a Mac and want to preview your work in the actual app before submitting:

1. Install the latest release from [rosettacheck.com](https://rosettacheck.com).
2. Force the language at launch from Terminal:

   ```bash
   /Applications/Rosetta\ Check.app/Contents/MacOS/Rosetta\ Check -AppleLanguages '(fr)'
   ```

   Replace `fr` with your language code.

Note: you'll be testing against the *currently shipped* translations, not your PR. Maintainers will verify your changes against a development build before merging.

---

## Licence & contributor agreement

By opening a pull request against this repo you agree that:

1. Your contribution is your own work (or properly attributed if adapted).
2. You licence your contribution under the [MIT licence](LICENSE).
3. You grant the Rosetta Check maintainers a perpetual, worldwide, royalty-free right to use, modify, and redistribute your contribution inside the closed-source Rosetta Check application and any related materials (App Store listings, marketing, documentation).

If you can't agree to these terms, please don't submit a PR.

---

## Questions

Open an issue with the [Question](.github/ISSUE_TEMPLATE/question.yml) template, or email `hello@rosettacheck.com`.
