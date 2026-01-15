---
name: biz:help
description: Display available business design commands and usage guide
allowed-tools: []
---

<objective>
Display all available business design commands organized by category, with brief descriptions for each.
</objective>

<context>
Reference documents for deeper information:
- `lib/prompts.md` — How commands gather information from users
- `lib/formatting.md` — How frameworks are formatted as markdown
- `lib/types.md` — Complete field structures for all frameworks
- `lib/storage.md` — Storage patterns and file conventions
</context>

<process>

<step name="display_help">
Display the following help content:

```
# Business Design Commands

Slash commands for creating and managing business frameworks.

---

## Project Commands

| Command | Description |
|---------|-------------|
| `biz:new-project` | Initialize a business design project in `.planning/business/` |
| `biz:list` | List all frameworks in the current project |
| `biz:view <id>` | View a specific framework by ID |
| `biz:link <source> <target>` | Create relationship between frameworks |

---

## Canvas Frameworks

| Command | Description |
|---------|-------------|
| `biz:bmc` | Create a Business Model Canvas (9 building blocks) |
| `biz:lean` | Create a Lean Canvas (startup-focused) |
| `biz:vpc` | Create a Value Proposition Canvas (customer-value fit) |
| `biz:swot` | Create a SWOT Analysis (strategic positioning) |

---

## Analysis Frameworks

| Command | Description |
|---------|-------------|
| `biz:persona` | Create a User Persona (customer archetype) |
| `biz:competitive` | Create a Competitive Analysis (market landscape) |
| `biz:market-sizing` | Create a Market Sizing estimate (TAM/SAM/SOM) |

---

## Research

| Command | Description |
|---------|-------------|
| `biz:research <topic>` | Research a business topic using WebSearch |

---

## Quick Start

1. Run `biz:new-project` to initialize
2. Create frameworks with `biz:bmc`, `biz:lean`, etc.
3. Link related frameworks with `biz:link`
4. View your work with `biz:list` and `biz:view`

---

## Storage

All frameworks are stored in `.planning/business/` within your project:
- `project.json` — Project metadata
- `{type}/{id}.json` — Framework data
- `{type}/{id}.md` — Human-readable summary

---

## References

For detailed documentation:
- Prompting patterns: `lib/prompts.md`
- Output formatting: `lib/formatting.md`
- Type definitions: `lib/types.md`
- Storage conventions: `lib/storage.md`
```

</step>

</process>

<output>
Help text displayed to user showing all available commands.
</output>

<success_criteria>
- All command categories displayed (Project, Canvas, Analysis, Research)
- Each command has name and brief description
- Quick start guide provided
- References to lib/ documentation included
</success_criteria>
