# Business Design

## What This Is

A markdown-based slash command system for business framework design and analysis, intended for use within the builder app. Provides Claude Code with structured prompts to create Business Model Canvas, Lean Canvas, Value Proposition Canvas, SWOT Analysis, User Personas, Competitive Analysis, and Market Sizing — all stored locally in `.planning/` directories.

## Core Value

Complete business framework coverage with full field fidelity — every framework captures all essential business design elements without shortcuts or compression.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Business Model Canvas command — 9 building blocks (Customer Segments, Value Propositions, Channels, Customer Relationships, Revenue Streams, Key Resources, Key Activities, Key Partnerships, Cost Structure)
- [ ] Lean Canvas command — startup-focused adaptation (Problem, Solution, Key Metrics, Unique Value Proposition, Unfair Advantage, Channels, Customer Segments, Cost Structure, Revenue Streams)
- [ ] Value Proposition Canvas command — customer-value fit (Customer Jobs, Pains, Gains ↔ Products/Services, Pain Relievers, Gain Creators)
- [ ] SWOT Analysis command — strategic positioning (Strengths, Weaknesses, Opportunities, Threats)
- [ ] User Personas command — customer archetypes (Demographics, Behaviors, Goals, Frustrations, Context)
- [ ] Competitive Analysis command — market landscape (Competitors, Positioning, Features, Pricing, Strengths/Weaknesses)
- [ ] Market Sizing command — opportunity quantification (TAM, SAM, SOM with methodology)
- [ ] Project management commands — create, list, view projects
- [ ] Web research integration — use Claude's WebSearch for AI-powered business research
- [ ] Entity linking — relationships between frameworks (canvas → personas, SWOT → competitive analysis)

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
| Markdown commands over MCP | Builder app needs /dev integration, not MCP server | — Pending |
| Local project storage | Keep business artifacts with their projects | — Pending |
| WebSearch over OpenAI | Reduce external dependencies, leverage Claude's built-in capabilities | — Pending |

---
*Last updated: 2026-01-15 after initialization*
