# Changelog

All notable changes to this project will be documented in this file.
Format: [Keep a Changelog](https://keepachangelog.com/) · [Semantic Versioning](https://semver.org/)

## [1.0.0] — 2026-03-05

Initial release as an OpenClaw Skill template.

### Added

- `SKILL.md` — annotated template with all frontmatter fields (name, description, emoji, requires.bins, requires.env, install methods)
- `examples/hello-world/SKILL.md` — minimal skill with no dependencies
- `examples/api-connector/SKILL.md` — skill requiring an env var and a binary, with multiple install methods
- `README.md` — skill format reference, local install instructions, ClawHub publish guide
- `.github/workflows/ci.yml` — validates SKILL.md frontmatter on push/PR
