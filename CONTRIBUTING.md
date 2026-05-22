# Contributing translations

Thanks for helping make Rosetta Check usable in more languages. This guide covers the two common cases:

1. **Fix or improve an existing translation** (a string sounds wrong or is missing).
2. **Add a brand-new language** that isn't in the table yet.

Please also read [STYLE-GUIDE.md](STYLE-GUIDE.md) before you start — it lists terms that **must not** be translated (e.g. "Rosetta Check", "Apple silicon", "Intel") and explains the tone the app uses.

For **practical advice on editors, machine translation, plurals, format strings (`%@`, `%d`), and per-language reference material**, see [TRANSLATION-TIPS.md](TRANSLATION-TIPS.md). New contributors should skim it at least once — it'll save you time and avoid the most common mistakes.

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

## Testing your translation

Rosetta Check is closed-source, so you can't build it locally. Verification of a PR happens in two stages:

1. **You** — read the [STYLE-GUIDE.md](STYLE-GUIDE.md), check your translations are well-formed in the diff, and confirm `state` is set to `translated`.
2. **Maintainer** — runs your branch against a development build before merging, checks for length/truncation issues in the UI, and flags anything that doesn't fit.

Once merged and shipped in the next release of the app (download: <https://rosettacheck.com/app>), you can force-launch in your language to see your work live:

```bash
/Applications/Rosetta\ Check.app/Contents/MacOS/Rosetta\ Check -AppleLanguages '(fr)'
```

Replace `fr` with your language code.

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
