# Changelog

All notable changes to the Skylift extension are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.5] - 2026-05-07

### Fixed

- **Push current file via right-click in Explorer**: the command now uses the URI of the right-clicked file instead of the active editor's file. Previously, right-clicking `fileB` in the Explorer while `fileA` was open in the editor would push `fileA`'s content to `fileA`'s remote path, leaving `fileB` untouched.
- **Push current file with unsaved edits**: the document is now saved to disk before upload. Previously, `fastPut` would ship the stale on-disk version (often identical to what's already on the server) and `setMtime` would still update the remote mtime - making the operation appear to "succeed" while the content didn't actually change.

## [0.3.4] - 2026-05-07

### Added

- **Review auto-scan** (Settings → Review auto-scan, off by default). Runs a Review scan in the background at a configurable cadence (1 min to 1 hour). Keeps the activity-bar badge fresh and lets Review & push open instantly with cached results (a "last scan X min ago" banner with a manual refresh button is shown at the top of the panel). Paused while a Push is running, in untrusted workspaces, and when no active target is set. Diagnose anomalies is unaffected — it always scans on open.
- **Live counter updates**: after applying actions in Review & push or Diagnose anomalies, the activity-bar badge updates in real time without waiting for the next scan.

### Changed

- **Friendlier network error**: "host unreachable" / DNS errors now surface a clear, localized message ("Cannot reach the host - check the server address or your network connection.") instead of the raw Node.js error code.
- **Settings UI cleanup**: removed the floating green bell badge that used to overlay alert-enabled chips. The chip's bell icon now has a green background that conveys the same state.

### Fixed

- **Activity-bar badge after cold start**: the upload count computed by the background auto-scan now applies immediately the moment you open the Skylift sidebar (previously the badge stayed empty until the next tick because the webview view wasn't yet resolved).

## [0.3.2] - 2026-05-05

### Changed

- **Marketplace listing**: shipped the polished, marketing-grade README (English + French) directly inside the `.vsix`, with absolute URLs to screenshots hosted on the public GitHub repo. Earlier versions shipped a minimal placeholder README, which led to a broken hero image on the marketplace listing.
- **Diagnose anomalies**: reframed all user-visible strings around the primary purpose — production audit. The sidebar card subtitle, panel subtitle, source badges (`Critical leak`, `Orphan on server`) and folder card title now lead with security/audit framing instead of generic "find extra remote files".

## [0.3.0] - 2026-05-05

### Changed

- **Smart Push**: Push code now walks both local AND remote on every run (using the dir-mtime cache to skip stable subtrees - typically near-zero round-trips on steady-state pushes). Diff is local↔remote, not local↔manifest. Result: accurate regardless of git pulls, multi-dev mtime drift, or out-of-band server edits. The manifest is now purely a cache, no longer source of truth.

### Added

- **Trust same-size** preference (default ON, in Settings → Preferences). When local and remote sizes match, treat the file as in sync even if mtimes drifted. Mirrors `rclone`/`rsync` default. Eliminates re-uploads after `git pull`.
- **Skylift: Reset mtimes from git** command (palette + Settings → Maintenance tools). Sets each tracked file's local mtime to its last git-commit date. Files with uncommitted changes are preserved. Optional - only needed if you want to keep the manifest stable across machines on the same git history. Requires git on PATH; surfaces a clear error otherwise.
- **Skylift: Verify against remote (deep check)** command (palette + Settings → Maintenance tools). Manual content-hash verification: hashes every same-size pair on both sides and reports mismatches. Slow (downloads each remote file to compute its hash) but bulletproof. Run on demand when you suspect drift.

### Fixed

- After-upload mtime preservation: every uploaded file's remote mtime is now aligned with the local mtime via SFTP `setstat`/`utimes` (`rsync -t` semantics). Prevents the "remote is now newer" false positive after every push.

## [0.1.0] - 2026-05-02

Initial public release.

### Added

- Activity-bar sidebar with status, target switcher and one-click action cards (Push code, Review & push, Diagnose anomalies, Settings).
- Setup wizard: 4-step flow (connection → test → remote folder browser → local folder) writes to `.skylift/config.json`.
- Multi-target support: switch active target from the sidebar; push to several targets via `Skylift: Push to multiple targets`.
- Push code: uploads only what changed since the last sync, using a per-target manifest (no remote walk needed).
- Review & push: scans both sides and lets you pick what to upload, skip, or always-ignore. Uses a remote-dir cache that skips `readdir` round-trips on subtrees whose mtime hasn't changed.
- Diagnose anomalies: finds files only on the server and offers delete / download / backup / always-ignore actions, plus orphan temp-file cleanup.
- Atomic uploads: write to `.<file>.skylift_tmp.<n>`, then `posix-rename` (or `delete + rename` on servers without OpenSSH extensions).
- SSH key auth: auto-detects keys in `~/.ssh/`, supports passphrases and PuTTY `.ppk` keys (converted on the fly).
- Password auth: secrets live in the OS keychain via VS Code `SecretStorage`, never in the repo.
- Conflict detection (opt-in): stat each remote file before upload and warn if it changed since the last sync.
- Auto-upload on save (opt-in): pushes the current file every time you save it. Disabled in untrusted workspaces.
- Git push trigger: auto-runs Push code when you commit on a branch you've watched (`gitPushOnCommitBranches`).
- Settings panel: project-scoped preferences live in `.skylift/config.json` (versionable with the rest of your config). Manages exclusion patterns, push behavior toggles, git trigger branches and Skylift's UI language.
- Language override: pick a UI language for Skylift independent of VS Code's display language. Bundled translations: French, Spanish, German, Italian, Brazilian Portuguese.
- Compare with remote: right-click any file → Skylift: Compare with remote → side-by-side diff in VS Code.
- Push current file: keyboard shortcut `Ctrl+Alt+P` (`Cmd+Alt+P` on macOS) or right-click in editor / explorer.
- Export / import config: copy a target setup between machines without re-entering credentials.
- Workspace trust: declared as `limited` - auto-upload-on-save is gated, manual commands always work.
