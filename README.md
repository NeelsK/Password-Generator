# Password Generator

A fully offline, browser-based password generator with three modes: random, memorable, and PIN. Features an entropy-based strength meter, a cognitive science-backed Memory Score for word-based passwords, and colour-coded character display. Everything runs client-side — nothing is ever transmitted.

Open `password-generator.html` in any modern browser. No installation, no dependencies, no build step.

---

## Modes

### Random

Generates a password from a configurable character set. You control the length (4–100 characters) and which character classes to include: uppercase letters (A–Z), lowercase letters (a–z), digits (0–9), and symbols (`!@#$%^&*()_+-=[]{}|;:,.<>?`). At least one class must be enabled.

The generator guarantees at least one character from each enabled class, then fills the remaining length from the combined pool. The result is shuffled using a Fisher-Yates shuffle with cryptographic randomness.

**Default:** 20 characters with all four classes enabled.

### Memorable

Generates a passphrase by picking random words from a built-in dictionary of common English nouns (3–6 letters each, roughly 800 words). The words are chosen for concreteness — things you can picture in your mind — which makes them easier to remember.

Options:

- **Words** (2–8) — how many words to include. Default: 4.
- **Separator** — what goes between words. Choices: Hyphens (`-`), Spaces, Periods (`.`), Commas (`,`), Underscores (`_`), Numbers (a random digit between each word), or Numbers and Symbols (a random digit or symbol between each word).
- **Capitalize** — when on, capitalises the first letter of one randomly chosen word. The capitalised word changes each time you regenerate.
- **Full Words** — when on, words appear in full. When off, words are truncated to 3–5 character fragments, which produces shorter but less readable passwords.

### PIN

Generates a numeric-only code of configurable length (3–12 digits). Default: 6 digits.

---

## Strength Meter

A colour-coded bar below the password display estimates the password's strength based on Shannon entropy. The calculation considers the size of the character pool actually used in the password and its length.

| Entropy (bits) | Rating      | Colour |
| -------------- | ----------- | ------ |
| < 30           | Very Weak   | Red    |
| 30–49          | Weak        | Orange |
| 50–69          | Fair        | Yellow |
| 70–89          | Strong      | Green  |
| 90+            | Very Strong | Cyan   |

The strength meter applies to all three modes.

---

## Memory Score

Visible only in Memorable mode. A 0–100 score estimating how easy the generated passphrase will be to remember, based on the Brysbaert et al. concreteness ratings — a database of 40,000 English words rated on a 1–5 scale for how concrete and imageable they are (5 = maximally concrete, like "apple"; 1 = highly abstract, like "theory").

Each word in the passphrase is looked up in this database. The raw concreteness rating is normalised to a 0–100 score, then adjusted:

- **Bonus (+5):** all words score 60 or above (all concrete).
- **Penalty (-10 per word):** any word scores below 30 (abstract).
- **Recall load penalty (-3 per word):** for phrases longer than 4 words, each additional word reduces the score.

| Score | Label            | Colour |
| ----- | ---------------- | ------ |
| 85+   | Easy to remember | Cyan   |
| 65–84 | Moderate         | Green  |
| 40–64 | Challenging      | Yellow |
| < 40  | Hard to remember | Red    |

Regenerate until you get a score you're comfortable with. A 4-word phrase with concrete nouns typically scores 75–95.

---

## Colour-Coded Characters

The password display uses colour to distinguish character types at a glance:

- **White** — lowercase letters
- **Purple** — uppercase letters
- **Cyan** — digits
- **Pink** — symbols and separators

---

## Keyboard Shortcuts

- **R** — regenerate (when no input field is focused)
- **Ctrl+C / Cmd+C** — copy the current password (when no text is selected)

---

## Security

All randomness comes from `crypto.getRandomValues()`, the browser's cryptographically secure pseudorandom number generator. No Math.random() is used anywhere.

The generator runs entirely in your browser. There are no network requests, no analytics, no cookies, and no external dependencies beyond the page itself. The word list and Memory Score lookup table are embedded directly in the HTML file.

---

## Running the Tool

Open `password-generator.html` in any modern browser (Chrome, Firefox, Edge, Safari). It works from a local filesystem, a USB drive, or a web server.

**Requirements:** A browser that supports the Web Crypto API (`crypto.getRandomValues`). All browsers released after 2015 qualify.

---

## License

MIT
