# Contributing to Skylift

Thanks for considering a contribution! Skylift's source code is currently in a **private** repository, which limits the ways we accept contributions. Here's what you can still do.

## What we accept

### 🐛 Bug reports

Open an issue with the **Bug report** template. Detailed steps to reproduce, expected vs. actual behavior, logs, and your environment (VS Code version, OS, server type) help a lot.

### 💡 Feature requests

Open an issue with the **Feature request** template. Describe the use case, why your current workflow is painful, and what an ideal solution looks like for you. Please check existing open issues first — there might already be a thread.

### 🌍 Translations

Skylift ships with French, Spanish, German, Italian and Brazilian Portuguese. Want to add a language or fix an existing translation?

1. Open an issue titled `[i18n] <language code>` (e.g. `[i18n] it`).
2. List the strings you want changed/added (they're keyed by the source English string).
3. We'll fold the contribution into the next release and credit you in CHANGELOG.

### 📸 Screenshots / GIFs / videos

Got a great-looking screenshot or short demo? Attach it to an issue tagged `screenshots` and we'll feature it in the README.

### ⭐ Stars & reviews

Starring this repo and leaving a Marketplace review are the easiest ways to support the project. Reviews directly impact discoverability for other developers searching for an SFTP extension.

## What we don't accept (yet)

- **Pull requests** to the source code. The source is in a private repository for now. Future open-sourcing is possible — see the [roadmap](README.md#roadmap).
- **Reverse-engineering** of the published `.vsix` to submit a patched fork. The license is MIT (see [LICENSE](LICENSE)) so you may study and modify the bundled JavaScript for personal use, but we won't merge such patches because we'd lose ability to maintain the canonical version.

## Code of conduct

Be kind. Disagree about ideas, not people. Issues with insults, harassment, or aggressive language will be closed without further engagement.

---

Thanks again — even just opening an issue with a clear repro helps Skylift improve for every other developer who's deploying via SFTP at 11pm.
