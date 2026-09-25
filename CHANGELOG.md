# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- A portable Agent Plugins 1.0 manifest, `plugin.json` at the repository root, which Copilot and Codex read before any host-specific manifest. The host manifests stay for Claude Code, Cursor and older Copilot and Codex, with the same name, version and description.

### Changed
- **Renamed the skill `be-precise` → `urd`** as part of unifying the Yggdrasil family (the world-tree + its keepers). Updated the skill identifier (`skills/urd/SKILL.md` `name:`), the skill directory, `plugin.json`/`marketplace.json` names, and the install command to `urd@urd-marketplace`; rebranded `README.md` into the family's shared style (the root-well / source-of-truth metaphor) with a "Part of the Yggdrasil family" block; fixed the install activation to `/reload-plugins` (the previously documented `/plugin reload` is not a real command). No behavioral change to the skill body (only `name:`, the H1 title, and one self-reference changed). The GitHub repository is renamed `BePreciseSkill` → `UrdSkill`; the old name redirects.

## [0.2.0] - 2026-05-30

### Added
- New Core Rule **"Distinguish verified from believed"** (`skills/be-precise/SKILL.md`). Closes the gap where a checkable-but-unverified inference (a cause, a mechanism, how an API behaves) gets stated as fact: state as fact only what you checked against the source, label or verify everything else, and when challenged re-verify at the source rather than defend an unconfirmed position.
- Two **Common Mistakes** entries mirroring the rule as failure modes: "Stating an unverified inference as fact" and "Defending a challenged claim instead of re-checking".

## [0.1.0] - 2026-05-13

### Added
- Initial release of the `be-precise` skill (`skills/be-precise/SKILL.md`). Pushes the agent toward asking rather than guessing whenever the spec is silent on a hit case, contradicts itself, or tempts a workaround. Five core rules, an Ask/Proceed table, and an explicit list of red-flag thoughts.
- Claude Code plugin scaffolding: `.claude-plugin/plugin.json` (manifest) and `.claude-plugin/marketplace.json` (single-plugin marketplace listing). Installable via `/plugin marketplace add krzysztofdudek/BePreciseSkill` then `/plugin install be-precise@be-precise-marketplace`. Single-file drop-in works for any agent that reads markdown skills.
- MIT license, README, CLAUDE.md with versioning workflow.

[Unreleased]: https://github.com/krzysztofdudek/UrdSkill/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/krzysztofdudek/UrdSkill/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/krzysztofdudek/UrdSkill/releases/tag/v0.1.0
