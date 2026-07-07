# Password-Checker
# Password Strength Checker

A password strength assessment tool available in three forms: a command-line script, a desktop GUI, and a standalone web app. It evaluates a password's strength using length, character variety, Shannon-style entropy, and a common/leaked password check.

Built as **Project 1** for the DecodeLabs Industrial Training Kit (Batch 2026), then extended beyond the original brief with entropy scoring and crack-time estimation.

**[Live demo](#)** — enable GitHub Pages (see below) and drop the link here.

---

## Preview

The web version presents results as a security assessment report: a stamped verdict (Weak / Medium / Strong), an entropy-based strength meter, and a findings list with concrete recommendations.

---

## Features

-  Password length check
-  Character variety check — lowercase, uppercase, digits, symbols
-  Entropy calculation (`length × log2(character pool size)`)
-  Estimated crack time, derived from entropy at a realistic offline attack rate
-  Common/leaked password detection
-  Pattern detection — sequential runs (`123`, `qwerty`) and repeated characters (`aaa`)
-  Actionable, specific recommendations
-  Strong password generator
-  Three interfaces: CLI, desktop GUI, and browser-based web app

---

## Files

| File | Description |
|---|---|
| `password_checker.py` | Command-line version. Run in a terminal, type a password, get instant feedback. |
| `password_checker_gui.py` | Desktop GUI built with Tkinter. Live strength meter, checklist, and tips as you type. |
| `password_checker.html` | Self-contained web app. Dark themed, entropy-based scoring, crack-time estimate, password generator. Runs entirely client-side — no data ever leaves the browser. |

---

## Requirements

- Python 3.7+ for the CLI and GUI versions (standard library only — no external installs needed)
- Any modern browser for the web version (`password_checker.html`), no server required

If `tkinter` isn't available on Linux:
```bash
sudo apt install python3-tk
```

---

## Usage

### CLI version
```bash
python password_checker.py
```
Type a password when prompted. Type `exit` to quit.

### Desktop GUI version
```bash
python password_checker_gui.py
```
Type into the password field — the strength bar, checklist, and tips update live.

### Web version
Open `password_checker.html` directly in a browser, or serve it via GitHub Pages for a shareable link (see below).

---

## How Scoring Works

### CLI / GUI versions
| Criterion | Points |
|---|---|
| Length ≥ 8 characters | +1 |
| Length ≥ 12 characters | +2 |
| Lowercase / uppercase / digit / symbol present | +1 each |

Classified as **Weak** if on the common-password list, fewer than 2 character types are present, or score ≤ 2. **Strong** requires ≥3 character types and score ≥5. Everything else is **Medium**.

### Web version (entropy-based)
Entropy is calculated as:

```
entropy_bits = password_length × log2(character_pool_size)
```

where the pool size is the sum of the character classes actually present (26 lowercase, 26 uppercase, 10 digits, 32 symbols). Crack time is estimated by assuming an offline attack rate of 10 billion guesses/second, a realistic figure for GPU-based hash cracking.

A password on the common/leaked list is automatically marked **Weak** regardless of entropy — predictability matters more than raw complexity.

---

## Design Notes

Length and character variety alone are weak signals in isolation:
- A long password of one repeated character (`aaaaaaaaaaaa`) is not secure.
- A short password with every character type (`P@s1`) is still easy to brute-force.

The web version's entropy + pattern-detection approach models this more realistically, closer to how tools like `zxcvbn` or HaveIBeenPwned approach password evaluation.

### Possible extensions
- Real breach checking via the HaveIBeenPwned API using k-anonymity (partial hash lookup, so the password itself is never transmitted) — requires a small backend or CORS-friendly proxy, since browsers can't call it directly from a static page
- Replace the demo `COMMON_PASSWORDS` set with a full leaked-password dataset
- Package the desktop GUI as a standalone executable with `pyinstaller`

---

## Deploying the Web Version (GitHub Pages)

1. Push this repo to GitHub (see commands below).
2. In the repo, go to **Settings → Pages**.
3. Under **Source**, select the `main` branch and `/ (root)` folder.
4. Save. GitHub will publish at `https://<your-username>.github.io/password-strength-checker/password_checker.html`.
5. Optionally rename `password_checker.html` to `index.html` for a cleaner URL.

---

## Author

Built as part of the DecodeLabs Cyber Security Industrial Training program, Project 1: Password Strength Checker, and independently extended with entropy-based scoring and a web interface.
