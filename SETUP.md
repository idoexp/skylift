# Setup guide — bringing the public repo online

This file walks you through pushing the contents of `public-repo/` to a fresh GitHub repository, configuring it for SEO/AEO, and linking your private source repository to it. Once done, this `SETUP.md` itself can be deleted from the repo (it's a one-time setup doc, not part of the public-facing content).

## Prerequisites

- A GitHub account with permission to create repositories under `idoexp` (or whatever username you use).
- Git installed locally and authenticated against GitHub (HTTPS with token, or SSH).
- The current `public-repo/` directory is what you're going to push as-is.

## Step 1 — Create the two GitHub repositories

### Public repo

1. Go to <https://github.com/new>.
2. **Owner**: `idoexp` (or your account).
3. **Repository name**: `skylift`.
4. **Description**: copy/paste this (it's what shows up under the repo name + on social previews and feeds AEO/AI search):

   > Modern SFTP for VS Code — point-and-click setup, atomic uploads, smart diff, anomaly detection. No JSON to write. Multi-language UI.

5. **Visibility**: Public ✅
6. **Initialize**: leave everything UNCHECKED (no README / .gitignore / license — you bring your own).
7. Click **Create repository**.

### Private source repo

1. Go to <https://github.com/new> again.
2. **Repository name**: `skylift-source` (or any name you want).
3. **Visibility**: Private ✅
4. Initialize: same — leave unchecked.
5. Click **Create repository**.

## Step 2 — Configure the public repo's "About" panel

This is the single most important SEO move on GitHub. The "About" panel feeds Google's snippets and AI engines' summaries.

On the public repo's home page, click the gear icon next to **About** (top right of the file list) and set:

- **Description**: same as above.
- **Website**: `https://marketplace.visualstudio.com/items?itemName=IdoExperiences.skylift`
- **Topics** (these are the tags GitHub uses for discovery — pick 8 to 12 relevant ones):
  - `vscode-extension`
  - `vscode`
  - `sftp`
  - `sftp-client`
  - `ssh`
  - `deploy`
  - `deployment-tool`
  - `upload`
  - `sync`
  - `ftp`
  - `remote-development`
  - `developer-tools`
- **Releases**: ✅ checked
- **Packages**: leave unchecked
- **Deployments**: leave unchecked

Save.

## Step 3 — Push the public repo content

From your local machine, in the project's `public-repo/` directory:

```bash
cd public-repo
git init -b main
git add .
git commit -m "chore: initial public release scaffolding"
git remote add origin https://github.com/idoexp/skylift.git
git push -u origin main
```

Once this push succeeds, your public repo is live with all the docs.

> **Note**: this `SETUP.md` is committed by the command above. After everything is configured and you've verified the repo looks right, you can delete it: `git rm SETUP.md && git commit -m "chore: remove setup doc" && git push`. Or leave it — it's harmless and may help future maintainers.

## Step 4 — Push the private source

From the project root (one level above `public-repo/`):

```bash
git init -b main
echo "public-repo/" >> .gitignore   # don't double-track the public artefacts
git add .
git commit -m "chore: initial source import"
git remote add origin https://github.com/idoexp/skylift-source.git
git push -u origin main
```

The private repo now holds your full Skylift source code.

## Step 5 — Cut the first GitHub release on the public repo

Releases are how users get the `.vsix` outside the marketplace (offline installs, pre-releases, etc.).

1. From your private source repo, build the `.vsix`:

   ```bash
   npx @vscode/vsce package
   ```

   This produces `skylift-X.Y.Z.vsix` (where `X.Y.Z` is the `version` field in `package.json`).

2. On the public repo: <https://github.com/idoexp/skylift/releases/new>
3. Click **Choose a tag** → type `v0.3.0` → **Create new tag: v0.3.0 on publish**.
4. **Release title**: `Skylift v0.3.0`.
5. **Description**: copy the relevant section from `CHANGELOG.md` (the `## [0.3.0]` block).
6. **Attach binaries**: drag the `skylift-0.3.0.vsix` file into the upload area.
7. ✅ Set as the latest release.
8. Click **Publish release**.

## Step 6 — Publish to the VS Code Marketplace

1. From the private source directory, log in once with `vsce`:

   ```bash
   npx @vscode/vsce login IdoExperiences
   ```

   You'll be prompted for an Azure DevOps Personal Access Token (PAT). Generate one at <https://dev.azure.com/_usersSettings/tokens> with `Marketplace → Manage` scope.

2. Publish:

   ```bash
   npx @vscode/vsce publish
   ```

   `vsce` will package and upload. Within ~5 minutes the new version appears on the marketplace.

## Step 7 — Add screenshots / GIFs

The README references `screenshots/sidebar.png`. Until those files exist, the README will show broken-image placeholders.

Recommended captures and tips: see [screenshots/README.md](screenshots/README.md). Once you have them, simply drop them into `public-repo/screenshots/` and push.

A 15-second GIF demo (the wizard + first push) is the single highest-impact addition for both the README hero and the marketplace listing.

## Step 8 — Future releases

Routine flow once everything's bootstrapped:

1. Work on `skylift-source` (private). Bump `version` in `package.json`.
2. Update `CHANGELOG.md` with the new entry. Run tests, smoke-test the .vsix locally.
3. Build: `npx @vscode/vsce package`.
4. **Public repo**: copy the new `CHANGELOG.md` into `public-repo/`, commit, push.
5. **Public repo**: cut a GitHub release with the new tag and attach the `.vsix`.
6. **Marketplace**: `npx @vscode/vsce publish`.

That's it. Steps 4–6 can be automated via a GitHub Action later — for now manual is fine and gives you a final sanity check.

## Optional — GitHub Discussions

If you want a Q&A space for users (separate from issues), enable Discussions: repo Settings → Features → Discussions. The README's Support section already mentions issues; you can add a "Discussions" link there if you turn this on.

## Optional — Branch protection

For the public repo, you probably want main branch protection so a typo in a tag doesn't accidentally rewrite history:

Settings → Branches → Add rule:
- Branch name pattern: `main`
- ✅ Require a pull request before merging
- ✅ Require status checks (none yet, but reserve the option)
- ✅ Do not allow bypassing the above settings

Optional but good hygiene.
