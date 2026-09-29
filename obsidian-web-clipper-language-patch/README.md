# Obsidian Web Clipper — Browser Language Patch

YouTube字幕の抽出時に、ブラウザーの優先言語をDefuddleへ渡すための非公式パッチです。

日本語環境で日本語動画をClipしたとき、英語字幕が選ばれる問題を改善することを目的としています。**日本語固定ではありません。** Chromeの言語が日本語なら日本語字幕を優先し、対応する字幕がない場合はDefuddle本来の選択処理へ戻ります。翻訳は行いません。

このパッケージは **パッチとドキュメントのみ** を公開します。公式ソース全体、ビルド済み拡張、公式アイコン、個人設定ファイルは含みません。Obsidian公式による提供・承認を示すものではありません。

## 対応バージョン

| 項目 | 検証対象 |
|---|---|
| パッチ | v0.1 |
| 公式Web Clipper | 1.7.1 |
| 公式commit | [`6d56d618b00bd970aa738d6a7a61edee27783e81`](https://github.com/obsidianmd/obsidian-clipper/commit/6d56d618b00bd970aa738d6a7a61edee27783e81) |
| Defuddle | 0.19.2 |

**上記commitへの適用を確認しています。公式の最新mainへの適用・動作は保証しません。**

## 使い方

1. このリポジトリと公式ソースを取得する。
2. 公式ソースを対応commitに合わせ、[パッチ](patches/preferred-language.patch)を適用する。
3. 自分の環境でビルドし、生成した `dist` をChromeに読み込む。

コマンド、Chromeへの導入、既存設定の移行は **[導入手順](docs/installation.md)** にまとめています。

## 内容

```text
obsidian-web-clipper-language-patch/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── patches/
│   └── preferred-language.patch
└── docs/
    ├── installation.md
    ├── testing.md
    └── maintenance.md
```

拡張内の7か所で、次の優先順に決めた言語をDefuddleへ渡します。

1. `navigator.language`
2. 抽出対象の `document.documentElement.lang`
3. `undefined`（Defuddleの標準動作）

変更範囲は通常Clip、Reader、保存ページのHighlight関連抽出です。API／CLIは変更しません。字幕選択UI、独自の字幕取得ロジック、AI翻訳は追加しません。

## 検証状況

- Chromium / Firefox / Safariのビルド成功。
- 追加した自動テスト9件成功。
- Windows / Chromeで日本語字幕が読めたこと、公式版から設定を移行できたことを利用者が報告。
- 全自動テストは217件成功・6件失敗。6件は変更前でも再現する既存テストの失敗。
- 英語のみの実動画、実ブラウザーのReader／Highlightなど、未検証の項目があります。

詳細は **[検証結果と制限](docs/testing.md)**、更新・復帰・公式への提案は **[メンテナンス手順](docs/maintenance.md)** を参照してください。

## 関連リンクとライセンス

- [公式Obsidian Web Clipper](https://github.com/obsidianmd/obsidian-clipper)
- [関連する公式Issue #957](https://github.com/obsidianmd/obsidian-clipper/issues/957)
- [公式の開発・ビルド手順（対応commit）](https://github.com/obsidianmd/obsidian-clipper/blob/6d56d618b00bd970aa738d6a7a61edee27783e81/README.md)

パッチとドキュメントはMITライセンスです。元コードの著作権表示・許諾文を [LICENSE](LICENSE) に保持しています。Obsidianの商標や公式アイコン等の権利は各権利者に帰属します。このリポジトリはそれらの利用許諾を与えるものではありません。
