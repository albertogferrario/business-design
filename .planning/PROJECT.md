# Business Design

## What This Is

A markdown-based slash command system for business framework design and analysis, intended for use within the builder app. Provides Claude Code with structured prompts to create Business Model Canvas, Lean Canvas, Value Proposition Canvas, SWOT Analysis, User Personas, Competitive Analysis, and Market Sizing — all stored locally in `.planning/` directories.

## Core Value

Complete business framework coverage with full field fidelity — every framework captures all essential business design elements without shortcuts or compression.

## Requirements

### Validated

- ✓ Business Model Canvas command — v1.0
- ✓ Lean Canvas command — v1.0
- ✓ Value Proposition Canvas command — v1.0
- ✓ SWOT Analysis command — v1.0
- ✓ User Personas command — v1.0
- ✓ Competitive Analysis command — v1.0
- ✓ Market Sizing command — v1.0
- ✓ Project management commands (create, list, view) — v1.0
- ✓ Web research integration (WebSearch) — v1.0
- ✓ Entity linking (targets, informs, complements, validates) — v1.0

### Active

(None — v1.0 complete)

### Out of Scope

- Export formats (PDF, presentations, formatted documents) — keep scope focused on framework creation
- Collaboration features — single user only, no sharing or team workflows
- Historical versioning — no version history or rollback for frameworks
- OpenAI integration — use Claude's built-in WebSearch instead
- MCP server implementation — this is the non-MCP alternative

## Context

This project creates a non-MCP equivalent of `business-design-mcp` for use as `/dev` commands in the builder app. The architecture follows the Conductor pattern: markdown files that define prompts and workflows which Claude Code executes directly.

**Reference implementations:**
- `../business-design-mcp/` — MCP server with the framework schemas and storage patterns
- `../conductor/` — slash command structure and file organization patterns
- `../../appolabs/builder/docs/` — builder app integration context

**Storage model:**
- All outputs stored in `.planning/business/` within project directories
- JSON files for framework data
- Markdown summaries for human readability

## Constraints

- **Architecture**: Markdown-based slash commands (like Conductor) — not a TypeScript runtime
- **Storage**: Local `.planning/` directory — not shared with business-design-mcp's `~/.business-design/`
- **Research**: Claude's WebSearch tool — not external API dependencies

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Markdown commands over MCP | Builder app needs /dev integration, not MCP server | ✓ Good |
| Local project storage | Keep business artifacts with their projects | ✓ Good |
| WebSearch over OpenAI | Reduce external dependencies, leverage Claude's built-in capabilities | ✓ Good |
| Timestamp-based IDs | Natural chronological ordering | ✓ Good |
| Dual file output (JSON + MD) | JSON for programmatic use, MD for human readability | ✓ Good |
| 5-step process pattern | Consistent UX across all framework commands | ✓ Good |
| Conversation-style prompting | Natural information gathering flow | ✓ Good |
| Four entity link types | Clear semantic relationships between frameworks | ✓ Good |

## Current State

**v1.0 shipped:** 2026-01-15

- 13 commands in `commands/business/`
- 4 lib files in `lib/`
- 4,624 lines of markdown
- All 7 business frameworks implemented
- Project management and entity linking complete
- WebSearch research integration working

---
*Last updated: 2026-01-15 after v1.0 milestone*
