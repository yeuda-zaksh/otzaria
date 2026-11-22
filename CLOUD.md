# Cloud agent instructions (Otzaria fork yeuda-zaksh/otzaria)

AGENTS.md on upstream dev overrides everything here. Read it and docs/agent_test_map.md first.

## FIRST BUILD IS LOCAL — cloud runs follow-up rounds only
- The first build of every fix (analysis, reproduction, the failing verification test, the fix, the two commits) is ALWAYS done locally, under the owner's control.
- The cloud runs only round 2 onward: independent QA, fixing QA findings, rebasing on newer dev, maintainer review comments, broad test runs.
- If your task has no locally built branch, do NOT build the fix from scratch: write NOT READY with "missing local first build".
- Do not change the approach of the locally built fix. If you think it is wrong, return NOT READY with evidence; the decision goes back to local.

## Setup (every run)
- `git config user.name "yeuda-zaksh"; git config user.email "358741176+yeuda-zaksh@users.noreply.github.com"` before any commit.
- `git remote add upstream https://github.com/Otzaria/otzaria.git; git fetch upstream dev`.
- Install Flutter: read the version from the repo (pubspec.yaml environment / .github workflows / .fvmrc), download the Linux tarball from storage.googleapis.com/flutter_infra_release/releases/stable/linux/, extract, add to PATH, `git config --global --add safe.directory '*'`, then `flutter pub get --enforce-lockfile`. If pub get fails on a git-pinned commit, `cd ~/.pub-cache/git/cache/<pkg>-* && git fetch origin <sha>` and retry.
- You run as root: some chmod/permission tests fail on dev too — always compare counts branch vs base.

## Rules
- NO AI attribution anywhere: no Co-Authored-By, no "Claude", no "Generated with", no 🤖 — in commits, comments, or PR body.
- A PR branch has exactly two commits over upstream/dev: (1) `בדיקת אימות: …` tests only, failing on dev; (2) the fix, Hebrew message, body states the root cause. When you change an existing PR branch you may rewrite it (it has no PR yet) and push with `--force-with-lease`.
- Minimal fix at the root; comments ≤2 lines, Hebrew, explain why, no history.
- Run the relevant tests (files importing what you touched) on branch AND on upstream/dev, report exact counts. `flutter analyze` on changed files, `dart format --set-exit-if-changed` on changed files, `dart tool/ui_lint_cli.dart`, and for API changes `dart run tool/plugins/generate_plugin_spec.dart --check`.
- Overlap: `git fetch upstream pull/<N>/head:pr-<N>` for open PRs touching the same files (list given in your notes or found via `gh api -X GET search/issues -f q='repo:Otzaria/otzaria is:pr is:open <file or topic>'`), then `git merge-tree --write-tree` and report.
- No md/result files in the PR branch. Don't guess: when you can't reproduce or need a maintainer decision → NOT READY with evidence.

## Result channel
Write your result to an orphan branch `prbody/<slug>-<role>` (role = fix or qa) in the fork containing:
- VERDICT.md: first line exactly `READY` or `NOT READY`; then branch + SHA, PR title, test counts branch vs base, overlaps, open decisions.
- BODY.md: the Hebrew PR body (sections: מה השינוי, השורש/למה, התיקון, ## בדיקות with counts branch vs base, PRs חופפים, סיכונים). No AI attribution.
Create it with: `git checkout --orphan prbody/<slug>-<role> && git rm -rf . -q && (write files) && git add VERDICT.md BODY.md && git commit -m result && git push -f origin prbody/<slug>-<role>`.

## QA role
Review the fix branch adversarially per the lead maintainer: "the work must be clean, efficient, and compliant with AGENTS.md". Verify: authorship (author AND committer yeuda-zaksh), no AI attribution, exactly 2 commits, commit 1 tests-only and red on dev, each fix guarded by a test that fails without it (mutate, run, restore), analyze/format/ui_lint, overlaps, no unrelated files, no pubspec.lock change. Fix small issues yourself in the right commit and push; then decide READY / NOT READY.

## Maintainer context
Maintainer Y-PLONI closed the large PR #5553 and asked for focused PRs with reproductions. His findings: (a) with nikud hidden, deleting first ב of `בַבֹ` keeps `בַ` (merge ignored the edit range); (b) Enter between voweled letters, delete second, undo → nikud lost (multi-line path loses source in undo history); (c) in a paragraph with a saved correction reopened for editing, Escape is swallowed; (d) highlight rescans ranges per segment on every build; (e) the getBookCorrectionStatus docs example must read `status` from the SDK response data envelope. Nikud hiding and search highlight are OUT of scope for the current PRs.

## Quality bar (non-negotiable)
The PR must meet Otzaria's highest standard. Before READY, check every item of `.github/pull_request_template.md` and AGENTS.md against the branch, and fill BODY.md following the template's structure exactly (all sections, including the visual-change section with each item answered or "אין"). Read the 5 most recently merged PRs touching the same area (`git log upstream/dev --merges -- <files>`) and match their style. Any doubt, shortcut, unguarded fix, unrelated change, or unexplained test difference → NOT READY.

## Loop mode (role: loop) — finish a locally built PR
You own the slug end-to-end in this one session. Repeat up to 4 rounds:
1. Start from the existing, locally built branch. The first round in the cloud is QA (step 2). Later rounds: FIX exactly the QA findings.
2. QA: launch a SEPARATE subagent (Agent tool, fresh context) with only: CLOUD.md path, notes/<slug>.md, the branch name, and "act as the QA role; return READY/NOT READY + findings". Never give it your reasoning or conclusions. Do not review your own work.
3. If QA returns READY → stop. If NOT READY → fix exactly its findings, then go back to step 2.
Owner-waived items are not findings: screenshots, Fixes# (use "Refs #5553").
Stop early with NOT READY only for a decision that belongs to a maintainer/owner, with evidence.
At the end push the branch (2 commits, --force-with-lease) and prbody/<slug>-final (VERDICT.md first line READY/NOT READY, the round history, final test counts branch vs base; BODY.md final). The manager will only verify and submit.
