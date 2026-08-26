# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

This file is not edited by hand. Every change writes its own fragment under
`.changes/unreleased/` with [chlog](https://github.com/luizjhonata/chlog), and a release compiles
the pending fragments into a version section here — so two branches each adding an entry no
longer touch the same lines, and a rebase that used to conflict on this file now conflicts on
nothing.

When a new release is proposed:

1. Create a new branch `bump/x.x.x` (this isn't a long-lived branch!!!);
2. The fragments pending under `.changes/unreleased/` are compiled into a version section by `chlog batch auto && chlog merge` (AutoBump does this for you — it reads the fragments directly);
3. Open a Pull Request with the bump version changes targeting the `main` branch;
4. When the Pull Request is merged, the [`release.yaml`](.github/workflows/release.yaml) workflow creates the Git tag and the GitHub release automatically from the version in the bump commit.

Releases to productive environments should run from a tagged version.
Exceptions are acceptable depending on the circumstances (critical bug fixes that can be cherry-picked, etc.).

## [Unreleased]

## [0.2.0] - 2026-06-30

### Added

- created `CLAUDE.md` to provide Claude Code guidance covering the character-by-character HTTP parser, H-bridge motor pin mapping, and the absence of a build/test system

## [0.1.2] - 2026-05-19

### Changed

- refreshed `.github/copilot-instructions.md` to document the release workflow and include it in the repository structure

## [0.1.1] - 2026-04-28

### Changed

- refreshed `.github/copilot-instructions.md` to include `CHANGELOG.md` in the repository structure

## [0.1.0] - 2026-03-24

The changes weren't tracked until this version.
