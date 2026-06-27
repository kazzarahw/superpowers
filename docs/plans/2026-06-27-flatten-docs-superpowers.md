# Flatten docs/superpowers/ → docs/ Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remove the redundant `docs/superpowers/` nesting by moving `docs/superpowers/specs/` → `docs/specs/` and `docs/superpowers/plans/` → `docs/plans/`, then update all references across the codebase.

**Architecture:** Blind find-and-replace migration. No content changes — only path string updates. Move files first, then update every reference to `docs/superpowers/` in skills, tests, docs, and release notes. Cross-references within the moved files are updated since the files move; all other content is preserved verbatim.

**Tech Stack:** git, sed/mv, shell tests for verification

## Global Constraints

- NO content changes to any file — only path string updates
- Every `docs/superpowers/` reference must be updated or intentionally preserved as a historical annotation
- Files in `docs/superpowers/specs/` and `docs/superpowers/plans/` are historical dated artifacts — their path references within them are updated since the files themselves move
- Global opencode agent config at `~/.config/opencode/agents/develop.md` and `~/.config/opencode/AGENTS.md` also reference `docs/superpowers/` — handled in Task 10 via `customize-opencode`
- After migration: `docs/specs/` (14 files) and `docs/plans/` (11 moving + 4 existing = 15 files)

---

### Task 1: Move spec files from docs/superpowers/specs/ → docs/specs/

**Files:**
- Move: `docs/superpowers/specs/2026-01-22-document-review-system-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-02-19-visual-brainstorming-refactor-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-03-11-zero-dep-brainstorm-server-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-03-23-codex-app-compatibility-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-04-06-worktree-rototill-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-05-05-platform-neutral-config-refs-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-05-05-platform-neutral-prose-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-05-05-platform-neutral-readme-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-05-06-lift-drill-into-evals-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-06-09-sdd-task-scoped-review-dispatch-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-06-10-positive-instruction-redesign-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-06-10-strict-cost-sdd-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-06-10-visual-companion-auth-hardening-design.md` → `docs/specs/`
- Move: `docs/superpowers/specs/2026-06-11-visual-companion-final-hardening-fixup-design.md` → `docs/specs/`

- [ ] **Step 1: Create `docs/specs/` directory**

    ```bash
    mkdir -p docs/specs
    ```

- [ ] **Step 2: Move all spec files**

    ```bash
    mv docs/superpowers/specs/*.md docs/specs/
    ```

- [ ] **Step 3: Verify count**

    ```bash
    ls docs/specs/*.md | wc -l
    # Expected: 14
    ls docs/superpowers/specs/*.md 2>&1
    # Expected: "No such file or directory" (empty)
    ```

- [ ] **Step 4: Commit**

    ```bash
    git add docs/specs/
    git add -u docs/superpowers/specs/   # stage deletions of moved files
    git commit -m "chore: move docs/superpowers/specs/ -> docs/specs/"
    ```

---

### Task 2: Move plan files from docs/superpowers/plans/ → docs/plans/

**Files:**
- Move: `docs/superpowers/plans/2026-01-22-document-review-system.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-02-19-visual-brainstorming-refactor.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-03-11-zero-dep-brainstorm-server.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-03-23-codex-app-compatibility.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-04-06-worktree-rototill.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-05-06-lift-drill-into-evals.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-05-07-pi-extension-and-evals.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-06-09-sdd-task-scoped-review-dispatch.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-06-09-visual-companion-issues.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-06-10-visual-companion-auth-hardening.md` → `docs/plans/`
- Move: `docs/superpowers/plans/2026-06-11-visual-companion-final-hardening-fixup.md` → `docs/plans/`

- [ ] **Step 1: Move all plan files**

    ```bash
    mv docs/superpowers/plans/*.md docs/plans/
    ```

- [ ] **Step 2: Verify counts**

    ```bash
    ls docs/plans/*.md | wc -l
    # Expected: 15 (11 moved + 4 existing)
    ls docs/superpowers/plans/*.md 2>&1
    # Expected: "No such file or directory"
    ```

- [ ] **Step 3: Commit**

    ```bash
    git add docs/plans/
    git add -u docs/superpowers/plans/   # stage deletions of moved files
    git commit -m "chore: move docs/superpowers/plans/ -> docs/plans/"
    ```

---

### Task 3: Remove empty docs/superpowers/ directory

After Tasks 1-2, `docs/superpowers/` contains empty `specs/` and `plans/` subdirectories with no tracked files. Since git does not track empty directories, use shell-level removal (not `git rm`).

- [ ] **Step 1: Verify directory contains no remaining files**

    ```bash
    find docs/superpowers/ -type f
    # Expected: no output (all files were moved in Tasks 1-2)
    ```

- [ ] **Step 2: Remove the empty directory tree**

    ```bash
    rm -rf docs/superpowers/
    ```

- [ ] **Step 3: Confirm removal**

    ```bash
    ls -d docs/superpowers/ 2>&1
    # Expected: "No such file or directory"
    git status --short
    # Expected: nothing staged — git doesn't track empty dirs
    ```

---

### Task 4: Update references in skill files

**Files to modify:**
- `skills/brainstorming/SKILL.md` — 2 refs (lines 29, 106)
- `skills/brainstorming/spec-document-reviewer-prompt.md` — 1 ref (line 7)
- `skills/writing-plans/SKILL.md` — 2 refs (lines 18, 160)
- `skills/subagent-driven-development/SKILL.md` — 1 ref (line 277)
- `skills/requesting-code-review/SKILL.md` — 1 ref (line 60)

These skill files use three distinct patterns:
1. `docs/superpowers/specs/` → `docs/specs/`
2. `docs/superpowers/plans/` → `docs/plans/`
3. `docs/superpowers/` (generic) — requires context-specific handling

- [ ] **Step 1: Update all skill file references**

    ```bash
    # brainstorming/SKILL.md — two occurrences of docs/superpowers/specs/
    sed -i 's|docs/superpowers/specs/|docs/specs/|g' skills/brainstorming/SKILL.md

    # brainstorming/spec-document-reviewer-prompt.md — one occurrence
    sed -i 's|docs/superpowers/specs/|docs/specs/|g' skills/brainstorming/spec-document-reviewer-prompt.md

    # writing-plans/SKILL.md — two occurrences of docs/superpowers/plans/
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' skills/writing-plans/SKILL.md

    # subagent-driven-development/SKILL.md — one occurrence
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' skills/subagent-driven-development/SKILL.md

    # requesting-code-review/SKILL.md — one occurrence
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' skills/requesting-code-review/SKILL.md
    ```

- [ ] **Step 2: Verify no remaining references in skill files**

    ```bash
    grep -rn 'docs/superpowers/' skills/ || echo "CLEAN"
    # Expected: "CLEAN" — no remaining references
    ```

- [ ] **Step 3: Commit**

    ```bash
    git add skills/brainstorming/SKILL.md skills/brainstorming/spec-document-reviewer-prompt.md skills/writing-plans/SKILL.md skills/subagent-driven-development/SKILL.md skills/requesting-code-review/SKILL.md
    git commit -m "chore: update skill file path refs from docs/superpowers/ -> docs/"
    ```

---

### Task 5: Update references in RELEASE-NOTES.md

**File:** `RELEASE-NOTES.md` — 3 references (lines 412, 413, 452)

- [ ] **Step 1: Read lines 410-416 and 449-454 to verify exact context**

    ```bash
    sed -n '410,416p' RELEASE-NOTES.md
    sed -n '449,454p' RELEASE-NOTES.md
    ```

- [ ] **Step 2: Update references**

    Line 412: `docs/superpowers/specs/` → `docs/specs/` (release note describing feature at time of release — this is a historical artifact describing what existed at that time. But since the paths literally change, update it.)
    Line 413: `docs/superpowers/plans/` → `docs/plans/`
    Line 452: `docs/superpowers/` → `docs/` (ambient mention of where spec/plan lived)

    Apply replacements:
    ```bash
    # Line 412: docs/superpowers/specs/ → docs/specs/
    # Line 413: docs/superpowers/plans/ → docs/plans/
    # Line 452: docs/superpowers/ → docs/
    sed -i 's|docs/superpowers/specs/|docs/specs/|g' RELEASE-NOTES.md
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' RELEASE-NOTES.md
    sed -i 's#docs/superpowers/#docs/#g' RELEASE-NOTES.md
    ```

- [ ] **Step 3: Verify**

    ```bash
    grep -n 'docs/superpowers' RELEASE-NOTES.md || echo "CLEAN"
    ```

- [ ] **Step 4: Commit**

    ```bash
    git add RELEASE-NOTES.md
    git commit -m "chore: update RELEASE-NOTES.md path refs from docs/superpowers/ -> docs/"
    ```

---

### Task 6: Update references in test shell scripts

**Files to modify:**
- `tests/explicit-skill-requests/run-extended-multiturn-test.sh` — 1 ref (line 15)
- `tests/explicit-skill-requests/run-multiturn-test.sh` — 3 refs (lines 19, 30, 62)
- `tests/explicit-skill-requests/run-test.sh` — 2 refs (lines 46, 49)
- `tests/explicit-skill-requests/run-haiku-test.sh` — 2 refs (lines 15, 34)
- `tests/claude-code/test-subagent-driven-development-integration.sh` — 4 refs (lines 56, 59, 135, 149)
- `tests/claude-code/test-helpers.sh` — 1 ref (line 146)
- `tests/claude-code/test-worktree-path-policy.sh` — 2 refs (lines 12, 13)

- [ ] **Step 1: Update all test script references**

    ```bash
    # explicit-skill-requests tests
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' tests/explicit-skill-requests/run-extended-multiturn-test.sh
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' tests/explicit-skill-requests/run-multiturn-test.sh
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' tests/explicit-skill-requests/run-test.sh
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' tests/explicit-skill-requests/run-haiku-test.sh

    # claude-code tests
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' tests/claude-code/test-subagent-driven-development-integration.sh
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' tests/claude-code/test-helpers.sh
    # test-worktree-path-policy.sh has both specs/ and plans/ refs
    sed -i 's|docs/superpowers/specs/|docs/specs/|g' tests/claude-code/test-worktree-path-policy.sh
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' tests/claude-code/test-worktree-path-policy.sh
    ```

- [ ] **Step 2: Verify**

    ```bash
    grep -rn 'docs/superpowers/' tests/ || echo "CLEAN"
    ```

- [ ] **Step 3: Commit**

    ```bash
    git add tests/
    git commit -m "chore: update test script path refs from docs/superpowers/ -> docs/"
    ```

---

### Task 7: Update references in test prompt files

**Files to modify:**
- `tests/explicit-skill-requests/prompts/skip-formalities.txt` — 1 ref
- `tests/explicit-skill-requests/prompts/after-planning-flow.txt` — 1 ref
- `tests/explicit-skill-requests/prompts/mid-conversation-execute-plan.txt` — 1 ref
- `tests/explicit-skill-requests/prompts/action-oriented.txt` — 1 ref
- `tests/explicit-skill-requests/prompts/i-know-what-sdd-means.txt` — 1 ref
- `tests/explicit-skill-requests/prompts/claude-suggested-it.txt` — 1 ref

All six files contain `docs/superpowers/plans/` references.

- [ ] **Step 1: Update all prompt file references**

    ```bash
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' tests/explicit-skill-requests/prompts/*.txt
    ```

- [ ] **Step 2: Verify**

    ```bash
    grep -rn 'docs/superpowers/' tests/explicit-skill-requests/prompts/ || echo "CLEAN"
    ```

- [ ] **Step 3: Commit**

    ```bash
    git add tests/explicit-skill-requests/prompts/
    git commit -m "chore: update test prompt path refs from docs/superpowers/ -> docs/"
    ```

---

### Task 8: Update cross-references within moved spec and plan files

Some files in `docs/specs/` and `docs/plans/` contain internal references to the old `docs/superpowers/` paths. These need updating.

**Affected files and occurrences:**

`docs/specs/2026-05-06-lift-drill-into-evals-design.md`:
- Line 111: ``docs/superpowers/plans/`` (in Search targets table)
- Line 189: ``docs/superpowers/plans/``
- Line 193: ``docs/superpowers/plans/``
- Line 229: ``docs/superpowers/plans/``

`docs/specs/2026-05-05-platform-neutral-prose-design.md`:
- Line 25: ``docs/superpowers/specs/*.md`` (historical artifact annotation — this spec itself was documenting the old path structure, and now it's outdated. Update the reference.)

`docs/specs/2026-06-10-positive-instruction-redesign-design.md`:
- Line 129: ``docs/superpowers/skills/micro-testing-prompt-guidance.md`` — this references `docs/superpowers/skills/` which doesn't exist in the current directory structure. This is a reference to a planned file path in a design doc. Since `docs/superpowers/` is being removed, update to `docs/skills/`. The referenced file `micro-testing-prompt-guidance.md` does not exist at either path — this is a speculative reference in a design doc.

`docs/plans/2026-03-11-zero-dep-brainstorm-server.md`:
- Line 11: ``docs/superpowers/specs/``

`docs/plans/2026-06-11-visual-companion-final-hardening-fixup.md`:
- Line 7: ``docs/superpowers/specs/``
- Lines 46, 924, 943, 986, 996: ``docs/superpowers/plans/``

`docs/plans/2026-03-23-codex-app-compatibility.md`:
- Line 11: ``docs/superpowers/specs/``

`docs/plans/2026-02-19-visual-brainstorming-refactor.md`:
- Line 11: ``docs/superpowers/specs/``

`docs/plans/2026-01-22-document-review-system.md`:
- Line 11: ``docs/superpowers/specs/``
- Line 33: ``docs/superpowers/specs/``

`docs/plans/2026-04-06-worktree-rototill.md`:
- Line 11: ``docs/superpowers/specs/``

`docs/plans/2026-06-09-sdd-task-scoped-review-dispatch.md`:
- Line 11: ``docs/superpowers/specs/``
- Lines 605, 615, 674, 675, 676: ``docs/superpowers/plans/``

`docs/plans/2026-05-06-lift-drill-into-evals.md`:
- Line 11: ``docs/superpowers/specs/``
- Line 966: ``docs/superpowers/plans/*.md``
- Line 1008: ``docs/superpowers/plans/*.md``
- Line 1018: ``docs/superpowers/plans/*.md``
- Line 1058: ``docs/superpowers/plans/*.md``
- Line 1224: ``docs/superpowers/specs/``
- Line 1330: ``docs/superpowers/specs/``
- Line 1372: ``docs/superpowers/plans/`` (two occurrences on one line)

**IMPORTANT:** The plan file itself (`docs/plans/2026-06-27-flatten-docs-superpowers.md`) contains `docs/superpowers/` as source paths being migrated FROM. Exclude it from sed replacements so it remains historically accurate.

- [ ] **Step 1: Count references before replacements**

    ```bash
    NOTPLAN='docs/specs/*.md docs/plans/2026-01-22-document-review-system.md docs/plans/2026-02-19-visual-brainstorming-refactor.md docs/plans/2026-03-11-zero-dep-brainstorm-server.md docs/plans/2026-03-23-codex-app-compatibility.md docs/plans/2026-04-06-worktree-rototill.md docs/plans/2026-05-06-lift-drill-into-evals.md docs/plans/2026-05-07-pi-extension-and-evals.md docs/plans/2026-06-09-sdd-task-scoped-review-dispatch.md docs/plans/2026-06-09-visual-companion-issues.md docs/plans/2026-06-10-visual-companion-auth-hardening.md docs/plans/2026-06-11-visual-companion-final-hardening-fixup.md'
    BEFORE=$(grep -c 'docs/superpowers/' $NOTPLAN 2>/dev/null | awk -F: '{s+=$NF} END {print s}')
    echo "References to replace: $BEFORE"
    ```

- [ ] **Step 2: Update all references (excluding this plan file)**

    ```bash
    # Replace docs/superpowers/specs/ → docs/specs/
    sed -i 's|docs/superpowers/specs/|docs/specs/|g' $NOTPLAN

    # Replace docs/superpowers/plans/ → docs/plans/
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' $NOTPLAN

    # Replace docs/superpowers/skills/ → docs/skills/ (any speculative refs)
    sed -i 's|docs/superpowers/skills/|docs/skills/|g' $NOTPLAN
    ```

- [ ] **Step 3: Preview catch-all — verify only known patterns remain**

    ```bash
    grep -rn 'docs/superpowers/' $NOTPLAN || echo "CLEAN - no remaining refs"
    # If output is shown, inspect each match before running the catch-all
    ```

- [ ] **Step 4: Apply catch-all for any remaining edge cases**

    ```bash
    sed -i 's#docs/superpowers/#docs/#g' $NOTPLAN
    ```

- [ ] **Step 5: Count-verify zero remaining**

    ```bash
    AFTER=$(grep -c 'docs/superpowers/' $NOTPLAN 2>/dev/null | awk -F: '{s+=$NF} END {print s}')
    echo "References remaining: $AFTER"
    if [ "$AFTER" != "0" ]; then echo "WARNING: $AFTER references remain!"; fi
    ```

- [ ] **Step 6: Verify docs/ is clean (excluding this plan file)**

    ```bash
    # The plan file should still have its original docs/superpowers/ refs (they describe source paths)
    grep -c 'docs/superpowers/' docs/plans/2026-06-27-flatten-docs-superpowers.md
    # Expected: > 0 (plan file preserved as-is)
    ```

- [ ] **Step 7: Commit**

    ```bash
    git add docs/specs/ docs/plans/
    git commit -m "chore: update internal doc path refs from docs/superpowers/ -> docs/"
    ```

---

### Task 9: Final verification — no remaining references

- [ ] **Step 1: Full repository grep**

    ```bash
    # Search all tracked files for remaining references
    # Exclude .git, .worktrees, node_modules, evals
    git grep 'docs/superpowers/' -- ':!.worktrees/*' ':!node_modules/*' ':!evals/*'
    # Expected: no matches
    ```

- [ ] **Step 2: Verify directory structure**

    ```bash
    ls -d docs/superpowers/ 2>&1
    # Expected: "No such file or directory"
    ls docs/specs/ | wc -l
    # Expected: 14
    ls docs/plans/ | wc -l
    # Expected: 15 (or maybe some .md.swp etc)
    ```

- [ ] **Step 3: Run baseline tests**

    ```bash
    # Verify the migration didn't break anything
    bash tests/shell-lint/test-lint-shell.sh
    bash tests/opencode/run-tests.sh
    ```

- [ ] **Step 4: Last substantive commit (if any fixes needed)**

    ```bash
    git add -A
    git status
    # Commit if any changes remain
    ```

---

### Task 10: Post-migration — update global opencode agent config

**NOTE:** The Develop Agent's system prompt (in `~/.config/opencode/agents/develop.md` and `~/.config/opencode/AGENTS.md`) references `docs/superpowers/specs/`, `docs/superpowers/plans/`, and `docs/superpowers/` in its strict boundaries and lifecycle phases. After the migration, ALL sessions using the Develop Agent will tell subagents to save to paths that no longer exist.

**These files are inside the opencode config directory**, which is tracked as part of the Superpowers repo. Apply the same path replacements.

- [ ] **Step 1: Invoke `customize-opencode` skill and load config files**

    Use the `customize-opencode` skill to edit the opencode agent definitions, then locate:
    ```bash
    ls ~/.config/opencode/agents/develop.md 2>/dev/null
    ls ~/.config/opencode/AGENTS.md 2>/dev/null
    ```

- [ ] **Step 2: Apply the same path replacements**

    ```bash
    # Update docs/superpowers/specs/ → docs/specs/ in develop.md
    sed -i 's|docs/superpowers/specs/|docs/specs/|g' ~/.config/opencode/agents/develop.md
    # Update docs/superpowers/plans/ → docs/plans/ in develop.md
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' ~/.config/opencode/agents/develop.md
    # Update remaining docs/superpowers/ → docs/ in develop.md
    sed -i 's#docs/superpowers/#docs/#g' ~/.config/opencode/agents/develop.md
    ```

    Repeat for `~/.config/opencode/AGENTS.md` if it contains references:
    ```bash
    sed -i 's|docs/superpowers/specs/|docs/specs/|g' ~/.config/opencode/AGENTS.md
    sed -i 's|docs/superpowers/plans/|docs/plans/|g' ~/.config/opencode/AGENTS.md
    sed -i 's#docs/superpowers/#docs/#g' ~/.config/opencode/AGENTS.md
    ```

- [ ] **Step 3: Verify zero remaining references**

    ```bash
    grep -n 'docs/superpowers' ~/.config/opencode/agents/develop.md ~/.config/opencode/AGENTS.md 2>/dev/null || echo "CLEAN"
    ```

- [ ] **Step 4: Commit**

    These files are tracked in the repo (they're part of `.opencode/` or the opencode config):
    ```bash
    # If these files are inside the repo (under .opencode/ or similar):
    git add ~/.config/opencode/agents/develop.md ~/.config/opencode/AGENTS.md 2>/dev/null || echo "Outside repo — commit manually"
    git status
    git commit -m "chore: update opencode agent config path refs from docs/superpowers/ -> docs/"
    ```
