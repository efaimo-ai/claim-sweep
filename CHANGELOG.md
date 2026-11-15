# Changelog

All notable changes to claim-sweep are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/); this project uses
[Semantic Versioning](https://semver.org/).

## [Unreleased]

> **Not published yet.** This entry describes what is on `main` and is
> dated when it ships. The first publish of `claim-sweep` cannot carry provenance:
> npm's trusted publishing is configured from an existing package's settings
> page, so it cannot create a package that has never been published, and the
> authenticated session that can create one cannot mint an OIDC attestation.
> From the version after that the release workflow owns it and every release
> carries SLSA provenance naming the commit that built it. Install it from the
> repository in the meantime; the README carries the command.

### Added

- **The skill is installable.** `npx -y --allow-git=all github:efaimo-ai/claim-sweep` copies `SKILL.md` and its
  `references/` into `./.claude/skills/claim-sweep/`, with `--global`, `--dir`,
  `--check`, `--print`, `--uninstall` and `--force`. The installer reads every
  byte back after writing it and fails if what landed is not what it wrote, and
  it prints how many files it installed rather than only that it succeeded.
- Three exit codes rather than two: 0 installed or already identical, 1 present
  and different, 2 could not run at all. "It differs" and "I could not do this"
  are different answers and collapsing them is how a broken install reads as a
  clean one.
- CI on four cells (Ubuntu and Windows, Node 20 and 24), including an end to end
  run that packs the package the way npm would, installs it from that tarball,
  and installs the skill from there.

### Changed

- The description fires after a project fact changed, when two project
  documents disagree, or before a rename, migration or domain move. It no
  longer fires on "is this still true" about the world, on a dependency bump,
  or on every release, which is `unreleased-guard`'s moment.
- Step 1's example is a plain price limit instead of a grade only this set's
  own tooling prints.
- Step 6 proposes a guard and adds it when the user wants one, instead of
  installing CI checks nobody asked for.
- New "Report" section: one line per carrier category, and the same list handed
  to the user when there is no shell or repository to search.
- "The value that was never named" moved to `references/never-named.md`, with a
  short pointer left in `SKILL.md`.

### Notes

- The package version and the version in `SKILL.md` frontmatter are one number,
  pinned by a test.

[Unreleased]: https://github.com/efaimo-ai/claim-sweep/commits/main
