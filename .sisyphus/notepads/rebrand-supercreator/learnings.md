## [2026-04-06] Task 1: Delete baoyu-image-gen

- Deleted `skills/baoyu-image-gen/` entirely
- Removed entry from `.claude-plugin/marketplace.json` skills array
- Removed `migrateLegacyExtendConfig()` from `skills/baoyu-imagine/scripts/main.ts`
- Removed deprecated row from `CLAUDE.md` Deprecated Skills table
- Cleaned `docs/image-generation.md` of baoyu-image-gen references
- CHANGELOG historical entries preserved intact
- npm test passed, commit b01f66c created

## [2026-04-06] Task 4: Rename packages/baoyu-chrome-cdp → packages/sc-chrome-cdp

- Used `git mv packages/baoyu-chrome-cdp packages/sc-chrome-cdp`
- Updated `packages/sc-chrome-cdp/package.json`: `"name": "sc-chrome-cdp"`
- No internal `baoyu-chrome-cdp` references in any .ts files inside the package
- `appDataDirName = "baoyu-skills"` left as-is per Task 10 instructions
- `BAOYU_CHROME_PROFILE_DIR` env var refs left as-is per Task 11 instructions
- `bun install` cleaned up lockfile, created new `node_modules/sc-chrome-cdp` symlink, removed old `node_modules/baoyu-chrome-cdp` symlink
- npm test: 115 pass, 1 pre-existing fail (x-utils.test.ts - vendor copy not yet updated, Task 5 handles that)
- The x-utils.test.ts failure was pre-existing BEFORE Task 4 started - confirmed via git stash test

## [2026-04-06] Task 5: Sync vendors and update skill imports to sc-* packages

- Sync script matches workspace package names in `skills/*/scripts/package.json` against `packages/*` names — update dep names FIRST, then run sync
- Sync script auto-removes old vendor dirs entirely (`fs.rm(vendorRoot, recursive)`), creates new `vendor/sc-*` dirs, runs `bun install` in each skill
- `baoyu-markdown-to-html/scripts/main.ts` used direct path imports (`./vendor/baoyu-md/src/index.ts`) instead of package name imports — needed separate sed handling (not covered by ast_grep package-name rewrite)
- 119 tests pass after all changes (pre-existing x-utils.test.ts failure from Task 4 is now resolved by vendor sync)
- `--enforce-clean` exits 0 post-commit
- Commit: 7a00bbe `refactor: sync vendors and update skill imports to sc-* packages`

## [2026-04-06] Task 6: Rename skill dirs batch A + update marketplace.json

- `git mv` renames are auto-staged; explicit `git add skills/<dir>` after `git mv` triggers gitignore warning (harmless) since some dir names like `comic/`, `cover-image/` match top-level gitignore patterns for output artifacts
- The staged renames are already captured by `git mv` — the `git add` warning doesn't affect the commit
- marketplace.json: updated top-level `"name"` AND `plugins[0].name` both to `"supercreator"` (two separate fields)
- All 18 skill paths updated in one pass (baoyu-image-gen was deleted in Task 1, so 18 remain)
- Tasks 7 and 8 running in parallel will rename remaining skill dirs; marketplace.json already points to their future paths
- Commit: a49b1d5 `refactor(wave3-A): rename skill dirs batch A and update marketplace.json`

## Task 7: Batch B Renames (6 skills)
- Successfully renamed via single chained `git mv` command: post-to-wechat, post-to-weibo, post-to-x, markdown-to-html, format-markdown, translate
- All 6 renames staged, verified via `git status` and `ls skills/`
- Remaining baoyu- prefixed dirs: baoyu-danger-gemini-web, baoyu-danger-x-to-markdown, baoyu-url-to-markdown, baoyu-xhs-images, baoyu-youtube-transcript (Batch C)
