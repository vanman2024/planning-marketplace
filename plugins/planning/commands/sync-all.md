---
description: Sync features.json, specs/, project.json, and tasks.md - keeps all planning artifacts in sync
argument-hint: [--auto-fix] [--report-only]
allowed-tools: Read, Bash, Task, TodoWrite, Write, Edit, Glob, Grep
---

---
🚨 **EXECUTION NOTICE FOR CLAUDE**

When you invoke this command via SlashCommand, the system returns THESE INSTRUCTIONS below.

**YOU are the executor. This is NOT an autonomous subprocess.**

- ✅ The phases below are YOUR execution checklist
- ✅ YOU must run each phase immediately using tools (Bash, Read, Write, Edit, TodoWrite)
- ✅ Complete ALL phases before considering this command done
- ❌ DON'T wait for "the command to complete" - YOU complete it by executing the phases
- ❌ DON'T treat this as status output - it IS your instruction set

**Immediately after SlashCommand returns, start executing Phase 0, then Phase 1, etc.**

See `@CLAUDE.md` section "SlashCommand Execution - YOU Are The Executor" for detailed explanation.

---

**Arguments**: $ARGUMENTS

Goal: Ensure complete sync between features.json, specs/features/, specs/infrastructure/, project.json, and tasks.md files. Detects drift and fixes discrepancies.

Parse Arguments:
- If contains "--auto-fix": MODE = "auto" (fix issues automatically)
- If contains "--report-only": MODE = "report" (report issues only, no fixes)
- Otherwise: MODE = "interactive" (ask before fixing)

Phase 0: Initialize Sync Process
Goal: Create tracking and prepare for sync analysis

Actions:
- Create todo list:
  * "Scan all planning sources"
  * "Detect drift and discrepancies"
  * "Generate sync report"
  * "Apply fixes (if mode allows)"
- Display: "🔄 Starting comprehensive sync check..."
- Display: "Mode: [MODE]"
- Update todos

Phase 1: Scan All Planning Sources
Goal: Read and analyze all planning artifacts

Actions:
- Read features.json: !{bash jq '.features | length' roadmap/features.json 2>/dev/null || echo "0"}
  * Count total features
  * Get status breakdown (completed, in-progress, planned)
  * Store feature IDs: F001, F002, etc.
- Scan specs/features/ directories: !{bash find specs/features -type d -name "F0*" | wc -l}
  * Count spec directories
  * Get list of feature IDs from directory names
  * Store extra specs (specs without roadmap/features.json entry)
  * Store missing specs (roadmap/features.json entries without spec dirs)
- Read project.json infrastructure section:
  * Count infrastructure.existing items
  * Count infrastructure.needed items
  * Store infrastructure IDs: I001, I002, etc.
- Scan specs/infrastructure/ directories: !{bash find specs/infrastructure -type d -name "I0*" | wc -l 2>/dev/null || echo "0"}
  * Count infrastructure spec directories
  * Get list of infra IDs from directory names
  * Store extra infra specs
  * Store missing infra specs
- Update todos

Phase 2: Detect Drift and Discrepancies
Goal: Identify all sync issues between sources

Actions:
- Compare roadmap/features.json ↔ specs/features/:
  * Missing specs: Features in roadmap/features.json without spec directories
  * Extra specs: Spec directories without roadmap/features.json entries
  * Status mismatches: Features marked "completed" but tasks.md shows incomplete tasks
- Compare project.json ↔ specs/infrastructure/:
  * Missing infra specs: Infrastructure items without spec directories
  * Extra infra specs: Spec directories not in project.json
  * Status mismatches: Infrastructure marked "completed" but no spec exists
- Validate tasks.md completion:
  * For each "completed" feature, check if tasks.md has all tasks marked [x]
  * For each "in-progress" feature, check if tasks.md exists
- Store all discrepancies in memory
- Update todos

Phase 3: Generate Sync Report
Goal: Display comprehensive report of all issues found

Actions:
- Write report to `.claude/sync-report.md`:

```markdown
# Planning Sync Report
Generated: [TIMESTAMP]

## 📊 Summary

| Source | Count | Status |
|--------|-------|--------|
| roadmap/features.json | [X] features | ✅ |
| specs/features/ | [Y] specs | [⚠️ if X != Y] |
| project.json (infra.existing) | [A] items | ✅ |
| project.json (infra.needed) | [B] items | ✅ |
| specs/infrastructure/ | [C] specs | [⚠️ if A+B != C] |

## 🔍 Discrepancies Found

### Features Issues ([N] total)

#### Missing Specs ([N])
Features in roadmap/features.json without spec directories:
- F0XX: [Feature Name]
- F0YY: [Feature Name]

#### Extra Specs ([N])
Spec directories without roadmap/features.json entries:
- specs/features/phase-X/F0ZZ-feature-name/

#### Status Mismatches ([N])
Features marked completed but tasks incomplete:
- F0AA: [Feature Name] - 3/10 tasks incomplete

### Infrastructure Issues ([N] total)

#### Missing Infrastructure Specs ([N])
Infrastructure items in project.json without spec directories:
- I0XX: [Infra Name]
- I0YY: [Infra Name]

#### Extra Infrastructure Specs ([N])
Spec directories without project.json entries:
- specs/infrastructure/I0ZZ-infra-name/

## 🔧 Recommended Actions

[If MODE = "auto"]:
✅ Auto-fix mode enabled - applying fixes automatically

[If MODE = "report"]:
ℹ️  Report-only mode - no changes will be made
Run with --auto-fix to apply fixes automatically

[If MODE = "interactive"]:
⚠️  Interactive mode - will prompt before each fix
```

- Display report to user
- Update todos

Phase 4: Apply Fixes (Conditional)
Goal: Fix sync issues based on mode

Actions:
- If MODE = "report": Skip this phase, display report only and exit
- If MODE = "auto" or MODE = "interactive":

For each missing spec (roadmap/features.json entry without spec dir):
  - If MODE = "interactive": Ask user: "Create spec for F0XX: [Feature Name]? (y/n)"
  - If yes or MODE = "auto":
    * Create spec directory: specs/features/phase-[N]/F0XX-feature-slug/
    * Generate spec.md using feature-spec-writer agent
    * Generate tasks.md template
    * Display: "✅ Created spec for F0XX"

For each extra spec (spec dir without roadmap/features.json entry):
  - If MODE = "interactive": Ask user: "Add F0XX to roadmap/features.json? Extract from spec? (y/n)"
  - If yes or MODE = "auto":
    * Read spec.md to extract feature metadata
    * Add entry to roadmap/features.json with proper structure
    * Display: "✅ Added F0XX to roadmap/features.json"

For each missing infrastructure spec (project.json entry without spec dir):
  - If MODE = "interactive": Ask user: "Generate spec for I0XX: [Infra Name]? (y/n)"
  - If yes or MODE = "auto":
    * Create spec directory: specs/infrastructure/I0XX-infra-slug/
    * Generate spec.md from project.json metadata
    * Generate setup.md template
    * Display: "✅ Created infrastructure spec for I0XX"

For each status mismatch (completed feature with incomplete tasks):
  - Display warning: "⚠️  F0XX marked completed but tasks incomplete"
  - If MODE = "interactive": Ask user: "Mark feature as in-progress? (y/n)"
  - If yes or MODE = "auto":
    * Update roadmap/features.json status to "in-progress"
    * Display: "✅ Updated F0XX status to in-progress"

- Update todos

Phase 5: Finalize and Report
Goal: Summary of changes made and next steps

Actions:
- Count fixes applied
- Display summary:
  ```
  🎉 Sync Complete!

  Changes Made:
  - ✅ Created [N] missing specs
  - ✅ Added [N] features to roadmap/features.json
  - ✅ Generated [N] infrastructure specs
  - ✅ Fixed [N] status mismatches

  Sync Report: .claude/sync-report.md
  ```
- If MODE = "report":
  ```
  ℹ️  Report generated (no changes made)

  To apply fixes, run:
  /planning:sync-all --auto-fix
  ```
- Mark all todos complete
- Display: "Sync check complete! ✨"

Troubleshooting:
- If roadmap/features.json doesn't exist: Error and suggest running /planning:extract-config first
- If project.json doesn't exist: Error and suggest running /foundation:detect first
- If specs/ directory doesn't exist: Error and suggest running /planning:init-project first

Notes:
- This command should be run periodically to catch drift
- Can be added as a git pre-commit hook
- Safe to run multiple times (idempotent)
- Report-only mode is safe for CI/CD pipelines
