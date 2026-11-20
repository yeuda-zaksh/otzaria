# keyboard — branch fix/correction-editor-keyboard (role: fix, then qa)
Done: Enter = newline (TextInputAction.newline; session service accepts '\n' in proposed text only; '\n' skips structured correction), arrows up/down between wrapped lines and to adjacent paragraph keeping column, Escape leaves editing incl. maintainer repro (c). Local counts: dev +497 -12 (12 = new tests), branch all pass after updating combined_book_screen_test's '\n' expectation.
REMAINING (must fix in this PR — reachable only because Enter now works):
1. A formatted paragraph (e.g. `אב<b>גד</b>ה`) split by Enter renders as plain text: `_HardBreakController.buildTextSpan` in lib/widgets/text/rtl_text_field.dart falls back to plain when text has hard breaks. Fix: expand the source controller's span across hard breaks (reference: `_expandSpan` in branch fix/correction-editor-usability of this fork, file lib/widgets/text/rtl_text_field.dart).
2. Undo after deleting a formatted char in a split paragraph loses formatting: the multi-line path's undo history doesn't carry CorrectionFormat metadata. Smallest fix in HardBreakValueMapper / _HardBreakController / _FormattedEditingValue; if disproportionate → NOT READY with evidence.
Partial unfinished attempt at these is on branch wip/keyboard-format-undo (reference only, do not merge as-is).
Also verify maintainer repro (b) on this branch (no nikud hiding here): Enter between `בַבֹ`, delete second, undo → nikud intact.
Tests for both fixes, red without fix. Keep exactly 2 commits. PR title: "העורך בתוסף התיקון: Enter מכניס ירידת שורה, חצים נעים בין שורות ופסקאות, Escape יוצא מהעריכה". Overlaps: #5551 (plugin_book_correction_service.dart). Refs #5553.

## QA round (manager)
Fix round result: branch fac89b956 (2 commits over 7f2b6c4ec), +1468 vs dev +1450, mutations proven for format expand and undo copyWith. Owner decisions: no screenshots required, no issue — use "Refs #5553". The combined_book_screen_test expectation update belongs in commit 2 (behavior change) — verify it only changes the '\n' expectation and keeps tag rejection coverage. Read recent merged PRs via `git log upstream/dev --merges -- lib/widgets/text lib/plugins/view` for style.
