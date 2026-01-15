---
phase: 01-foundation
plan: 02
subsystem: commands
tags: [markdown, prompts, formatting, help, conductor-pattern]

requires: []
provides:
  - Prompting patterns reference for all 7 frameworks
  - Formatting utilities reference for markdown output
  - Help command listing all available commands
affects: [02-framework-commands, 03-advanced-frameworks, 04-integration]

tech-stack:
  added: []
  patterns: [conductor-style-commands, framework-prompting-flow, markdown-formatting]

key-files:
  created:
    - lib/prompts.md
    - lib/formatting.md
    - commands/business/biz-help.md
  modified: []

key-decisions:
  - "Use conversation-style prompting with decision gates"
  - "Follow conductor command structure (frontmatter + XML)"

patterns-established:
  - "Framework prompting: entry point -> follow-up sequence -> probing questions -> decision gate"
  - "Markdown formatting: H1 for entity, H2 for sections, bold primary fields in lists"

duration: 3min
completed: 2026-01-15
---

# Phase 1 Plan 02: Shared Command Utilities Summary

**Prompting patterns, formatting utilities, and help command for consistent UX across all business framework commands**

## Performance

- **Duration:** 3 min
- **Started:** 2026-01-15T05:00:08Z
- **Completed:** 2026-01-15T05:03:22Z
- **Tasks:** 3
- **Files created:** 3

## Accomplishments

- Created prompting patterns reference with framework-specific questioning flows for all 7 frameworks
- Created formatting utilities reference with markdown templates for all framework types
- Created help command listing all available commands by category (Project, Canvas, Analysis, Research)
- Established decision gate pattern for controlling prompt depth
- Documented escape hatches for flexible conversation flow

## Task Commits

Each task was committed atomically:

1. **Task 1: Create prompting patterns reference** - `da9bd1f` (feat)
2. **Task 2: Create formatting utilities reference** - `1dec56a` (feat)
3. **Task 3: Create help command** - `e4de6fd` (feat)

## Files Created/Modified

- `lib/prompts.md` — Prompting patterns for gathering information across all 7 frameworks
- `lib/formatting.md` — Markdown formatting templates for framework output
- `commands/business/biz-help.md` — Help command with conductor-style structure

## Decisions Made

- **Conversation-style prompting:** Entry point questions that open discussion, probing questions that follow the thread, decision gates that give users control over depth
- **Conductor command structure:** Frontmatter (name, description, allowed-tools) + XML body (objective, context, process, output, success_criteria)

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- Shared utilities ready for use by all framework commands
- Help command provides discoverability for users
- Ready for Phase 2: Framework Commands (BMC, Lean, VPC, SWOT)

---
*Phase: 01-foundation*
*Completed: 2026-01-15*
