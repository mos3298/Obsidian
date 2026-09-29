# Obsidian Web Clipper — Browser Language Patch

English | [日本語](README.ja.md)

An unofficial patch that passes the browser's preferred language to Defuddle when extracting YouTube transcripts.

It addresses cases where clipping a Japanese video produces an English transcript. **The patch is not limited to Japanese.** When the browser language is Japanese, it prefers a matching Japanese caption track. If none is available, Defuddle uses its existing fallback behavior. The patch does not translate text.

This package distributes **only a patch and documentation**. It does not include the full upstream source, a built extension, official icons, or personal settings. It is not provided or endorsed by Obsidian.

## Supported version

| Component | Tested version |
|---|---|
| Patch | v0.1 |
| Patch repository tag | [`web-clipper-language-patch-v0.1`](https://github.com/mos3298/Obsidian/tree/web-clipper-language-patch-v0.1/obsidian-web-clipper-language-patch) |
| Upstream Web Clipper | 1.7.1 |
| Upstream commit | [`6d56d618b00bd970aa738d6a7a61edee27783e81`](https://github.com/obsidianmd/obsidian-clipper/commit/6d56d618b00bd970aa738d6a7a61edee27783e81) |
| Defuddle | 0.19.2 |

**The patch was checked against this commit. Compatibility with the latest upstream main is not guaranteed.**

## Getting started

1. Clone this repository and the upstream source.
2. Check out the supported upstream commit and apply the [patch](patches/preferred-language.patch).
3. Build locally and load the generated `dist` directory into Chrome.

See the **[installation guide](docs/en/installation.md)** for commands, Chrome setup, and settings migration.

## Contents

```text
obsidian-web-clipper-language-patch/
├── README.md
├── README.ja.md
├── LICENSE
├── CHANGELOG.md
├── CHANGELOG.ja.md
├── patches/
│   └── preferred-language.patch
└── docs/
    ├── en/
    │   ├── installation.md
    │   ├── testing.md
    │   └── maintenance.md
    └── ja/
        ├── installation.md
        ├── testing.md
        └── maintenance.md
```

The patch passes a language to seven Defuddle construction sites in the extension, using this priority:

1. `navigator.language`
2. The source document's `document.documentElement.lang`
3. `undefined` (Defuddle's default behavior)

It covers regular clipping, Reader, and extraction of saved pages associated with highlights. The API and CLI are unchanged. It adds no caption-selection UI, custom caption-fetching logic, or AI translation.

## Verification status

- Chromium, Firefox, and Safari builds succeeded.
- All nine added automated tests passed.
- The full automated suite had 217 passes and six failures. The same six existing failures were reproduced on the unmodified upstream commit.
- Manual checks in patched Chrome covered extraction and preview for videos with both English and Japanese tracks, Japanese-only tracks, and English-only tracks; a regular article was also saved to Obsidian.
- Saved video transcripts and saving through Reader/Highlight remain unverified.

See **[test results and limitations](docs/en/testing.md)** and **[maintenance and upstream contributions](docs/en/maintenance.md)**.

## Links and license

- [Upstream Obsidian Web Clipper](https://github.com/obsidianmd/obsidian-clipper)
- [Related upstream issue #957](https://github.com/obsidianmd/obsidian-clipper/issues/957)
- [Upstream development instructions at the supported commit](https://github.com/obsidianmd/obsidian-clipper/blob/6d56d618b00bd970aa738d6a7a61edee27783e81/README.md)

The patch and documentation are distributed under the MIT License. The upstream copyright and permission notice are preserved in [LICENSE](LICENSE). Obsidian trademarks, official icons, and other brand assets belong to their respective owners; this repository does not grant permission to use those assets.
