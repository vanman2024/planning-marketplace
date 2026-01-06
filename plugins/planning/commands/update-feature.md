---
description: Update existing feature across roadmap, specs, and architecture docs when requirements change
argument-hint: <spec-number> [changes] [--all]
---

---
🚨 **EXECUTION NOTICE FOR CLAUDE**

When you invoke this command via SlashCommand, the system returns THESE INSTRUCTIONS below.

**YOU are the executor. This is NOT an autonomous subprocess.**

- ✅ The phases below are YOUR execution checklist
- ✅ YOU must run each phase immediately using tools (Bash, Read, Write, Edit, TodoWrite)
- ✅ Complete ALL phases before considering this command done
- ❌ DON't wait for "the command to complete" - YOU complete it by executing the phases
- ❌ DON't treat this as status output - it IS your instruction set

**Immediately after SlashCommand returns, start executing Phase 0, then Phase 1, etc.**

See `@CLAUDE.md` section "SlashCommand Execution - YOU Are The Executor" for detailed explanation.

---


**Arguments**: $ARGUMENTS

Goal: Update an existing feature across all planning documentation when requirements, priorities, or architecture changes.

Core Principles:
- Identify scope of change: Requirements vs priority vs architecture
- Cascade updates across all affected docs
- Create new ADR if architecture decision changed
- Maintain consistency across roadmap, specs, and architecture

## Source of Truth: features.json

**CRITICAL**: `roadmap/features.json` is the source of truth. Always read JSON first.

Phase 1: Discovery
Goal: Identify feature and understand what needs to change

Actions:
- Create todo list tracking workflow phases using TodoWrite
- Parse $ARGUMENTS for:
  - Spec number or --all flag
  - Change description
  - Flags: --all (update all features with same change)
- If --all flag present:
  - Display: "🔄 Updating ALL features with change: [changes]"
  - Read features.json: `cat roadmap/features.json | jq '.features[].id'`
  - Confirm with AskUserQuestion: "Update all [count] features?"
- Else if spec number not provided, use AskUserQuestion to ask:
  - Which feature needs updating? (spec number or name)

**Read features.json FIRST (source of truth):**
```bash
cat roadmap/features.json | jq '.features[] | select(.id == "F[NUMBER]")'
```
- Extract: id, name, phase, status, priority, estimated_days, complexity, dependencies, infrastructure_dependencies
- **VALIDATE**:
  - phase is a STRING (MVP/Post-MVP/Beta) not a number
  - status uses hyphens (in-progress not in_progress)
  - priority is P0/P1/P2

- Find spec directory:
  !{bash find specs/features -type d -name "F$SPEC_NUMBER-*" 2>/dev/null | head -1}
- If not found, display error and list available features
- Load existing feature files (from discovered path):
  - Read spec: [SPEC_DIR]/spec.md
  - Read tasks: [SPEC_DIR]/tasks.md
- Find feature in ROADMAP.md:
  !{bash grep -n "$SPEC_NUMBER" docs/ROADMAP.md}

Phase 2: Determine Change Scope
Goal: Understand what changed and what needs updating

Actions:
- Use AskUserQuestion to determine change type:
  - What changed? (select all that apply)
    - Requirements/scope changed
    - Priority changed (P0 ↔ P1 ↔ P2)
    - Timeline/phase changed
    - Architecture/approach changed
    - Dependencies changed
  - Describe the changes:
- For each change type, determine affected docs:
  - Requirements → spec.md, tasks.md
  - Priority → spec.md, ROADMAP.md
  - Timeline → ROADMAP.md, tasks.md
  - Architecture → spec.md, docs/architecture/*, new ADR
  - Dependencies → spec.md, ROADMAP.md
- Ask: Does this change require a new architecture decision? (Yes/No)

Phase 3: Update Spec
Goal: Update feature specification

Actions:

Task(description="Update spec", subagent_type="planning:feature-spec-writer", prompt="Update spec [NUMBER]-[NAME] with changes: [from Phase 2]. Read specs/features/[NUMBER]-*/spec.md and tasks.md. Apply changes (requirements/priority/timeline/architecture/dependencies). Maintain minimal format (100-150 lines). Preserve architecture references.")

Update todos

Phase 4: Update Roadmap
Goal: Update roadmap

Actions:
- If Priority/Timeline/Dependencies changed:
  Task(description="Update roadmap", subagent_type="planning:roadmap-planner", prompt="Update docs/ROADMAP.md for spec [NUMBER]: Priority [old→new], Timeline [old→new], Dependencies [old→new], Phase [old→new]. Read ROADMAP.md, find feature, apply changes, update gantt if needed, recalculate totals.")
- Update todos

Phase 5: Create ADR (if needed)
Goal: Document decision change

Actions:
- If architecture decision changed:
  Task(description="Create ADR", subagent_type="planning:decision-documenter", prompt="Create ADR for spec [NUMBER] decision change: [from Phase 2]. Document old→new approach, rationale, impact, consequences. Reference previous ADR if superseding. Create docs/adr/[NUMBER]-[slug].md.")
- Update todos

Phase 6: Update Architecture (if needed)
Goal: Update architecture docs

Actions:
- If architecture approach changed:
  Task(description="Update architecture", subagent_type="planning:architecture-designer", prompt="Update docs/architecture/ for spec [NUMBER] changes: [from Phase 2]. Read affected files, update sections, update diagrams, cross-reference spec and ADR.")
- Update todos

Phase 7: Update Mem0
Goal: Update stored relationships

Actions:
- Run doc-sync: !{bash python plugins/planning/skills/doc-sync/scripts/update-relationships.py --spec [NUMBER]}
- Update todos

Phase 8: Summary
Goal: Report results

Actions:
- Mark all todos complete
- Display:
  - Feature: [NUMBER]-[NAME]
  - Changes: Spec (updated), Roadmap (if changed), ADR (if created), Architecture (if updated)
  - Before→After: Priority [old→new], Timeline [old→new], Dependencies [old→new]
- Show changes: !{bash git status --short specs/features/[NUMBER]-* docs/ROADMAP.md docs/adr/ docs/architecture/ 2>/dev/null}
- Next steps: Review (git diff), sync code (/iterate:sync if needed), commit
