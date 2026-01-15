---
name: biz:link
description: Create or remove relationships between business design entities
allowed-tools: [Read, Write, Bash, Glob]
---

<objective>
Create or remove relationships between business design entities.

This command establishes links between frameworks to express connections like "this BMC targets this Persona" or "this VPC informs this SWOT". Links are stored in the source entity's linkedEntities array.

**Usage:**
- `biz:link [source-id] [target-id]` — Create a link
- `biz:link --remove [source-id] [target-id]` — Remove a link
</objective>

<context>
**Storage location:** `.planning/business/`
**Project file:** `.planning/business/project.json`
**Entity directories:** bmc/, lean/, vpc/, swot/, persona/, competitive/, market/

**Relationship types:**
- `targets` — Source targets the linked entity (e.g., BMC targeting a Persona)
- `informs` — Source is informed by linked entity (e.g., BMC informed by SWOT)
- `complements` — Complementary analyses (e.g., Lean Canvas complements BMC)
- `validates` — Source validates linked entity (e.g., VPC validates Persona assumptions)
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

Continue to parse_arguments step.
</step>

<step name="parse_arguments">
Parse command arguments.

**Check for --remove flag:**
- If `--remove` present: Set mode to "remove", extract source-id and target-id
- Otherwise: Set mode to "add", extract source-id and target-id

**If source-id or target-id missing:**
```
Usage:
  biz:link <source-id> <target-id>          — Create a link
  biz:link --remove <source-id> <target-id> — Remove a link

Run `biz:list` to see available entity IDs.
```

Wait for user to provide IDs or run biz:list.

**If both IDs provided:**
Continue to validate_entities step.
</step>

<step name="validate_entities">
Validate both source and target entities exist in project.json.

**For each ID (source and target):**
- Search project.json entities array for matching id
- If found, record the entity type and name

**If source not found:**
```
Source entity "{source-id}" not found.

Available entities:
{list each entity: "- {id}: {name} ({type})"}

Use `biz:list` for full details.
```

Stop execution.

**If target not found:**
```
Target entity "{target-id}" not found.

Available entities:
{list each entity: "- {id}: {name} ({type})"}

Use `biz:list` for full details.
```

Stop execution.

**If source equals target:**
```
Cannot link entity to itself.

Provide two different entity IDs:
  biz:link <source-id> <target-id>
```

Stop execution.

**If both valid and different:**
Continue based on mode (add or remove).
</step>

<step name="add_link">
**Mode: add (no --remove flag)**

Read the source entity JSON:
`.planning/business/{source-type}/{source-id}.json`

**Check for duplicate link:**
Search linkedEntities array for existing link to target-id.

**If link already exists:**
```
Link already exists between {source-name} and {target-name}.

Existing relationship: {relationship}

To change the relationship, remove the existing link first:
  biz:link --remove {source-id} {target-id}
```

Stop execution.

**If no duplicate:**
Continue to relationship_selection step.
</step>

<step name="relationship_selection">
Ask user to select relationship type:

```
Link: {source-name} ({source-type}) -> {target-name} ({target-type})

Select relationship type:

1. targets     — Source targets the linked entity
               Example: BMC targets a Persona

2. informs     — Source is informed by linked entity
               Example: BMC informed by SWOT analysis

3. complements — Complementary analyses
               Example: Lean Canvas complements BMC

4. validates   — Source validates linked entity
               Example: VPC validates Persona assumptions

Enter number or relationship name:
```

Wait for user selection.

**Validate selection:**
- Accept: 1, 2, 3, 4, targets, informs, complements, validates
- Invalid: Prompt again
</step>

<step name="create_link">
Add link to source entity's linkedEntities array:

```json
{
  "id": "{target-id}",
  "type": "{target-type}",
  "relationship": "{selected-relationship}"
}
```

Update source entity updatedAt timestamp.

Write updated source entity JSON.

Display confirmation:
```
Link created.

{source-name} ({source-type})
  └── {relationship} --> {target-name} ({target-type})

View entity: biz:view {source-id}
```
</step>

<step name="remove_link">
**Mode: remove (--remove flag present)**

Read the source entity JSON:
`.planning/business/{source-type}/{source-id}.json`

**Search linkedEntities for target:**
Find entry where id matches target-id.

**If link not found:**
```
No link exists between {source-name} and {target-name}.

View entity links: biz:view {source-id}
```

Stop execution.

**If link found:**
Remove the link entry from linkedEntities array.
Update source entity updatedAt timestamp.
Write updated source entity JSON.

Display confirmation:
```
Link removed.

{source-name} ({source-type})
  └── [removed] --> {target-name} ({target-type})

View entity: biz:view {source-id}
```
</step>

</process>

<output>
**On successful link creation:**
- Source entity JSON updated with new linkedEntities entry
- Confirmation showing source, relationship, and target

**On successful link removal:**
- Source entity JSON updated with link removed
- Confirmation showing removed link

**Stored link format in entity JSON:**
```json
{
  "linkedEntities": [
    {
      "id": "20260115-150000",
      "type": "persona",
      "relationship": "targets"
    }
  ]
}
```
</output>

<error_handling>
**No project:**
```
No business design project found.

Run `biz:new-project` first to initialize a project.
```

**Entity not found:**
```
Entity "{id}" not found.

Available entities:
- {id}: {name} ({type})
...

Use `biz:list` for full details.
```

**Self-link:**
```
Cannot link entity to itself.

Provide two different entity IDs:
  biz:link <source-id> <target-id>
```

**Duplicate link:**
```
Link already exists between {source-name} and {target-name}.

Existing relationship: {relationship}

To change the relationship, remove the existing link first:
  biz:link --remove {source-id} {target-id}
```

**No link to remove:**
```
No link exists between {source-name} and {target-name}.

View entity links: biz:view {source-id}
```
</error_handling>

<success_criteria>
- Project existence verified before linking
- Both source and target IDs validated
- Self-link prevented
- Duplicate link prevented with helpful guidance
- Four relationship types available (targets, informs, complements, validates)
- Links stored in linkedEntities array format matching lib/types.md
- --remove flag removes existing links
- Missing link handled with clear message
- Entity updatedAt timestamp updated on change
- Confirmation displays both entities and relationship
</success_criteria>
