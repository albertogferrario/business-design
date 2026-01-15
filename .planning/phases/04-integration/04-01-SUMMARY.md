---
phase: 04-integration
plan: 01
subsystem: commands
tags: [project-management, list, view, conductor]

requires:
  - phase: 01-foundation
    provides: storage patterns, project.json structure
  - phase: 02-framework-commands
    provides: entity types and directory structure
provides:
  - biz:list command for viewing all frameworks
  - biz:view command for viewing entity details
affects: [04-02 entity linking, future project management]

tech-stack:
  added: []
  patterns: [project check pattern, entity lookup pattern]

key-files:
  created:
    - commands/business/biz-list.md
    - commands/business/biz-view.md
  modified: []

key-decisions:
  - "Group frameworks by category (Canvas vs Analysis)"
  - "Display table format with ID, Type, Name, Created columns"

patterns-established:
  - "Entity lookup by ID from project.json"
  - "Error handling cascade: no project, empty entities, missing ID, missing file"

duration: 1min
completed: 2026-01-15
---

# Phase 4 Plan 1: Project Management Commands Summary

**Two commands for listing and viewing business design entities: biz:list shows project overview grouped by framework type, biz:view displays individual entity details by ID.**

## Performance

- **Duration:** 1 min
- **Started:** 2026-01-15T05:37:40Z
- **Completed:** 2026-01-15T05:39:00Z
- **Tasks:** 2
- **Files created:** 2

## Accomplishments

- Created biz:list command for viewing all frameworks in a project
- Created biz:view command for viewing individual entity details
- Both commands handle missing project, empty project, and error states
- Consistent conductor pattern structure across both commands

## Task Commits

Each task was committed atomically:

1. **Task 1: Create biz:list command** - `90535e8` (feat)
2. **Task 2: Create biz:view command** - `ab3d0f2` (feat)

## Files Created/Modified

- `commands/business/biz-list.md` - Lists all frameworks grouped by category (Canvas/Analysis)
- `commands/business/biz-view.md` - Views entity details by ID with linked entities display

## Decisions Made

- **Framework categorization:** Canvas Frameworks (BMC, Lean, VPC, SWOT) and Analysis Frameworks (Persona, Competitive, Market) for logical grouping
- **Table display format:** ID, Type, Name, Created columns for consistent listing
- **Error cascade:** Check project exists -> check entities exist -> validate ID -> check data file

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- Project management commands complete
- Ready for 04-02 (Entity linking between frameworks)
- biz:list and biz:view provide foundation for viewing linked entities

---
*Phase: 04-integration*
*Completed: 2026-01-15*
