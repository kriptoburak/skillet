# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com); versions follow semver.

## [Unreleased]

## [0.1.1] - 2026-06-11
### Fixed
- The demo GIF and the 中文 README link now render on the npm package page
  (absolute URLs instead of repo-relative ones).

## [0.1.0] - 2026-06-11

First public release.

### Added
- Install skills from a registry name, any `owner/repo[/path][#ref]`, or a
  local folder — copied into `.claude/skills/` (shadcn-style: files you can
  read and edit, not an opaque dependency).
- Reproducible installs: every install pins the commit SHA in
  `skillet.lock.json`; `skillet install` restores the exact set (the `npm ci`
  of skills). One broken lockfile entry skips with a report instead of
  aborting the rest.
- Zero-infrastructure registry: a single JSON index in a Git repo, served over
  raw GitHub. Seeded with 26 verified skills (all of Anthropic's + 9
  first-party).
- Authoring: `skillet new` scaffold and `skillet validate`.
- `skillet gallery` — a static, searchable registry page, auto-deployed to
  GitHub Pages on registry changes.
- MCP server (`skillet mcp`): `skillet_search` / `skillet_install`
  (registry-only, name-validated) / `skillet_list`.
- Security: path-traversal guard on skill names, even with `--force`.

[Unreleased]: https://github.com/jnMetaCode/skillet/compare/v0.1.1...HEAD
[0.1.1]: https://github.com/jnMetaCode/skillet/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/jnMetaCode/skillet/releases/tag/v0.1.0
