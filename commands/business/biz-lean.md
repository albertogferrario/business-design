---
name: biz:lean
description: Create a Lean Canvas for startup-focused business modeling
allowed-tools: [Read, Write, Bash, Glob, AskUserQuestion]
---

<objective>
Create a Lean Canvas through conversational prompting, storing the result as JSON data with a markdown summary output.

The Lean Canvas is a startup-focused adaptation of the Business Model Canvas, emphasizing problem-solution fit and key metrics. This command guides the user through each section using conversation-style prompting.
</objective>

<context>
**Storage location:** `.planning/business/lean/`
**Project file:** `.planning/business/project.json`

**Type definitions:** See lib/types.md (Lean Canvas section)
**Prompting patterns:** See lib/prompts.md (Lean Canvas section)
**Output format:** See lib/formatting.md (Lean Canvas section)
</context>

<process>

<step name="check_project">
Verify a business design project exists:

```bash
cat .planning/business/project.json 2>/dev/null
```

**If file does not exist:**
```
No business design project found.

Run `biz:new-project` first to initialize your business design workspace.
```

Stop execution - do not proceed without initialized project.

**If file exists:**
Read project data and continue to gather_info step.
</step>

<step name="gather_info">
Use conversational prompting to gather Lean Canvas information.

**Entry point:**
```
Create Lean Canvas

What problem are you solving?

Tell me about the problem your product or business addresses.
```

Wait for user response, then follow the thread based on what they share.

**Follow-up sequence** (adapt based on user responses):

1. **Problem** — "Who has this problem? What are they doing about it today?"
2. **Customer Segments** — "Who has this problem most acutely?" (identify early adopters)
3. **Unique Value Proposition** — "In one sentence, why would someone switch to you?"
4. **Solution** — "What features directly address the problem?"
5. **Channels** — "How will you reach early adopters?"
6. **Revenue Streams** — "How will you make money? What will people pay?"
7. **Cost Structure** — "What are your fixed and variable costs?"
8. **Key Metrics** — "What numbers will tell you you're succeeding?"
9. **Unfair Advantage** — "What makes you hard to copy?"

**Probing questions** (use when appropriate):
- For problem: "How painful is this? What do they sacrifice by not solving it?"
- For UVP: "What's the high-level concept? Twitter for X, Uber for Y?"
- For unfair advantage: "Is this truly defensible? Could a competitor acquire it?"

**Decision gate:**
After gathering sufficient information, present:

```
Ready to create your Lean Canvas?

Options:
1. **Create now** — I have enough to build a solid Lean Canvas
2. **Ask more questions** — Probe deeper on specific areas
3. **Let me add context** — I want to share more details first
```

Wait for user selection before proceeding.
</step>

<step name="create_entity">
Generate the Lean Canvas entity.

**1. Generate ID:**
```bash
date +"%Y%m%d-%H%M%S"
```

**2. Create directory if needed:**
```bash
mkdir -p .planning/business/lean
```

**3. Create JSON file** (`.planning/business/lean/{id}.json`):

```json
{
  "id": "[generated-id]",
  "projectId": "[from project.json]",
  "type": "lean",
  "name": "[derived from UVP or user-provided]",
  "description": "[optional description]",
  "createdAt": "[ISO timestamp]",
  "updatedAt": "[ISO timestamp]",
  "linkedEntities": [],

  "problem": [
    {
      "problem": "[problem statement]",
      "existingAlternatives": "[current solutions]"
    }
  ],

  "customerSegments": [
    {
      "segment": "[segment name]",
      "earlyAdopters": "[early adopter characteristics]"
    }
  ],

  "uniqueValueProposition": {
    "proposition": "[single clear message]",
    "highLevelConcept": "[X for Y analogy]"
  },

  "solution": [
    {
      "feature": "[feature name]",
      "description": "[how it solves the problem]"
    }
  ],

  "channels": ["[channel names]"],

  "revenueStreams": [
    {
      "stream": "[revenue source]",
      "amount": "[expected amount or range]"
    }
  ],

  "costStructure": [
    {
      "cost": "[cost item]",
      "amount": "[expected amount or range]"
    }
  ],

  "keyMetrics": [
    {
      "metric": "[metric name]",
      "target": "[target value]"
    }
  ],

  "unfairAdvantage": "[competitive moat]"
}
```

**4. Create markdown summary** (`.planning/business/lean/{id}.md`):

```markdown
# Lean Canvas: {name}

{description}

---

## Problem
- **{problem}**
  - Existing alternatives: {alternatives}

## Customer Segments
- **{segment}**
  - Early adopters: {early adopters}

## Unique Value Proposition
**{proposition}**

High-level concept: {concept}

## Solution
- **{feature}** - {description}

## Channels
- {channel}

## Revenue Streams
- {stream} ({amount})

## Cost Structure
- {cost} ({amount})

## Key Metrics
- **{metric}** - Target: {target}

## Unfair Advantage
{unfair advantage text}

---
*Created: {date}*
```

Omit empty/optional fields rather than showing placeholders.
</step>

<step name="update_project">
Add entity reference to project.json:

**1. Read current project.json**

**2. Add to entities array:**
```json
{
  "id": "[entity-id]",
  "type": "lean",
  "name": "[entity-name]",
  "createdAt": "[ISO timestamp]"
}
```

**3. Update project's updatedAt timestamp**

**4. Write updated project.json**
</step>

<step name="confirm">
Display confirmation with entity details:

```
Lean Canvas created.

Name: {name}
ID: {id}
Location: .planning/business/lean/{id}.md

Summary:
- Problem: {problem count} identified
- Customer Segments: {segment count} defined
- UVP: {proposition preview}
- Solutions: {solution count} proposed
- Key Metrics: {metric count} tracked

Next steps:
- `biz:bmc` — Create Business Model Canvas
- `biz:vpc` — Create Value Proposition Canvas
- `biz:swot` — Create SWOT Analysis
- `biz:list` — View all frameworks

View full canvas: cat .planning/business/lean/{id}.md
```
</step>

</process>

<output>
**Created:**
- `.planning/business/lean/{id}.json` — Structured data
- `.planning/business/lean/{id}.md` — Human-readable summary

**Updated:**
- `.planning/business/project.json` — Entity reference added
</output>

<success_criteria>
- `.planning/business/project.json` exists (prerequisite)
- Lean Canvas JSON file created with valid structure
- Lean Canvas markdown file created with readable format
- Entity reference added to project.json entities array
- User received confirmation with next steps
</success_criteria>
