---
phase: 03-advanced-frameworks
plan: 02
subsystem: commands
tags: [competitive-analysis, business-framework, conductor-command]

requires:
  - phase: 02
    provides: 5-step process pattern from biz-bmc.md
provides:
  - biz:competitive command for market landscape analysis
  - Competitor profiling conversational flow
affects: [biz:help, business-design-commands]

tech-stack:
  added: []
  patterns: [competitor-profiling-pattern, position-mapping-pattern]

key-files:
  created: [commands/business/biz-competitive.md]
  modified: []

key-decisions:
  - "Entry point: Who are your main competitors? - follows lib/prompts.md"
  - "Competitor-first gathering: profile competitors before mapping own position"

patterns-established:
  - "Competitor profile sequence: basics -> strengths -> weaknesses -> positioning"

duration: 1min
completed: 2026-01-15
---

# Phase 03 Plan 02: biz:competitive Command Summary

**Conductor-style command for Competitive Analysis with competitor profiling and market position mapping**

## Performance

- **Duration:** 1 min
- **Started:** 2026-01-15T05:23:32Z
- **Completed:** 2026-01-15T05:24:40Z
- **Tasks:** 1
- **Files modified:** 1

## Accomplishments

- Created biz:competitive command following established 5-step process pattern
- Implemented conversational prompting for competitor profiling
- Mapped own position with differentiators, gaps, and opportunities
- Integrated with lib/types.md Competitive Analysis schema

## Task Commits

Each task was committed atomically:

1. **Task 1: Create biz:competitive command** - `df0957f` (feat)

## Files Created/Modified

- `commands/business/biz-competitive.md` - Conductor-style command for competitive analysis

## Decisions Made

- Entry point "Who are your main competitors?" per lib/prompts.md Competitive Analysis pattern
- Competitor-first gathering: profile all competitors before transitioning to own position mapping
- Probing questions cover market share, pricing models, and ideal customers

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- Command ready for use with existing project infrastructure
- Ready for 03-03-PLAN.md (biz:market command)

---
*Phase: 03-advanced-frameworks*
*Completed: 2026-01-15*
