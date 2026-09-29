# Installation and settings migration

English | [日本語](../ja/installation.md) · [README](../../README.md)

## Requirements

- Git
- Node.js and npm (tested with Node.js 24.15.0 and npm 11.12.1)
- Chrome

The commands below also work in PowerShell. If PowerShell blocks `npm.ps1`, use `npm.cmd` instead of `npm`, and `npx.cmd` instead of `npx`.

## 1. Get the patch and upstream source

Run these commands in a working directory. If directories with these names already exist, inspect them and choose a different working location before proceeding.

```sh
git clone https://github.com/mos3298/Obsidian.git
git clone https://github.com/obsidianmd/obsidian-clipper.git obsidian-clipper-local
cd obsidian-clipper-local
git checkout --detach 6d56d618b00bd970aa738d6a7a61edee27783e81
git switch -c local/browser-language-v0.1
```

## 2. Apply the patch

```sh
git apply --check ../Obsidian/obsidian-web-clipper-language-patch/patches/preferred-language.patch
git apply ../Obsidian/obsidian-web-clipper-language-patch/patches/preferred-language.patch
git diff --check
```

If `git apply --check` fails, stop and check the upstream commit and local changes. Do not force the patch onto incompatible source.

## 3. Test and build

```sh
npm ci
npx vitest run src/utils/preferred-language.test.ts
npm run build:chrome
```

A successful build creates `obsidian-clipper-local/dist`. Use `npm run build` to build all browser variants.

You can also run the full suite with `npm test`. Six tests already failed on the unmodified upstream commit in the tested Windows environment; see [test results](testing.md).

## 4. Load into Chrome

1. In the official extension, open **Settings → General → Export all settings** and save the settings file.
2. Enter `chrome://extensions` in Chrome's address bar.
3. Temporarily disable the official Web Clipper. You do not need to remove it.
4. Enable **Developer mode**.
5. Select **Load unpacked** and choose the generated **`obsidian-clipper-local/dist`** directory, not the patch directory or a ZIP file.
6. Confirm that the local extension appears and is enabled.

The patch does not change the source display name or version, so the local build appears as “Obsidian Web Clipper” version “1.7.1”, just like upstream. Distinguish it by its unpacked-extension status or source path.

## 5. Migrate settings

1. Reload the target page and open the local extension's settings.
2. Under **General → Import all settings**, select the JSON exported from the official extension.
3. Confirm replacement, then check your templates and destination vault.

Importing replaces the local extension's current settings. Export those first if you have already customized the local extension. Previously saved highlights use a separate export/import feature.

Keep exported settings JSON as a private backup. It may contain API keys; do not attach it to public issues or commit it to this repository.

## 6. Check Japanese transcripts

1. Set Chrome's preferred language to Japanese. The patch prioritizes `navigator.language`, rather than the caption selection displayed on YouTube.
2. Reload the YouTube tab.
3. Include `{{transcript}}` in the template body.
4. Clip the video and check the language of the saved text.

For missing transcripts or an unexpected language, see [test results and limitations](testing.md).

## Return to the official extension

Disable the local extension, enable the official extension, and reload the target page. If you changed settings in the local extension, export and import them back into the official extension as needed.
