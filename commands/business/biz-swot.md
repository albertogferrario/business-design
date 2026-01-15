---
name: biz:swot
description: Create a SWOT Analysis
allowed-tools: [Read, Write, Bash, Glob, AskUserQuestion]
---

<objective>
Create a SWOT Analysis through conversational prompting, examining internal strengths/weaknesses and external opportunities/threats.

Output: JSON data file + markdown summary for human readability.
</objective>

<context>
**Type definition (from lib/types.md):**
```json
{
  "strengths": [
    {
      "item": "string",              // Strength (required)
      "impact": "string?"            // High, Medium, Low
    }
  ],

  "weaknesses": [
    {
      "item": "string",              // Weakness (required)
      "impact": "string?",           // High, Medium, Low
      "mitigation": "string?"        // How to address
    }
  ],

  "opportunities": [
    {
      "item": "string",              // Opportunity (required)
      "timeframe": "string?",        // Short-term, Medium-term, Long-term
      "requiredAction": "string?"    // Action to capture
    }
  ],

  "threats": [
    {
      "item": "string",              // Threat (required)
      "likelihood": "string?",       // High, Medium, Low
      "contingency": "string?"       // Response plan
    }
  ],

  "context": "string?"               // Market/competitive context for the analysis
}
```

**Prompting flow (from lib/prompts.md):**
- Entry point: "What's the strategic decision you're analyzing?"
- Context first: Get the decision or direction being evaluated
- Internal: Strengths, then Weaknesses
- External: Opportunities, then Threats
- Decision gate: Ready to create, or explore more?

**Output format (from lib/formatting.md):**
```markdown
# SWOT Analysis: {name}

{description}

**Context:** {strategic context}

---

## Strengths
- {item} (impact: {impact})

## Weaknesses
- {item} (impact: {impact})
  - Mitigation: {mitigation}

## Opportunities
- {item} ({timeframe})
  - Action required: {action}

## Threats
- {item} (likelihood: {likelihood})
  - Contingency: {contingency}

---
*Created: {date}*
```

**Storage (from lib/storage.md):**
- Directory: `.planning/business/swot/`
- JSON file: `{id}.json`
- Markdown file: `{id}.md`
- Update project.json entities array
</context>

<process>

<step name="check_project">
Verify business design project exists:

```bash
ls .planning/business/project.json 2>/dev/null
```

**If file does not exist:**
Display error and stop:
```
No business design project found.

Run `biz:new-project` first to initialize your project.
```

**If file exists:**
Read the project to get projectId:
```bash
cat .planning/business/project.json
```
Continue to gather_info step.
</step>

<step name="gather_info">
Use conversational prompting to gather SWOT information.

**Entry point:**
```
Create SWOT Analysis

What's the strategic decision you're analyzing?

This could be:
- A market entry decision
- A product launch evaluation
- A competitive positioning assessment
- A strategic pivot consideration

Describe the decision or direction you're evaluating.
```

Wait for user response, then gather context.

**Context gathering:**
```
What timeframe are we considering for this analysis?
What's the broader market or competitive context?
```

**Internal analysis — Strengths:**
```
Let's look at internal factors first.

What internal capabilities give you an advantage?

Consider:
- Core competencies and skills
- Resources you have (financial, human, technological)
- Market position or brand recognition
- Unique processes or intellectual property
```

For each strength mentioned, optionally probe:
- "What's the impact of this strength — high, medium, or low?"

**Internal analysis — Weaknesses:**
```
What internal limitations hold you back?
What do competitors do better than you?

Consider:
- Resource gaps or constraints
- Skill or knowledge deficiencies
- Process inefficiencies
- Reputation or brand issues
```

For each weakness mentioned, optionally probe:
- "What's the impact — high, medium, or low?"
- "What could mitigate this weakness?"

**External analysis — Opportunities:**
```
Now let's look at external factors.

What external trends or changes could you benefit from?

Consider:
- Market trends or shifts
- Regulatory changes
- Technology developments
- Competitor vulnerabilities
- Customer behavior changes
```

For each opportunity mentioned, optionally probe:
- "What timeframe — short-term, medium-term, or long-term?"
- "What action would be required to capture this?"

**External analysis — Threats:**
```
What external factors could harm your position?

Consider:
- Competitive threats
- Market changes or disruptions
- Economic factors
- Regulatory risks
- Technology obsolescence
```

For each threat mentioned, optionally probe:
- "What's the likelihood — high, medium, or low?"
- "What's your contingency plan?"

**Decision gate:**
```
Ready to create your SWOT Analysis?

Options:
1. **Create now** — I have enough to build a solid analysis
2. **Ask more questions** — Probe deeper on specific areas
3. **Let me add context** — I want to share more details first
```

Wait for user decision before proceeding.
</step>

<step name="create_entity">
Generate entity ID and create files.

**Generate ID:**
```bash
date +"%Y%m%d-%H%M%S"
```

**Create directory if needed:**
```bash
mkdir -p .planning/business/swot
```

**Create JSON file** at `.planning/business/swot/{id}.json`:

```json
{
  "id": "[generated-id]",
  "projectId": "[from project.json]",
  "type": "swot",
  "name": "[from user — based on strategic decision]",
  "description": "[from user response]",
  "createdAt": "[ISO timestamp]",
  "updatedAt": "[ISO timestamp]",
  "linkedEntities": [],

  "strengths": [
    {
      "item": "[strength]",
      "impact": "[High/Medium/Low or null]"
    }
  ],

  "weaknesses": [
    {
      "item": "[weakness]",
      "impact": "[High/Medium/Low or null]",
      "mitigation": "[mitigation or null]"
    }
  ],

  "opportunities": [
    {
      "item": "[opportunity]",
      "timeframe": "[Short-term/Medium-term/Long-term or null]",
      "requiredAction": "[action or null]"
    }
  ],

  "threats": [
    {
      "item": "[threat]",
      "likelihood": "[High/Medium/Low or null]",
      "contingency": "[contingency or null]"
    }
  ],

  "context": "[strategic context from user]"
}
```

**Create markdown file** at `.planning/business/swot/{id}.md`:

```markdown
# SWOT Analysis: {name}

{description}

**Context:** {context}

---

## Strengths
- {item} (impact: {impact})
[repeat for each strength, omit impact if null]

## Weaknesses
- {item} (impact: {impact})
  - Mitigation: {mitigation}
[repeat for each weakness, omit impact/mitigation if null]

## Opportunities
- {item} ({timeframe})
  - Action required: {requiredAction}
[repeat for each opportunity, omit timeframe/action if null]

## Threats
- {item} (likelihood: {likelihood})
  - Contingency: {contingency}
[repeat for each threat, omit likelihood/contingency if null]

---
*Created: {date}*
```

Follow formatting.md rules: omit empty/optional fields rather than showing empty placeholders.
</step>

<step name="update_project">
Add entity reference to project.json entities array.

Read current project.json, add new entity reference:

```json
{
  "id": "[entity-id]",
  "type": "swot",
  "name": "[entity name]",
  "createdAt": "[ISO timestamp]"
}
```

Update project.json `updatedAt` timestamp.
Write updated project.json.
</step>

<step name="confirm">
Display confirmation:

```
SWOT Analysis created.

Name: {name}
ID: {id}
Location: .planning/business/swot/

Files created:
- .planning/business/swot/{id}.json (data)
- .planning/business/swot/{id}.md (summary)

Summary:
- Strengths: {count}
- Weaknesses: {count}
- Opportunities: {count}
- Threats: {count}

Next steps:
- `biz:list` — View all frameworks
- `biz:bmc` — Create Business Model Canvas
- `biz:lean` — Create Lean Canvas
- `biz:vpc` — Create Value Proposition Canvas
- `biz:competitive` — Create Competitive Analysis
```
</step>

</process>

<output>
**Created:**
- `.planning/business/swot/{id}.json` — Structured SWOT data
- `.planning/business/swot/{id}.md` — Human-readable summary

**Updated:**
- `.planning/business/project.json` — Added entity reference
</output>

<success_criteria>
- `.planning/business/project.json` exists (checked before proceeding)
- SWOT JSON file created with valid structure per lib/types.md
- SWOT markdown file created with proper formatting per lib/formatting.md
- At least one entry in any quadrant (per validation rules)
- Project.json updated with entity reference
- User received confirmation with file locations and next steps
</success_criteria>
