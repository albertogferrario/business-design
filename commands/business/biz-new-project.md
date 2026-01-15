---
name: biz:new-project
description: Initialize a business design project within .planning/business/
allowed-tools: [Read, Write, Bash, Glob]
---

<objective>
Initialize a new business design project by creating the `.planning/business/` directory structure and project metadata file.

This command sets up the foundation for storing business frameworks (Business Model Canvas, Lean Canvas, SWOT, etc.) within a project's planning directory.
</objective>

<context>
**Storage location:** `.planning/business/`
**Project file:** `.planning/business/project.json`

This command should be run once per project to initialize the business design workspace.
</context>

<process>

<step name="check_existing">
Check if a business design project already exists:

```bash
ls .planning/business/project.json 2>/dev/null
```

**If file exists:**
Read the existing project and present:
```
Business design project already initialized.

Project: {name}
Created: {createdAt}
Entities: {count}

Use `biz:list` to view frameworks or other biz: commands to add new ones.
```

Stop execution - do not reinitialize.

**If file does not exist:**
Continue to gather_info step.
</step>

<step name="gather_info">
Ask user for project details:

```
Initialize Business Design Project

What is this business/product?

Name: [required - short identifier]
Description: [optional - what does it do, who is it for]
```

Wait for user response before proceeding.
</step>

<step name="create_structure">
Create the directory structure:

```bash
mkdir -p .planning/business
```

Generate project ID using timestamp format: `YYYYMMDD-HHMMSS`

Create `.planning/business/project.json`:

```json
{
  "id": "[generated-id]",
  "name": "[user provided]",
  "description": "[user provided or empty string]",
  "createdAt": "[ISO timestamp]",
  "updatedAt": "[ISO timestamp]",
  "entities": []
}
```
</step>

<step name="confirm">
Display confirmation:

```
Business design project initialized.

Project: {name}
Location: .planning/business/
Created: {createdAt}

Next steps:
- `biz:bmc` — Create Business Model Canvas
- `biz:lean` — Create Lean Canvas
- `biz:vpc` — Create Value Proposition Canvas
- `biz:swot` — Create SWOT Analysis
- `biz:persona` — Create User Persona
- `biz:competitive` — Create Competitive Analysis
- `biz:market` — Create Market Sizing

Use `biz:list` to view all frameworks in this project.
```
</step>

</process>

<output>
**Created:**
- `.planning/business/` directory
- `.planning/business/project.json` with project metadata

**Project structure:**
```
.planning/
  business/
    project.json          # Project metadata
    bmc/                   # Business Model Canvas entities (created by biz:bmc)
    lean/                  # Lean Canvas entities (created by biz:lean)
    vpc/                   # Value Proposition Canvas entities (created by biz:vpc)
    swot/                  # SWOT Analysis entities (created by biz:swot)
    persona/               # User Personas (created by biz:persona)
    competitive/           # Competitive Analysis (created by biz:competitive)
    market/                # Market Sizing (created by biz:market)
```
</output>

<success_criteria>
- `.planning/business/` directory exists
- `.planning/business/project.json` contains valid project metadata
- Project ID follows timestamp format
- User received confirmation with next steps
</success_criteria>
