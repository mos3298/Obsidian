# Maintenance and upstream contributions

English | [日本語](../ja/maintenance.md) · [README](../../README.md)

## Updating this patch

The patch is maintained against a pinned upstream commit. Changes to upstream main are not automatically incorporated into an installed local build.

The initial patch and its documentation are pinned by the `web-clipper-language-patch-v0.1` tag. Keep published version tags at their original commits; use a new version tag for a future release rather than moving an existing tag.

For each update:

1. Check whether upstream already includes an equivalent fix.
2. Review Defuddle construction sites and transcript-language propagation at the new upstream commit.
3. Apply the patch in a separate working directory and adapt conflicts to the current source.
4. Run language-selection tests, builds, and live regression checks for videos and the main extension workflows.
5. Update the supported commit in the README, the patch, test results, and changelog together.

A patch applying cleanly does not prove it works correctly. Do not list an untested upstream version as supported.

Keep English and Japanese documentation aligned, especially commands, supported versions, and verified versus unverified behavior.

## Updating an installed local build

1. Export the local extension's settings as a backup.
2. Build the newly supported version.
3. Update the `dist` directory currently loaded by Chrome with the new build.
4. Reload the local extension from Chrome's extensions page.
5. Reload the target page and verify behavior.

Reloading an extension from the same path normally preserves settings. Moving the source directory or removing and reinstalling the extension may change this behavior, so retain the backup.

## Proposing an upstream pull request

The containing repository, `mos3298/Obsidian`, is not an upstream fork. To submit a PR, separately fork `obsidianmd/obsidian-clipper`, apply the changes on a working branch, and submit that branch to upstream.

Explain:

- Cause: the browser's preferred language is not passed to Defuddle.
- Change: a small shared helper passes the language at seven extension construction sites.
- Related issue: [#957](https://github.com/obsidianmd/obsidian-clipper/issues/957).
- Automated test results, existing baseline failures, and remaining unverified behavior.

Present the change as a general fix that respects the browser language, rather than a Japanese-only feature. Upstream maintainers decide whether to accept it, how to implement it, and when to release it.

If upstream releases a fix, document the version users should migrate to and mark this patch as unnecessary for that version.
