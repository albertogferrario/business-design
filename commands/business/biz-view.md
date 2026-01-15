---
name: biz:view
description: View a specific business design entity by ID
allowed-tools: [Read, Bash, Glob]
---

<objective>
Display the full details of a business design entity by its ID.

This command retrieves the entity data from storage and displays it in a human-readable format, including any linked entities.
</objective>

<context>
**Storage location:** `.planning/business/`
**Project file:** `.planning/business/project.json`
**Entity directories:** bmc/, lean/, vpc/, swot/, persona/, competitive/, market/

**Usage:** `biz:view <id>` where ID is from `biz:list` output (format: YYYYMMDD-HHMMSS)
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

Continue to validate_id step.
</step>

<step name="validate_id">
Check if the entity ID was provided and exists in the project.

**If no ID provided:**
```
Usage: biz:view <id>

Run `biz:list` to see available framework IDs.
```

Stop execution.

**If ID provided:**
Search for entity in project.json entities array:
- Match entity.id against provided ID
- If found, note the entity type

**If entity not found:**
```
Entity "{id}" not found.

Available frameworks:
{list each entity from project.json: "- {id}: {name} ({type})"}

Use one of the IDs above, or run `biz:list` for full details.
```

Stop execution.

**If entity found:**
Continue to read_entity step.
</step>

<step name="read_entity">
Read the entity data from storage.

**Construct file path:**
`.planning/business/{type}/{id}.json`

**If file missing:**
```
Entity data file missing at .planning/business/{type}/{id}.json

The entity "{id}" is registered in project.json but its data file was not found.
This may indicate data corruption or manual file deletion.
```

Stop execution.

**If file exists:**
Read the JSON data and continue to display step.
</step>

<step name="display">
Display the entity using lib/formatting.md patterns for each framework type.

**Common header:**
```
# {Framework Type}: {name}

{description if present}

**ID:** {id}
**Type:** {type display name}
**Created:** {createdAt}
**Updated:** {updatedAt}

---
```

**Framework-specific content:**

Display fields according to lib/formatting.md patterns for each type:
- BMC: Customer Segments, Value Propositions, Channels, etc.
- Lean: Problem, Solution, Unique Value Proposition, etc.
- VPC: Customer Profile (Jobs, Pains, Gains), Value Map
- SWOT: Strengths, Weaknesses, Opportunities, Threats
- Persona: Demographics, Psychographics, Behavior, Scenario
- Competitive: Competitor profiles, Our Position
- Market: TAM, SAM, SOM, Growth Rate

**Linked entities section (if any):**
```
---

## Linked Entities
- **{Type}**: {name} ({relationship})
  ID: {linked-id}
```

**Footer:**
```
---

*Location: .planning/business/{type}/{id}.json*

Commands:
- `biz:list` — View all frameworks
- `biz:link {this-id} {other-id}` — Link to another framework
```
</step>

</process>

<output>
Full formatted display of the entity's contents, including:
- Header with ID, type, and timestamps
- All framework-specific fields per lib/formatting.md
- Linked entities if present
- File location and related commands
</output>

<success_criteria>
- Project existence verified before viewing
- ID argument required and validated
- Entity lookup by ID in project.json
- Missing entity handled with available IDs listed
- Missing data file handled with clear error message
- Entity displayed using lib/formatting.md patterns
- Linked entities shown if present
- Navigation hints provided
</success_criteria>
