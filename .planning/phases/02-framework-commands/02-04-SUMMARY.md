---
phase: 02-framework-commands
plan: 04
subsystem: commands
tags: [swot, analysis, strategic-planning, business-framework]

# Dependency graph
requires:
  - phase: 01-foundation
    provides: type definitions, prompting patterns, formatting utilities, storage patterns
provides:
  - biz:swot command for SWOT Analysis creation
  - Conversational prompting for strategic analysis
  - JSON + markdown dual output for SWOT entities
affects: [03-advanced-frameworks, 04-integration]

# Tech tracking
tech-stack:
  added: []
  patterns: [SWOT quadrant gathering flow, strategic context prompting]

key-files:
  created: [commands/business/biz-swot.md]
  modified: []

key-decisions:
  - "Entry point asks for strategic decision context first"
  - "Internal factors (S/W) before external factors (O/T)"
  - "Optional probing for impact, mitigation, likelihood, contingency"

patterns-established:
  - "SWOT prompting: context -> internal -> external -> decision gate"
  - "Four-quadrant analysis with optional metadata per item"

# Metrics
duration: 1 min
completed: 2026-01-15
---

# Phase 02 Plan 04: SWOT Analysis Command Summary

**SWOT Analysis command with conversational prompting for strategic positioning, gathering internal strengths/weaknesses and external opportunities/threats**

## Performance

- **Duration:** 1 min
- **Started:** 2026-01-15T05:13:21Z
- **Completed:** 2026-01-15T05:14:20Z
- **Tasks:** 1
- **Files modified:** 1

## Accomplishments
- Created biz:swot command following established conductor pattern
- Implemented strategic decision entry point with context gathering
- Internal factors flow: Strengths then Weaknesses with impact/mitigation probing
- External factors flow: Opportunities then Threats with timeframe/likelihood probing
- Decision gate pattern for user control over depth

## Task Commits

Each task was committed atomically:

1. **Task 1: Create biz:swot command** - `41ff64d` (feat)

## Files Created/Modified
- `commands/business/biz-swot.md` - SWOT Analysis command with full process steps

## Decisions Made
- Entry point: "What's the strategic decision you're analyzing?" (per lib/prompts.md)
- Context gathered before diving into quadrants (timeframe, market context)
- Internal factors (Strengths, Weaknesses) collected before external (Opportunities, Threats)
- Optional probing for each quadrant's metadata fields (impact, mitigation, timeframe, likelihood, contingency)

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness
- Phase 2 complete: All 4 framework commands created (BMC, Lean, VPC, SWOT)
- Ready for Phase 3: Advanced Frameworks (Personas, Competitive Analysis, Market Sizing)
- All commands follow consistent conductor-style pattern

---
*Phase: 02-framework-commands*
*Completed: 2026-01-15*
