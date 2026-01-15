---
name: biz:bmc
description: Create a Business Model Canvas
allowed-tools: [Read, Write, Bash, Glob, AskUserQuestion]
---

<objective>
Create a Business Model Canvas (BMC) through guided conversation, storing the data as JSON with a markdown summary for human readability.

The Business Model Canvas describes how an organization creates, delivers, and captures value through 9 building blocks: Customer Segments, Value Propositions, Channels, Customer Relationships, Revenue Streams, Key Resources, Key Activities, Key Partnerships, and Cost Structure.
</objective>

<context>
**Type definitions:** See lib/types.md section "1. Business Model Canvas (BMC)"
**Prompting patterns:** See lib/prompts.md section "Business Model Canvas"
**Output formatting:** See lib/formatting.md section "Business Model Canvas"
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
Gather BMC information through conversation.

**Entry point:**
```
Create Business Model Canvas

What's the core value you deliver to customers?
```

Wait for user response.

**Follow-up sequence:** Use the canvas order as a guide, but follow user's thread:

1. **Value Propositions** — "What problems do you solve or needs do you meet?"
2. **Customer Segments** — "Who are your customers? Are there distinct groups?"
3. **Channels** — "How do customers find out about you and receive your value?"
4. **Customer Relationships** — "What type of relationship do you establish with each segment?"
5. **Revenue Streams** — "How do you make money? What are customers willing to pay for?"
6. **Key Resources** — "What assets are essential to deliver your value proposition?"
7. **Key Activities** — "What do you need to do really well to succeed?"
8. **Key Partnerships** — "Who are your critical suppliers and partners?"
9. **Cost Structure** — "What are your major costs? Fixed? Variable?"

**Probing questions (use as needed):**
- For segments: "What size is this segment?" "What characterizes them?"
- For channels: "What phase — awareness, evaluation, purchase, delivery, after-sales?"
- For partnerships: "What's the motivation — optimization, economy of scale, risk reduction?"
- For costs: "Is this fixed or variable?"

**Decision gate:**
After gathering sufficient information, present:

```
Ready to create your Business Model Canvas?

Options:
1. **Create now** — I have enough to build a solid canvas
2. **Ask more questions** — Probe deeper on specific areas
3. **Let me add context** — I want to share more details first
```

Wait for user decision before proceeding.
</step>

<step name="create_entity">
Create the BMC entity files.

**1. Generate entity ID:**
```bash
date +"%Y%m%d-%H%M%S"
```

**2. Create directory if needed:**
```bash
mkdir -p .planning/business/bmc
```

**3. Generate timestamp:**
```bash
date -u +"%Y-%m-%dT%H:%M:%SZ"
```

**4. Write JSON file** (`.planning/business/bmc/{id}.json`):

Structure per lib/types.md:
```json
{
  "id": "{generated-id}",
  "projectId": "{from project.json}",
  "type": "bmc",
  "name": "{user-provided or 'Business Model Canvas'}",
  "description": "{optional description}",
  "createdAt": "{ISO timestamp}",
  "updatedAt": "{ISO timestamp}",
  "linkedEntities": [],

  "customerSegments": [
    {
      "segment": "{segment name}",
      "characteristics": "{key characteristics}",
      "size": "{market size estimate}"
    }
  ],

  "valuePropositions": [
    {
      "proposition": "{value proposition}",
      "customerPains": "{pains addressed}",
      "customerGains": "{gains created}"
    }
  ],

  "channels": [
    {
      "channel": "{channel name}",
      "phase": "{Awareness|Evaluation|Purchase|Delivery|After Sales}"
    }
  ],

  "customerRelationships": [
    {
      "relationship": "{relationship description}",
      "type": "{Personal|Self-service|Automated|Communities|Co-creation}"
    }
  ],

  "revenueStreams": [
    {
      "stream": "{revenue stream}",
      "type": "{Asset sale|Usage fee|Subscription|Lending|Licensing|Advertising}",
      "pricing": "{pricing mechanism}"
    }
  ],

  "keyResources": [
    {
      "resource": "{resource name}",
      "type": "{Physical|Intellectual|Human|Financial}"
    }
  ],

  "keyActivities": [
    {
      "activity": "{activity name}",
      "type": "{Production|Problem Solving|Platform/Network}"
    }
  ],

  "keyPartnerships": [
    {
      "partner": "{partner name}",
      "type": "{Strategic Alliance|Coopetition|Joint Venture|Supplier}",
      "motivation": "{Optimization|Risk reduction|Resource acquisition}"
    }
  ],

  "costStructure": [
    {
      "cost": "{cost item}",
      "type": "{Fixed|Variable}"
    }
  ]
}
```

**5. Write markdown summary** (`.planning/business/bmc/{id}.md`):

Format per lib/formatting.md:
```markdown
# Business Model Canvas: {name}

{description}

---

## Customer Segments
- **{segment}** - {characteristics} ({size})

## Value Propositions
- **{proposition}**
  - Pains addressed: {pain1}, {pain2}
  - Gains created: {gain1}, {gain2}

## Channels
- {channel} ({phase})

## Customer Relationships
- {relationship} ({type})

## Revenue Streams
- **{stream}** - {type} ({pricing})

## Key Resources
- {resource} ({type})

## Key Activities
- {activity} ({type})

## Key Partnerships
- **{partner}** - {type}: {motivation}

## Cost Structure
- {cost} ({type})

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
  "type": "bmc",
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
Business Model Canvas created.

Name: {name}
ID: {id}
Location: .planning/business/bmc/

Files:
- {id}.json — Structured data
- {id}.md — Human-readable summary

Your canvas covers:
- {count} customer segments
- {count} value propositions
- {count} channels
- {count} customer relationships
- {count} revenue streams
- {count} key resources
- {count} key activities
- {count} key partnerships
- {count} cost items

Next steps:
- `biz:lean` — Create Lean Canvas (startup focus)
- `biz:vpc` — Create Value Proposition Canvas (customer fit)
- `biz:swot` — Create SWOT Analysis (strategic positioning)
- `biz:list` — View all frameworks in this project
```
</step>

</process>

<output>
**Created:**
- `.planning/business/bmc/{id}.json` — BMC entity data
- `.planning/business/bmc/{id}.md` — Human-readable summary

**Updated:**
- `.planning/business/project.json` — Added entity reference
</output>

<success_criteria>
- `.planning/business/project.json` exists (prerequisite)
- BMC JSON file created with valid structure per lib/types.md
- BMC markdown summary created per lib/formatting.md
- Project.json updated with entity reference
- Entity ID follows timestamp format (YYYYMMDD-HHMMSS)
- All 9 BMC building blocks captured (even if some are empty arrays)
</success_criteria>
