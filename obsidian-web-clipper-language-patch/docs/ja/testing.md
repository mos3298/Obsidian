# 検証結果と制限

[English](../en/testing.md) | 日本語 · [README](../../README.ja.md)

検証日: 2026-09-29。ソースの対応commitはREADMEに固定しています。

## 環境

| 項目 | 値 |
|---|---|
| OS | Windows 11 / build 26200 |
| Chrome | 153.0.8010.53（インストール済み実行ファイルの製品バージョン） |
| Web Clipper | 1.7.1 / `6d56d618b00bd970aa738d6a7a61edee27783e81` |
| Defuddle | 0.19.2 |
| Node.js / npm | 24.15.0 / 11.12.1 |

## 実装範囲

| 変更ファイル | 内容 |
|---|---|
| `src/content.ts` | 通常Clip、1か所 |
| `src/utils/clip-utils.ts` | 通常／Readerからの同期抽出、2か所 |
| `src/utils/reader.ts` | Reader抽出、1か所 |
| `src/core/reader-view.ts` | Reader初回／URL移動、2か所 |
| `src/core/highlights.ts` | 保存ページの抽出、1か所 |
| `src/utils/preferred-language.ts` | 言語選択の共通関数 |
| `src/utils/preferred-language.test.ts` | 自動テスト9件 |

API／CLIとそのテスト内のDefuddle生成も検索で確認しましたが、ブラウザー拡張の対象外のため変更していません。

## 自動検証

| 検証 | 結果 |
|---|---|
| ブラウザー言語の優先、ページ言語へのfallback、言語情報なし | PASS |
| 模擬en-US/ja-JP字幕・言語未指定 | 英語抽出 PASS |
| 同じ模擬字幕・ja-JP指定／ja指定 | 日本語抽出 PASS |
| 模擬英語のみ・日本語優先指定 | 英語抽出 PASS |
| 模擬日本語のみ | 日本語抽出 PASS |
| 模擬一般記事のtitle・author・content・Markdown | PASS |
| 追加テスト合計 | 9 / 9 PASS |
| `tsc --noEmit --module ES2020`（ビルドと同じmodule指定） | PASS |
| `git diff --check` / 元commitへの `git apply --check` | PASS |
| `npm run build` | Chromium / Firefox / SafariすべてPASS |

字幕テストは実際のDefuddleの `parseAsync` を使用し、ネットワーク応答を模擬しています。YouTubeの実APIや実ChromeのClip保存を検証するテストではありません。ビルドにはサイズに関する警告が各3件ありました。

全テストは **223件中217成功・6失敗**。既存のtemplate-integrationテスト6件が失敗し、変更前commitの別worktreeでも同じ6件が失敗しました。期待値と実行環境のタイムゾーン・改行の差が含まれます。今回の変更に伴う新規失敗は観測していません。期待値の書換えや既存テストの除外は行っていません。

通常の `tsc --noEmit` は、既存tsconfigのmodule=es6とCLI等のdynamic importが整合しないため失敗します。Webpackはmodule=ES2020を指定しています。

## 既存の6件の失敗を照合する

失敗したのは、すべて `src/utils/template-integration.test.ts` の `Template fixtures` にある次の6件です。

| フィクスチャ名 | 変更前での観測 | パッチ適用後での観測 |
|---|---|---|
| `edge-cases` | FAIL | FAIL |
| `goodreads` | FAIL | FAIL |
| `imdb` | FAIL | FAIL |
| `minimal` | FAIL | FAIL |
| `schema-rich` | FAIL | FAIL |
| `youtube` | FAIL | FAIL |

例えば `minimal` では、生成結果のLF改行（`\n`）と期待値ファイルのCRLF改行（`\r\n`）に差がありました。`youtube` では日時にも差があり、期待値は `2025-01-15T04:00:00-08:00`、実際は `2025-01-15T21:00:00+09:00` でした。これは同じ時刻を異なるタイムゾーンで表したものです。

自分の環境で比較する場合は、導入手順のパッチ適用済み `obsidian-clipper-local` ディレクトリから実行します。隣の `obsidian-clipper-baseline` ディレクトリが存在しない状態で始めてください。

```sh
git worktree add --detach ../obsidian-clipper-baseline 6d56d618b00bd970aa738d6a7a61edee27783e81
cd ../obsidian-clipper-baseline
npm ci
npx vitest run src/utils/template-integration.test.ts
cd ../obsidian-clipper-local
npx vitest run src/utils/template-integration.test.ts
```

検証時はWindows、タイムゾーンAsia/Tokyo、期待値ファイルはCRLFでした。変更前でこのファイルを実行した結果は6件失敗・5件成功（計11件）です。改行やタイムゾーンによっては全件成功する環境もあるため、6件失敗させるために環境を変更する必要はありません。同じ環境で変更前・変更後のテスト名と差分を照合してください。失敗数が6件というだけで、新しい失敗を既存のものと判断しないでください。

追加テスト9件は、パッチ適用済みディレクトリで `npx vitest run src/utils/preferred-language.test.ts` を実行します。PowerShellスクリプトの実行が制限される場合は `npm.cmd`、`npx.cmd` を使用してください。

## Chromeでの手動確認

2026-09-29、パッチ版で次の結果を確認しました。

| ケース | 確認できた結果 |
|---|---|
| 英語・日本語の字幕がある動画 | Clipperのプレビューに日本語のTranscriptを表示 |
| 日本語字幕のみの動画 | Clipperのプレビューに日本語のTranscriptを表示 |
| 英語字幕のみの動画 | Clipperのプレビューに英語のTranscriptを表示 |
| 通常のWeb記事 | 抽出してObsidianへ保存。表示範囲でタイトル・画像・箇条書き・見出し・本文に大きな崩れなし |

動画の結果は抽出・プレビューの確認です。通常記事は保存後のノート表示まで確認しています。YouTube側の自動翻訳表示を取り込む機能は今回のパッチには含まれません。

## 未検証・制限

- 動画Transcriptの保存後の本文確認。
- 実ChromeでのReader・Highlight経由の保存。
- 実Firefox／Safariでの実行（ビルドのみ確認）。
- 上記commit以外への適用。
- YouTubeの応答や仕様変更による字幕取得失敗。

日本語固定や翻訳機能はありません。希望言語の字幕がない場合は、Defuddleが利用できる字幕を選びます。字幕取得自体が失敗すれば字幕が空になる可能性があります。通常Clipには既存の8秒の非同期抽出タイムアウトもあります。

不具合を報告する場合は、適用した公式commit、ブラウザーのバージョンと言語、公開動画のURL、期待した言語と実際の結果を記載してください。個人設定JSONや秘密情報は添付しないでください。
