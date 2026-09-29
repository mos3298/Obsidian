# Obsidian

[English](README.md) | 日本語

Obsidian関連の非公式パッチ、ツール、ドキュメントをまとめるリポジトリです。各プロジェクトは独立したフォルダーで管理します。Obsidian公式による提供・承認を示すものではありません。

## プロジェクト

| フォルダー | 内容 | 配布形式 |
|---|---|---|
| [obsidian-web-clipper-language-patch](obsidian-web-clipper-language-patch/README.ja.md) | Web ClipperでYouTube字幕の抽出時にブラウザー言語を優先する修正 | パッチ＋ドキュメント |

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

今後のプロジェクトも、このリポジトリ直下に個別フォルダーとして追加します。各フォルダーのREADMEに目的・導入方法・対応バージョンを、LICENSEにライセンスを記載します。ライセンスはプロジェクトごとに確認してください。

個人のVault、エクスポートした設定JSON、APIキー等は公開対象に含めません。

英語を標準の入口とし、日本語版へは言語リンクで切り替えます。更新時は、両言語のコマンド・対応commit・検証状況を揃えてください。
