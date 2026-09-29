# Test results and limitations

English | [日本語](../ja/testing.md) · [README](../../README.md)

Verification date: 2026-09-29. The supported upstream commit is pinned in the README.

## Environment

| Component | Value |
|---|---|
| OS | Windows 11 / build 26200 |
| Chrome | 153.0.8010.53 (product version of the installed executable) |
| Web Clipper | 1.7.1 / `6d56d618b00bd970aa738d6a7a61edee27783e81` |
| Defuddle | 0.19.2 |
| Node.js / npm | 24.15.0 / 11.12.1 |

## Implementation scope

| Modified file | Change |
|---|---|
| `src/content.ts` | Regular clipping: one construction site |
| `src/utils/clip-utils.ts` | Synchronous extraction from regular/Reader documents: two sites |
| `src/utils/reader.ts` | Reader extraction: one site |
| `src/core/reader-view.ts` | Initial Reader load and URL navigation: two sites |
| `src/core/highlights.ts` | Saved-page extraction: one site |
| `src/utils/preferred-language.ts` | Shared language helper |
| `src/utils/preferred-language.test.ts` | Nine automated tests |

Defuddle construction sites in the API, CLI, and their tests were also identified. They were left unchanged because they are outside the browser-extension scope.

## Automated verification

| Check | Result |
|---|---|
| Browser-language priority, document-language fallback, missing language | PASS |
| Synthetic en-US/ja-JP tracks, no preference | English extracted: PASS |
| Same synthetic tracks, ja-JP or ja preference | Japanese extracted: PASS |
| Synthetic English-only track, Japanese preference | English extracted: PASS |
| Synthetic Japanese-only track | Japanese extracted: PASS |
| Synthetic article title, author, content, and Markdown | PASS |
| Added tests | 9 / 9 PASS |
| `tsc --noEmit --module ES2020` (same module setting as the build) | PASS |
| `git diff --check` and `git apply --check` against the original commit | PASS |
| `npm run build` | Chromium / Firefox / Safari: PASS |

Transcript tests use Defuddle's actual `parseAsync` with synthetic network responses. They do not exercise YouTube's live API or saving a clip in Chrome. Each build produced three size-related warnings.

The full suite had **217 passes and six failures out of 223 tests**. Six existing template-integration tests failed; the same six failures were reproduced in a separate worktree at the unmodified upstream commit. Differences included timezone and line endings between expected output and the execution environment. No new failures caused by the patch were observed. Expected outputs were not rewritten, and existing tests were not excluded.

Plain `tsc --noEmit` fails because the existing tsconfig uses module=es6 while the CLI and other files use dynamic imports. Webpack specifies module=ES2020.

## Unverified behavior and limitations

- Live regression checks for English-only and Japanese-only videos.
- Complete regular-article, Reader, and Highlight workflows in Chrome.
- Runtime behavior in Firefox and Safari (builds only were checked).
- Applying the patch to commits other than the pinned version.
- Transcript retrieval failures caused by YouTube responses or future changes.

The patch neither forces Japanese nor translates content. When a matching track is unavailable, Defuddle selects an available track using its existing behavior. If transcript retrieval fails, the transcript may be empty. Regular clipping also retains the existing eight-second asynchronous extraction timeout.

When reporting a problem, include the upstream commit, browser version and language, public video URL, expected language, and actual result. Do not attach personal settings JSON or secrets.
