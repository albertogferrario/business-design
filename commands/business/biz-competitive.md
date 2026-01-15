---
name: biz:competitive
description: Create a Competitive Analysis
allowed-tools: [Read, Write, Bash, Glob, AskUserQuestion]
---

<objective>
Create a Competitive Analysis through guided conversation, storing the data as JSON with a markdown summary for human readability.

Competitive Analysis maps the market landscape by profiling competitors (strengths, weaknesses, positioning) and establishing your own position (differentiators, gaps, opportunities).
</objective>

<context>
**Type definitions:** See lib/types.md section "6. Competitive Analysis"
**Prompting patterns:** See lib/prompts.md section "Competitive Analysis"
**Output formatting:** See lib/formatting.md section "Competitive Analysis"
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
Gather Competitive Analysis information through conversation.

**Entry point:**
```
Create Competitive Analysis

Who are your main competitors?
```

Wait for user response.

**For each competitor mentioned, gather:**

1. **Basics** — "What's their website?"
2. **Strengths** — "What do they do well?"
3. **Weaknesses** — "Where do they fall short?"
4. **Positioning** — "Who do they target? How do they price?"

After covering competitors, transition to own position:

```
Now let's look at your position in the market.
```

5. **Differentiators** — "How are you different from these competitors?"
6. **Gaps** — "What do competitors have that you don't?"
7. **Opportunities** — "Where can you win against them?"

**Probing questions (use as needed):**
- "Do you have market share estimates for any of these?"
- "What's their pricing model — subscription, one-time, freemium?"
- "Who's their ideal customer compared to yours?"
- "Is there a competitor you're most concerned about? Why?"
- "Are there indirect competitors or substitutes to consider?"

**Decision gate:**
After gathering sufficient information, present:

```
Ready to create your Competitive Analysis?

Options:
1. **Create now** — I have enough to map the competitive landscape
2. **Ask more questions** — Probe deeper on specific competitors
3. **Let me add context** — I want to share more details first
```

Wait for user decision before proceeding.
</step>

<step name="create_entity">
Create the Competitive Analysis entity files.

**1. Generate entity ID:**
```bash
date +"%Y%m%d-%H%M%S"
```

**2. Create directory if needed:**
```bash
mkdir -p .planning/business/competitive
```

**3. Generate timestamp:**
```bash
date -u +"%Y-%m-%dT%H:%M:%SZ"
```

**4. Write JSON file** (`.planning/business/competitive/{id}.json`):

Structure per lib/types.md:
```json
{
  "id": "{generated-id}",
  "projectId": "{from project.json}",
  "type": "competitive",
  "name": "{user-provided or 'Competitive Analysis'}",
  "description": "{optional description}",
  "createdAt": "{ISO timestamp}",
  "updatedAt": "{ISO timestamp}",
  "linkedEntities": [],

  "competitors": [
    {
      "name": "{competitor name}",
      "website": "{website URL}",
      "description": "{brief description}",
      "strengths": ["{strength1}", "{strength2}"],
      "weaknesses": ["{weakness1}", "{weakness2}"],
      "pricing": "{pricing model/range}",
      "marketShare": "{market share estimate}",
      "targetAudience": "{primary target market}"
    }
  ],

  "ourPosition": {
    "differentiators": ["{differentiator1}", "{differentiator2}"],
    "gaps": ["{gap1}", "{gap2}"],
    "opportunities": ["{opportunity1}", "{opportunity2}"]
  }
}
```

**5. Write markdown summary** (`.planning/business/competitive/{id}.md`):

Format per lib/formatting.md:
```markdown
# Competitive Analysis: {name}

{description}

---

## {Competitor Name}
{website}

{description}

**Strengths:** {strength1}, {strength2}

**Weaknesses:** {weakness1}, {weakness2}

**Pricing:** {model} ({range})

**Market Share:** {share}

**Target Audience:** {audience}

---

## Our Position

**Differentiators:** {diff1}, {diff2}

**Gaps:** {gap1}, {gap2}

**Opportunities:** {opp1}, {opp2}

---
*Created: {date}*
```

Omit fields with no value.
</step>

<step name="update_project">
Add entity reference to project.json:

**1. Read current project.json**

**2. Add to entities array:**
```json
{
  "id": "{entity-id}",
  "type": "competitive",
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
Competitive Analysis created.

Name: {name}
ID: {id}
Location: .planning/business/competitive/

Files:
- {id}.json — Structured data
- {id}.md — Human-readable summary

Your analysis covers:
- {count} competitors profiled
- {count} differentiators identified
- {count} gaps to address
- {count} opportunities spotted

Next steps:
- `biz:market` — Create Market Sizing (quantify opportunity)
- `biz:swot` — Create SWOT Analysis (strategic positioning)
- `biz:persona` — Create User Persona (target customer)
- `biz:list` — View all frameworks in this project
```
</step>

</process>

<output>
**Created:**
- `.planning/business/competitive/{id}.json` — Competitive Analysis entity data
- `.planning/business/competitive/{id}.md` — Human-readable summary

**Updated:**
- `.planning/business/project.json` — Added entity reference
</output>

<success_criteria>
- `.planning/business/project.json` exists (prerequisite)
- Competitive Analysis JSON file created with valid structure per lib/types.md
- Competitive Analysis markdown summary created per lib/formatting.md
- Project.json updated with entity reference
- Entity ID follows timestamp format (YYYYMMDD-HHMMSS)
- At least one competitor with strengths and weaknesses captured
- Our position with differentiators, gaps, and opportunities captured
</success_criteria>
