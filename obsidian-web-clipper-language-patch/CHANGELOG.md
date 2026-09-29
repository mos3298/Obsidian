# Changelog

English | [日本語](CHANGELOG.ja.md)

## Manual verification update — 2026-09-29

- Documented Chrome transcript previews for bilingual, Japanese-only, and English-only caption tracks, and saving a regular article to Obsidian.
- Updated the remaining unverified cases. The patch and v0.1 tag are unchanged.

## Documentation update — 2026-09-29

- Added English and Japanese READMEs and guides with language-switching links.
- Kept links at the original guide paths to help existing readers find the translated guides.
- Clarified how to check the selected browser language and compare the six existing test failures.
- Pinned v0.1 with the `web-clipper-language-patch-v0.1` tag.
- No changes to the v0.1 patch or its supported upstream commit.

## v0.1 — 2026-09-29

- Initial patch for Web Clipper 1.7.1 / commit `6d56d618b00bd970aa738d6a7a61edee27783e81`.
- Pass the preferred language to seven Defuddle construction sites in the extension.
- Add nine automated tests for language selection, transcript extraction, and regular articles.
- Publish installation, settings migration, verification, and maintenance documentation.
- Distribute only the patch and documentation.
