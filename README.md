# Obsidian

English | [日本語](README.ja.md)

Unofficial patches, tools, and documentation for Obsidian. Each project has its own directory. This repository is not provided or endorsed by Obsidian.

## Projects

| Directory | Purpose | Distribution |
|---|---|---|
| [obsidian-web-clipper-language-patch](obsidian-web-clipper-language-patch/) | Pass the browser language to Defuddle when extracting YouTube transcripts in Web Clipper | Patch and documentation |

## Layout

```text
Obsidian/
├── README.md
├── README.ja.md
└── obsidian-web-clipper-language-patch/
    ├── README.md
    ├── README.ja.md
    ├── LICENSE
    ├── CHANGELOG.md
    ├── CHANGELOG.ja.md
    ├── patches/
    │   └── preferred-language.patch
    └── docs/
        ├── en/
        └── ja/
```

Future projects will be added as separate directories at the repository root. Each project documents its purpose, installation steps, supported versions, and license. Check the LICENSE in each project before using its contents.

English is the default documentation language; Japanese is available through the language links. When updating documentation, keep commands, supported commits, and verification status consistent in both languages.

Personal vaults, exported settings, and API keys are not included.
