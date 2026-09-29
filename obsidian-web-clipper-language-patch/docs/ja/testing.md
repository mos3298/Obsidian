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

## 未検証・制限

- 英語字幕のみ／日本語字幕のみの実動画での回帰確認。
- 実Chromeでの一般記事・Reader・Highlightの一連の操作。
- 実Firefox／Safariでの実行（ビルドのみ確認）。
- 上記commit以外への適用。
- YouTubeの応答や仕様変更による字幕取得失敗。

日本語固定や翻訳機能はありません。希望言語の字幕がない場合は、Defuddleが利用できる字幕を選びます。字幕取得自体が失敗すれば字幕が空になる可能性があります。通常Clipには既存の8秒の非同期抽出タイムアウトもあります。

不具合を報告する場合は、適用した公式commit、ブラウザーのバージョンと言語、公開動画のURL、期待した言語と実際の結果を記載してください。個人設定JSONや秘密情報は添付しないでください。
