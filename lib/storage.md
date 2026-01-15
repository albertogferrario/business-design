# Storage Patterns

Reference document defining storage patterns and file structures for business framework data.

## Storage Location

All business design data is stored in `.planning/business/` within project directories.

```
.planning/
  business/
    project.json              # Project metadata
    {entity-type}/            # Entity directories by framework type
      {id}.json               # Entity data
      {id}.md                 # Human-readable summary
```

## File Naming Conventions

### Project File

```
.planning/business/project.json
```

Single project metadata file per workspace.

### Entity Directories

Framework types and their directories:

| Framework | Directory | Example |
|-----------|-----------|---------|
| Business Model Canvas | `bmc/` | `bmc/20260115-143022.json` |
| Lean Canvas | `lean/` | `lean/20260115-143022.json` |
| Value Proposition Canvas | `vpc/` | `vpc/20260115-143022.json` |
| SWOT Analysis | `swot/` | `swot/20260115-143022.json` |
| User Persona | `persona/` | `persona/20260115-143022.json` |
| Competitive Analysis | `competitive/` | `competitive/20260115-143022.json` |
| Market Sizing | `market/` | `market/20260115-143022.json` |

### Entity Files

Each entity has two files:

- `{id}.json` — Structured data for programmatic use
- `{id}.md` — Human-readable summary for quick reference

## ID Generation

IDs use timestamp format for natural chronological ordering:

```
YYYYMMDD-HHMMSS
```

**Examples:**
- `20260115-143022` — January 15, 2026 at 14:30:22
- `20260203-091500` — February 3, 2026 at 09:15:00

**Generation:**

```bash
date +"%Y%m%d-%H%M%S"
```

## JSON Structure Templates

### Project

```json
{
  "id": "20260115-143022",
  "name": "My Business",
  "description": "Optional description of the business",
  "createdAt": "2026-01-15T14:30:22Z",
  "updatedAt": "2026-01-15T14:30:22Z",
  "entities": [
    {
      "id": "20260115-144500",
      "type": "bmc",
      "name": "Initial BMC",
      "createdAt": "2026-01-15T14:45:00Z"
    }
  ]
}
```

### Entity Reference (in project.json)

```json
{
  "id": "20260115-144500",
  "type": "bmc",
  "name": "Initial BMC",
  "createdAt": "2026-01-15T14:45:00Z"
}
```

### Entity File (e.g., bmc/20260115-144500.json)

```json
{
  "id": "20260115-144500",
  "projectId": "20260115-143022",
  "type": "bmc",
  "name": "Initial BMC",
  "description": "First iteration of our business model",
  "createdAt": "2026-01-15T14:45:00Z",
  "updatedAt": "2026-01-15T14:45:00Z",
  "linkedEntities": [],

  "customerSegments": [...],
  "valuePropositions": [...],
  // ... framework-specific fields
}
```

## File Operations

### Reading Project

```bash
cat .planning/business/project.json
```

Use the Read tool to access project metadata.

### Writing Project

Use the Write tool to create or update `project.json`. Always update `updatedAt` timestamp when modifying.

### Creating Entity

1. Generate ID: `date +"%Y%m%d-%H%M%S"`
2. Create directory if needed: `mkdir -p .planning/business/{type}`
3. Write entity JSON: `.planning/business/{type}/{id}.json`
4. Write entity summary: `.planning/business/{type}/{id}.md`
5. Add entity reference to `project.json` entities array

### Listing Entities

```bash
ls .planning/business/{type}/*.json 2>/dev/null
```

Or use Glob tool:

```
pattern: .planning/business/**/*.json
```

### Reading Entity

```bash
cat .planning/business/{type}/{id}.json
```

Use the Read tool for entity data access.

## Timestamps

All timestamps use ISO 8601 format in UTC:

```
2026-01-15T14:30:22Z
```

**Generation:**

```bash
date -u +"%Y-%m-%dT%H:%M:%SZ"
```

## Linked Entities

Entities can reference other entities via `linkedEntities` array:

```json
{
  "linkedEntities": [
    {
      "id": "20260115-150000",
      "type": "persona",
      "relationship": "targets"
    },
    {
      "id": "20260115-151000",
      "type": "swot",
      "relationship": "informs"
    }
  ]
}
```

**Relationship types:**
- `targets` — This entity targets the linked entity (e.g., BMC → Persona)
- `informs` — This entity is informed by the linked entity (e.g., BMC → SWOT)
- `complements` — Entities are complementary analyses
- `validates` — This entity validates the linked entity

## Error Handling

### Missing Directory

If `.planning/business/` doesn't exist:
- Prompt user to run `biz:new-project` first
- Do not create directories implicitly from entity commands

### Missing Project

If `project.json` doesn't exist:
- Project is not initialized
- Guide user to `biz:new-project`

### Duplicate IDs

Timestamp-based IDs should be unique. If collision occurs (same second):
- Append milliseconds or random suffix
- Example: `20260115-143022-a7f`
