---
name: biz:list
description: List all business design entities in the current project
allowed-tools: [Read, Glob]
---

<objective>
List all business design frameworks (entities) stored in the current project, displaying them in a organized table format grouped by framework type.

This command provides a quick overview of what frameworks exist, when they were created, and their IDs for use with `biz:view`.
</objective>

<context>
**Storage location:** `.planning/business/`
**Project file:** `.planning/business/project.json`
**Entity types:** bmc, lean, vpc, swot, persona, competitive, market
</context>

<process>

<step name="check_project">
Verify a business design project exists:

```bash
ls .planning/business/project.json 2>/dev/null
```

**If file does not exist:**
```
No business design project found.

Run `biz:new-project` first to initialize a project.
```

Stop execution.

**If file exists:**
Read the project metadata:
```bash
cat .planning/business/project.json
```

Continue to list_entities step.
</step>

<step name="list_entities">
Parse the entities array from project.json.

**If entities array is empty:**
```
Project: {project name}
Created: {project createdAt}

No frameworks created yet.

Get started:
- `biz:bmc` — Create Business Model Canvas
- `biz:lean` — Create Lean Canvas
- `biz:vpc` — Create Value Proposition Canvas
- `biz:swot` — Create SWOT Analysis
- `biz:persona` — Create User Persona
- `biz:competitive` — Create Competitive Analysis
- `biz:market` — Create Market Sizing
```

Stop execution.

**If entities exist:**
Continue to display_entities step.
</step>

<step name="display_entities">
Group entities by type and display in organized format.

**Framework type mappings:**

| Type | Display Name | Category |
|------|-------------|----------|
| bmc | Business Model Canvas | Canvas Frameworks |
| lean | Lean Canvas | Canvas Frameworks |
| vpc | Value Proposition Canvas | Canvas Frameworks |
| swot | SWOT Analysis | Canvas Frameworks |
| persona | User Persona | Analysis Frameworks |
| competitive | Competitive Analysis | Analysis Frameworks |
| market | Market Sizing | Analysis Frameworks |

**Display format:**

```
# Business Design: {project name}

Created: {project createdAt}
Total frameworks: {count}

---

## Canvas Frameworks

| ID | Type | Name | Created |
|----|------|------|---------|
| {id} | {display name} | {entity name} | {date} |

## Analysis Frameworks

| ID | Type | Name | Created |
|----|------|------|---------|
| {id} | {display name} | {entity name} | {date} |

---

View details: `biz:view <id>`
```

**Formatting rules:**
- Sort entities within each category by createdAt (newest first)
- Format date as YYYY-MM-DD
- Omit category sections with no entities
- ID column shows full ID for use with `biz:view`
</step>

<step name="summary">
Display count summary at the end:

```
---

Summary:
- Canvas Frameworks: {count}
- Analysis Frameworks: {count}
- Total: {total count}

Commands:
- `biz:view <id>` — View framework details
- `biz:bmc`, `biz:lean`, etc. — Create new frameworks
```
</step>

</process>

<output>
Formatted list of all entities in the project, grouped by framework type.

**Display elements:**
- Project name and creation date
- Table of entities per category
- Entity ID, type, name, and creation date
- Count summary
- Navigation hints
</output>

<success_criteria>
- Project existence checked before listing
- Empty project handled with helpful guidance
- Entities grouped by framework category
- Table format with ID, Type, Name, Created columns
- Entities sorted by creation date (newest first)
- Count summary provided
- Navigation to `biz:view` explained
</success_criteria>
