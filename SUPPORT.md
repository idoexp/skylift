# Getting help with Skylift

## Quick troubleshooting

Before opening an issue, please check:

1. **README**: the [main README](README.md) has a [Troubleshooting](README.md#troubleshooting) section covering the most frequent problems (connection refused, "all files showing as modified", slow push, git not found).
2. **Logs**: open the Output panel (`Ctrl+Shift+U`) and pick **Skylift** in the dropdown. Detailed activity is logged there. Settings → Debug logs to enable verbose output.
3. **Test connection**: from the sidebar, the **Test connection** action gives you a focused error message and is the fastest way to confirm whether the issue is auth, network, or something else.

## Bug reports & feature requests

Open an issue: <https://github.com/idoexp/skylift/issues/new/choose>

Pick the right template (Bug / Feature request) and please include:

- VS Code version (`Help → About` or `code --version`)
- Skylift version (`Extensions → Skylift → ⓘ`, or check `package.json` of the installed extension)
- OS (Windows 10/11, macOS, Linux distribution)
- A short description of what you expected vs. what happened
- Relevant logs from the Skylift Output channel (with `Debug logs` enabled if possible)
- For sync issues: whether you use git, whether you're solo or in a team, whether the problem reproduces after a fresh `Skylift: Rebuild manifest from scratch`

## Translation contributions

Want to translate Skylift to your language or improve an existing translation? Open an issue with the language code (e.g. `it`, `pl`, `ja`) and the translated strings — we'll fold them into the next release.

## Security issues

If you find a security vulnerability (credentials leaking, path traversal, code execution from a remote source, etc.), **please don't open a public issue**. Email the maintainer privately via the email listed on [the maintainer's GitHub profile](https://github.com/idoexp). We'll acknowledge within 72 hours and coordinate a fix before disclosure.

## Marketplace listing

The Marketplace page is the primary distribution channel: <https://marketplace.visualstudio.com/items?itemName=IdoExperiences.skylift>

You can rate/review there. Reviews help discoverability significantly — even a quick "works as expected" comment is valuable feedback.
