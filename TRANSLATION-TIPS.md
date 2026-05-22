# Translation tooling and tips

Practical advice for making translation work easier and avoiding the common pitfalls of Apple's String Catalog (`.xcstrings`) format.

If you just want the workflow basics, start with [CONTRIBUTING.md](CONTRIBUTING.md). This document covers **how** to do the work efficiently once you're set up.

---

## Pick an editor

You can use whichever of these you're comfortable with — the file format is the same JSON either way.

### Option 1 — GitHub web UI (zero install)

Best for: small fixes, one or two strings, contributors who don't want to install anything.

1. Browse to [`Localizable.xcstrings`](Localizable.xcstrings) on github.com.
2. Click the pencil icon ("Edit this file").
3. Use the in-browser editor's find (⌘F / Ctrl+F) to jump to your string.
4. Edit the `value`, set `state` to `"translated"`, commit to a new branch, open a PR.

Downsides: no syntax check (a misplaced comma breaks the file), no autocomplete, slow for big edits.

### Option 2 — VS Code or any text editor

Best for: medium-sized edits, batch finds-and-replaces.

1. Clone the repo: `gh repo clone Zesty0wl/rosetta-check-client-localisations` (or use the green "Code" button → Download ZIP).
2. Open `Localizable.xcstrings` in VS Code. It's just JSON — syntax highlighting, find/replace, and JSON linting come for free.
3. Install the [JSON Schema for Apple xcstrings](https://github.com/Wouter01/xcstrings-tools) extension if you want stricter validation (optional).

Tip: VS Code's **Find in Selection** is great when you only want to change one language — select the `"fr"` block and constrain your search/replace to it.

### Option 3 — Xcode (best UX, Mac-only, needs free Apple Developer ID)

Best for: heavy or first-time-language work. Apple's native editor is genuinely excellent.

1. Install Xcode from the Mac App Store (free, ~10 GB).
2. Clone this repo and double-click `Localizable.xcstrings`. Xcode opens it in a table editor with one row per source string, one column per language.
3. Click a cell and type. Xcode auto-flips `state` to `translated` for you, validates format strings live, and shows you plural variations in an expandable inline editor.
4. Save (⌘S). The file format on disk is identical to what you'd hand-edit.

Don't worry — Xcode here is just acting as a JSON editor. You don't need to build, run, or know any Swift.

### Option 4 — Web-based localisation platforms

If you're a translator who already lives in Crowdin, Weblate, POEditor, or Lokalise, all four understand `.xcstrings` natively. You can fork this repo, point your platform at it, and let it sync via PRs. Talk to the maintainers first so we don't end up with conflicting syncs.

---

## Things that look like ordinary text but **must not be changed**

These elements appear inside `value` strings. Translate the human-language words around them, but leave these tokens exactly as they appear:

| Token         | Example                            | What it does |
|---------------|------------------------------------|--------------|
| `%@`          | `Found %@ Intel apps`              | Inserts a string at runtime. |
| `%d`, `%lld`  | `%d apps`, `%lld bytes`            | Inserts a number. |
| `%1$@`, `%2$@`, `%1$d` | `Updated %1$@ on %2$@`    | **Positional** — you can re-order these in your translation, but keep the numbers (`1$`, `2$`) the same. |
| `%%`          | Literal `%` sign                   | Don't drop the second `%`. |
| `\n`          | Line break                         | Keep where it appears unless the line break makes no sense in your language. |
| `\"` and `\\` | Escaped quote / backslash          | Don't unescape. JSON parser needs these. |
| Anything in `{}`  | `{appName}`, `{count}`         | Named placeholder (rare in this app) — same rules as positional. |

### How to reorder positional placeholders (the one safe rewrite)

English: `"Updated %1$@ on %2$@"` ("Updated {app} on {date}")
French:  `"Mis à jour le %2$@ : %1$@"` ("Updated on {date}: {app}")

The numbers `1$` and `2$` follow the *original English meaning*, so the French still inserts the right values in the right order.

If the source string uses bare `%@` without numbers (no `$`), you cannot reorder — the values come out in source order.

---

## Plurals (the `variations` shape)

A small number of strings use Apple's plural-variations format because English's "1 app" vs "2 apps" doesn't map cleanly to other languages. They look like this:

```json
"%d Intel app(s) detected" : {
  "localizations" : {
    "en" : {
      "variations" : {
        "plural" : {
          "one"   : { "stringUnit" : { "state" : "translated", "value" : "%d Intel app detected" } },
          "other" : { "stringUnit" : { "state" : "translated", "value" : "%d Intel apps detected" } }
        }
      }
    }
  }
}
```

Your language may need **more** plural categories than English. The standard Unicode CLDR categories per language are:

- **French (`fr`)** — `one`, `many`, `other` (1 is `one`, big numbers like 1_000_000 are `many`, rest are `other`)
- **German (`de`)** — `one`, `other`
- **Japanese (`ja`)** — `other` only (no plurals; one form for all numbers)
- **Russian (`ru`)** — `one`, `few`, `many`, `other` (the famous tricky one)
- **Polish (`pl`)** — `one`, `few`, `many`, `other`
- **Arabic (`ar`)** — `zero`, `one`, `two`, `few`, `many`, `other`

Full reference: [Unicode CLDR plural rules](https://cldr.unicode.org/index/cldr-spec/plural-rules).

You only need to fill in the categories your language actually uses. If you skip a needed category, the app falls back to English.

---

## Per-language localisation references (gold)

These are the single most useful documents for getting tone, terminology, and punctuation right. Read the one for your language **before** you start.

| Language | Reference |
|----------|-----------|
| French   | [Microsoft French Style Guide (PDF)](https://learn.microsoft.com/en-us/globalization/reference/microsoft-style-guides), [Wikipedia: French typography](https://en.wikipedia.org/wiki/French_orthography) |
| German   | [Microsoft German Style Guide](https://learn.microsoft.com/en-us/globalization/reference/microsoft-style-guides) — covers du vs. Sie, capitalisation, compound nouns |
| Japanese | [Apple Japanese Localization Glossary](https://help.apple.com/applestyleguide/) (look for the JA section), [Microsoft Japanese Style Guide](https://learn.microsoft.com/en-us/globalization/reference/microsoft-style-guides) |
| Spanish, Portuguese, Italian, Korean, Chinese (S/T), Dutch, Polish, Russian, Arabic, etc. | All covered by [Microsoft's per-language style guides](https://learn.microsoft.com/en-us/globalization/reference/microsoft-style-guides) and Apple's [terminology glossaries](https://applelocalization.com/). |

**[Apple Localization Terms](https://applelocalization.com/)** is particularly useful — it lets you search any English Apple UI term ("Settings", "Quit", "Preferences") and see how Apple has translated it across all macOS/iOS releases for 40+ languages. If you're unsure how to translate a system-y word, copy what Apple uses.

---

## Machine translation as a starting point

Using MT (DeepL, Google Translate, ChatGPT, etc.) for a first pass is **fine and encouraged**, as long as you review every output. Strong rules:

- **Always read every machine-translated string before committing.** MT regularly produces grammatically correct but wrong-in-context translations ("Scan" → "scanner" the device vs. "analyser" the data).
- **DeepL** is currently the strongest for fr/de/es/it/pt/nl/pl/ru. Free tier handles this whole catalogue.
- **Apple's Translate.app** (macOS / iOS) is decent for ja/ko/zh and is offline.
- **Never run MT in bulk and submit without review.** It's obvious in PR review and will be rejected.
- MT does **not** understand format specifiers — it may delete `%@`, change `%1$d` to `%1 $d`, etc. Always check.

Recommended flow:
1. MT a batch of 20–30 strings into a scratch document.
2. Manually review/fix each.
3. Paste into the xcstrings file (or use the [legacy `merge-translations.py`](https://github.com/Zesty0wl/rosetta-check/blob/main/scripts/merge-translations.py) script in the app repo if a maintainer gives you a flat-JSON workflow).

---

## Working efficiently on a big language pass

If you're adding a new language from scratch (≈440 strings):

1. **Don't try to do it in one PR.** Open a draft PR early with the language code added in a few entries. Maintainers can sanity-check tone before you commit to the full pass.
2. **Group by screen, not by file order.** The xcstrings keys are roughly in the order strings were added to the app, not by where they appear in the UI. Translating "Cancel" and "Save" alongside "Scan complete" gives you better context than translating top-to-bottom.
3. **Use the `comment` field** if one exists on a key — it's a hint left by the developer about where the string is used.
4. **Push every 50–100 strings** so reviewers can comment incrementally instead of all-at-once.
5. **Mark genuinely uncertain translations as `state: "needs_review"`** rather than `"translated"`. The app will still show them, but the maintainer (or another native speaker) knows to double-check.

---

## Validation before you push

A GitHub Action runs on every PR and will catch broken JSON / invalid `state` values. To pre-check locally:

```bash
python3 -c "import json; json.load(open('Localizable.xcstrings'))" && echo "valid JSON ✓"
```

If that prints `valid JSON ✓`, you're safe to push. If it prints a `json.decoder.JSONDecodeError`, scroll to the line/column it mentions and fix the syntax (usually a stray comma or missing quote).

---

## Quick wins for a first PR

If you want to test the workflow before committing to a big translation:

1. Pick one obviously-wrong existing string in your language. Real misses exist — the current FR/DE/JA were partly machine-assisted.
2. Open the [translation-fix issue template](.github/ISSUE_TEMPLATE/translation-fix.yml) to flag it.
3. Or skip the issue and go straight to a PR fixing it.

Small, focused PRs get merged within a day or two.

---

## Questions

Open a [question issue](.github/ISSUE_TEMPLATE/question.yml) or email `hello@rosettacheck.com`.
