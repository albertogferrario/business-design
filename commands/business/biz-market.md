---
name: biz:market
description: Create a Market Sizing analysis
allowed-tools: [Read, Write, Bash, Glob, AskUserQuestion]
---

<objective>
Create a Market Sizing analysis through guided conversation, storing the data as JSON with a markdown summary for human readability.

Market Sizing quantifies market opportunity using the TAM, SAM, SOM methodology:
- TAM (Total Addressable Market): The total market demand for your product/service if you had 100% market share
- SAM (Serviceable Addressable Market): The portion of TAM that you can realistically address with your current offering
- SOM (Serviceable Obtainable Market): The portion of SAM that you can capture in a given timeframe
</objective>

<context>
**Type definitions:** See lib/types.md section "7. Market Sizing"
**Prompting patterns:** See lib/prompts.md section "Market Sizing"
**Output formatting:** See lib/formatting.md section "Market Sizing"
**Storage patterns:** See lib/storage.md
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
Read the project to get project ID and context:
```bash
cat .planning/business/project.json
```

Continue to gather_info step.
</step>

<step name="gather_info">
Gather Market Sizing information through conversation.

**Entry point:**
```
Create Market Sizing Analysis

What's the total market for what you're offering?

(For specific numbers, use format like "$50M USD annually" or "10 million users")
```

Wait for user response.

**Follow-up sequence:** TAM → SAM → SOM progression:

1. **TAM (Total Addressable Market)** — "If you had 100% of the market, what would that be worth?"
   - Probe for specific value, currency, and time unit
   - Ask about methodology: "Are you using top-down (industry reports) or bottom-up (customer count × price)?"
   - Request sources: "What data sources support this estimate?"
   - Capture assumptions: "What key assumptions are you making?"

2. **SAM (Serviceable Addressable Market)** — "What portion can you realistically address with your current offering?"
   - Probe for constraints: "What limits your addressable market? Geographic? Technical? Regulatory?"
   - Identify target segments: "Which specific segments can you reach?"

3. **SOM (Serviceable Obtainable Market)** — "What can you capture in the next [timeframe]?"
   - Clarify timeframe: "Are we talking 1 year? 3 years? 5 years?"
   - Explore capture strategy: "How will you capture this market share?"
   - Capture assumptions: "What assumptions are you making about capture rate?"

4. **Growth Rate** — "What's the market growth rate? What's driving that growth?"
   - Probe for rate and period: "Is that annual? CAGR over 5 years?"
   - Identify growth drivers: "What factors are driving this growth?"

**Numeric guidance:**
Guide users to provide specific numbers rather than vague ranges:
- Good: "$50M USD annually", "10 million potential customers at $50 ARPU"
- Needs clarification: "large market", "significant opportunity", "millions of dollars"

**Probing questions (use as needed):**
- "What methodology are you using — top-down (industry reports) or bottom-up (customer count × price)?"
- "What are your sources for these numbers?"
- "What key assumptions are you making?"
- "What's driving this market growth?"

**Decision gate:**
After gathering sufficient information, present:

```
Ready to create your Market Sizing analysis?

Options:
1. **Create now** — I have enough to build a solid analysis
2. **Ask more questions** — Probe deeper on specific areas
3. **Let me add context** — I want to share more details first
```

Wait for user decision before proceeding.
</step>

<step name="create_entity">
Create the Market Sizing entity files.

**1. Generate entity ID:**
```bash
date +"%Y%m%d-%H%M%S"
```

**2. Create directory if needed:**
```bash
mkdir -p .planning/business/market
```

**3. Generate timestamp:**
```bash
date -u +"%Y-%m-%dT%H:%M:%SZ"
```

**4. Write JSON file** (`.planning/business/market/{id}.json`):

Structure per lib/types.md:
```json
{
  "id": "{generated-id}",
  "projectId": "{from project.json}",
  "type": "market",
  "name": "{user-provided or 'Market Sizing Analysis'}",
  "description": "{optional description}",
  "createdAt": "{ISO timestamp}",
  "updatedAt": "{ISO timestamp}",
  "linkedEntities": [],

  "tam": {
    "value": {numeric value},
    "currency": "{currency code e.g., USD}",
    "unit": "{Annual|Monthly|etc.}",
    "methodology": "{Top-down|Bottom-up}",
    "sources": ["{source1}", "{source2}"],
    "assumptions": ["{assumption1}", "{assumption2}"]
  },

  "sam": {
    "value": {numeric value},
    "currency": "{currency code}",
    "unit": "{Annual|Monthly|etc.}",
    "constraints": ["{constraint1}", "{constraint2}"],
    "targetSegments": ["{segment1}", "{segment2}"]
  },

  "som": {
    "value": {numeric value},
    "currency": "{currency code}",
    "unit": "{Annual|Monthly|etc.}",
    "timeframe": "{timeframe e.g., 3 years}",
    "captureStrategy": "{strategy description}",
    "assumptions": ["{assumption1}", "{assumption2}"]
  },

  "growthRate": {
    "rate": {numeric percentage},
    "period": "{Annual|5-year CAGR|etc.}",
    "drivers": ["{driver1}", "{driver2}"]
  }
}
```

**5. Write markdown summary** (`.planning/business/market/{id}.md`):

Format per lib/formatting.md:
```markdown
# Market Sizing: {name}

{description}

---

## TAM (Total Addressable Market)
**{currency} {formatted value}** {unit}

Methodology: {methodology}

Sources: {source1}, {source2}

Assumptions:
- {assumption1}
- {assumption2}

## SAM (Serviceable Addressable Market)
**{currency} {formatted value}** {unit}

Constraints: {constraint1}, {constraint2}

Target Segments: {segment1}, {segment2}

## SOM (Serviceable Obtainable Market)
**{currency} {formatted value}** {unit}

Timeframe: {timeframe}

Capture Strategy: {strategy}

Assumptions:
- {assumption1}
- {assumption2}

## Growth Rate
**{rate}%** {period}

Drivers: {driver1}, {driver2}

---
*Created: {date}*
```

Format currency values appropriately:
- $1.2B (billions)
- $450M (millions)
- $25K (thousands)

Omit fields with no value.
</step>

<step name="update_project">
Add entity reference to project.json:

**1. Read current project.json**

**2. Add to entities array:**
```json
{
  "id": "{entity-id}",
  "type": "market",
  "name": "{entity name}",
  "createdAt": "{ISO timestamp}"
}
```

**3. Update project's updatedAt timestamp**

**4. Write updated project.json**
</step>

<step name="confirm">
Display confirmation:

```
Market Sizing Analysis created.

Name: {name}
ID: {id}
Location: .planning/business/market/

Files:
- {id}.json — Structured data
- {id}.md — Human-readable summary

Your analysis covers:
- TAM: {formatted TAM value} {currency} {unit}
- SAM: {formatted SAM value} {currency} {unit}
- SOM: {formatted SOM value} {currency} {unit}
- Growth Rate: {rate}% {period}

Next steps:
- `biz:bmc` — Create Business Model Canvas
- `biz:competitive` — Create Competitive Analysis
- `biz:persona` — Create User Persona
- `biz:list` — View all frameworks in this project
```
</step>

</process>

<output>
**Created:**
- `.planning/business/market/{id}.json` — Market Sizing entity data
- `.planning/business/market/{id}.md` — Human-readable summary

**Updated:**
- `.planning/business/project.json` — Added entity reference
</output>

<success_criteria>
- `.planning/business/project.json` exists (prerequisite)
- Market Sizing JSON file created with valid structure per lib/types.md
- Market Sizing markdown summary created per lib/formatting.md
- Project.json updated with entity reference
- Entity ID follows timestamp format (YYYYMMDD-HHMMSS)
- TAM, SAM, SOM all have numeric values with currency and unit
- Growth rate captured if provided
</success_criteria>
