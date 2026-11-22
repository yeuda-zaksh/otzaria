# highlight — new branch fix/correction-editor-search-highlight (role: loop)
Base: build on top of fork branch fix/correction-editor-hide-nikud (open PR #5735, not merged yet), because the highlight must work with hidden nikud/teamim. Commits: exactly 2 NEW commits on top of that branch (tests, then fix). When #5735 is merged the manager will rebase onto dev; until then this is not opened as a PR.
Goal: in the plugin correction editor, the reading view's search highlight appears identically (same ranges, colors, current-match styling) — for unchanged text AND after edits.
Single source of truth: reuse the reader's pipeline (TextRendererService.processText → highLight with fuzzy/alternativeWords/partial/matchPolicy/currentSearchIndex, and SearchHighlightCss.resolve with theme tokens). No own matcher, no hardcoded colors.
Maintainer finding (d) on closed #5553: the previous implementation rescanned highlight ranges per segment on every text build. Requirements: compute ranges once per (text, settings, highlight pattern revision utils.highlightPatternRevision, theme) and cache; buildTextSpan must only slice precomputed ranges (O(spans + ranges), no per-leaf `where` scans, no per-build processText). Add a test proving no recomputation across builds with unchanged inputs (e.g. counting calls through a seam), and a perf sanity test on a ~6000-char paragraph.
Tests (red on base branch): voweled text + unvoweled query highlights same ranges as reader (nikud shown and hidden), edited text highlighted, current match styling, alternative words, composing (IME) keeps underline, highlight in a paragraph split by Enter (hard-break path), revision change re-highlights.
Reference (has the perf issue, ideas only): branch fix/correction-editor-usability — coloredRanges/_highlightsFor in plugin_correction_controller.dart / plugin_correction_paragraph.dart.
PR title suggestion: "עורך התיקון: הדגשת תוצאות חיפוש כמו בתצוגה". Refs #5553.

## Resume (manager)
Previous loop run stopped on a spend limit after pushing round-1 commits (fix/correction-editor-search-highlight @ 89b3894a9: test commit e4beeb038 + fix). QA never finished. Do NOT redo the fix: start at step 2 (fresh QA subagent) on the pushed branch, then continue the loop as usual and write prbody/highlight-final.

## Round 5 (manager, after 4-round NOT READY at a9997c063)
The previous loop's code is fine; close ONLY these findings from prbody/highlight-final, keeping exactly 2 commits over origin/fix/correction-editor-hide-nikud (force-with-lease OK, no PR yet):
1. Add a test with a colored base style so `style.merge(highlight)` → `highlight.merge(style)` in `_resolve` fails.
2. Add a test that clears the search in an open editor and then bumps the pattern revision, so removing the `removeListener` branch for `next == null` in setHighlight fails.
3. Simplify `formats.length == text.length ? _highlights() : const []` to `_highlights()` if the earlier branch already returns super; prove with tests.
4. Make the "reader settings" test use an independent oracle (literal expected ranges, not the controller's regex/settings path).
5. The dispose test passes on base: either make it discriminating or drop it and say so.
6. BODY.md: the screenshot line must be exactly `- [ ] שינוי UI? — לא צורפו צילומי מסך` (owner waived; no [x]). Keep Refs #5553.
Merge order with #5739/#5667 is a maintainer matter, not a finding: mention in BODY risks only (#5667 may need the "current match" test's background expectation updated if it merges first).
Then a fresh QA subagent; loop up to 2 rounds; write prbody/highlight-final.
