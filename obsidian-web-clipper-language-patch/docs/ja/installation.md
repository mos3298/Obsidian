# 導入と設定移行

[English](../en/installation.md) | 日本語 · [README](../../README.ja.md)

## 必要なもの

- Git
- Node.jsとnpm（検証環境: Node.js 24.15.0 / npm 11.12.1）
- Chrome

以下のコマンドはPowerShellでも実行できます。PowerShellでnpm.ps1の実行が制限される場合は `npm` を `npm.cmd`、`npx` を `npx.cmd` に置き換えてください。

## 1. パッチと公式ソースを取得

作業用フォルダーで次を実行します。同名フォルダーが既にある場合は、その内容を確認してから別の作業場所を選んでください。

```sh
git clone https://github.com/mos3298/Obsidian.git
git clone https://github.com/obsidianmd/obsidian-clipper.git obsidian-clipper-local
cd obsidian-clipper-local
git checkout --detach 6d56d618b00bd970aa738d6a7a61edee27783e81
git switch -c local/browser-language-v0.1
```

## 2. パッチを適用

```sh
git apply --check ../Obsidian/obsidian-web-clipper-language-patch/patches/preferred-language.patch
git apply ../Obsidian/obsidian-web-clipper-language-patch/patches/preferred-language.patch
git diff --check
```

`git apply --check` が失敗した場合は、以降へ進まず、公式commitとファイルの変更状況を確認してください。無理に適用する必要はありません。

## 3. テストとビルド

```sh
npm ci
npx vitest run src/utils/preferred-language.test.ts
npm run build:chrome
```

成功すると `obsidian-clipper-local/dist` が生成されます。全ブラウザー向けにビルドする場合は `npm run build` を使います。

`npm test` で全テストも実行できます。検証時のWindows環境では変更前から失敗する6件がありました。詳しくは [testing.md](testing.md) を参照してください。

## 4. Chromeに読み込む

1. 既存の公式Clipperの「設定 → 一般 → すべての設定をエクスポート」で設定を保存します。
2. Chromeのアドレス欄に `chrome://extensions` と入力します。
3. 公式Web Clipperを一時的にOFFにします。削除は不要です。
4. デベロッパーモードをONにします。
5. 「パッケージ化されていない拡張機能を読み込む」で、生成された **`obsidian-clipper-local/dist`** を選びます。パッチのフォルダーやZIPは選びません。
6. ローカル版のカードが追加され、有効になったことを確認します。

ソース内の表示名・バージョンを変更していないため、公式版と同じ「Obsidian Web Clipper」「1.7.1」と表示されます。「パッケージ化されていない拡張機能」の表示や読み込み元のパスで区別してください。

## 5. 設定を移行

1. 対象ページを再読み込みし、ローカル版Clipperの設定を開きます。
2. 「一般 → すべての設定をインポート」で、公式版から保存したJSONを選びます。
3. 上書き確認後、テンプレートと保存先を確認します。

インポートはローカル版の現在の設定を置き換えます。ローカル版で既に設定を作った場合は先にエクスポートしてください。過去のハイライトは設定とは別のエクスポート／インポート機能で扱います。

エクスポートした設定JSONは個人用バックアップとして管理してください。APIキー等を含む可能性があるため、このリポジトリのIssueや公開ファイルへ添付しないでください。

## 6. 日本語字幕を確認

1. Chromeの優先言語を日本語にします。このパッチはYouTube画面の字幕選択ではなく、`navigator.language` を優先します。
2. YouTubeタブを再読み込みします。
3. テンプレートの本文へ `{{transcript}}` を含めます。
4. 動画をClipし、実際に保存された本文の言語を確認します。

字幕が取得できない場合や、英語のままの場合は [検証結果と制限](testing.md) を参照してください。

## 公式版へ戻す

Chromeの拡張機能一覧でローカル版をOFF、公式版をONにし、対象ページを再読み込みします。ローカル版で設定を変更した場合は、必要に応じてエクスポートし公式版へ移します。
