---
name: biz:vpc
description: Create a Value Proposition Canvas
allowed-tools: [Read, Write, Bash, Glob, AskUserQuestion]
---

<objective>
Create a Value Proposition Canvas through conversational prompting, storing data as JSON with markdown summary output.

The Value Proposition Canvas maps customer-value fit by analyzing the customer profile (jobs, pains, gains) against the value map (products/services, pain relievers, gain creators).
</objective>

<context>
**Storage location:** `.planning/business/vpc/`
**Reference documents:**
- `lib/types.md` — VPC field structure (customerProfile, valueMap, fitScore)
- `lib/prompts.md` — VPC prompting patterns (Customer Profile first, then Value Map)
- `lib/formatting.md` — VPC markdown output format
- `lib/storage.md` — File naming and storage conventions
</context>

<process>

<step name="check_project">
Check if a business design project exists:

```bash
ls .planning/business/project.json 2>/dev/null
```

**If file does not exist:**
```
No business design project found.

Run `biz:new-project` first to initialize your project.
```

Stop execution.

**If file exists:**
Read the project to get projectId:
```bash
cat .planning/business/project.json
```

Continue to gather_info step.
</step>

<step name="gather_info">
Gather Value Proposition Canvas information through conversation.

**Entry point:**
```
Let's create a Value Proposition Canvas.

What job is your customer trying to get done?
```

**Follow-up sequence — Customer Profile first, then Value Map:**

**Customer Profile:**

1. **Customer Jobs** — "What tasks are they trying to complete? Consider functional jobs (tasks), social jobs (how they want to be perceived), and emotional jobs (how they want to feel)."

2. **Pains** — "What frustrates them when trying to get these jobs done? What risks do they face? What's going wrong or could go wrong?"

3. **Gains** — "What would delight them? What outcomes do they want? What would make their life easier or better?"

**Value Map:**

4. **Products & Services** — "What are you offering to help them? List your products, services, or features."

5. **Pain Relievers** — "How does your offering eliminate or reduce their pains? Which specific pains does each feature address?"

6. **Gain Creators** — "How does your offering create the gains they want? Which specific gains does each feature enable?"

**Probing questions:**
- For jobs: "Which jobs are most important? Most frequent? Most underserved by current solutions?"
- For pains: "How severe is this pain — extreme, moderate, or mild?"
- For gains: "Is this gain required, expected, desired, or would be an unexpected delight?"
- For fit: "How well do your pain relievers match their actual pains?"

**Decision gate:**
After gathering sufficient information:

```
Ready to create your Value Proposition Canvas?

Options:
1. **Create now** — I have enough to build a solid VPC
2. **Ask more questions** — Probe deeper on [customer profile / value map]
3. **Let me add context** — I want to share more details first
```

Wait for user to select option 1 before proceeding.
</step>

<step name="create_entity">
Generate entity files.

**1. Generate ID:**
```bash
date +"%Y%m%d-%H%M%S"
```

**2. Create directory if needed:**
```bash
mkdir -p .planning/business/vpc
```

**3. Generate timestamp:**
```bash
date -u +"%Y-%m-%dT%H:%M:%SZ"
```

**4. Write JSON file** (`.planning/business/vpc/{id}.json`):

```json
{
  "id": "[generated-id]",
  "projectId": "[from project.json]",
  "type": "vpc",
  "name": "[user-provided or generated name]",
  "description": "[optional description]",
  "createdAt": "[ISO timestamp]",
  "updatedAt": "[ISO timestamp]",
  "linkedEntities": [],

  "customerProfile": {
    "customerJobs": [
      {
        "job": "[job description]",
        "type": "[Functional | Social | Emotional]",
        "importance": "[High | Medium | Low]"
      }
    ],

    "pains": [
      {
        "pain": "[pain description]",
        "severity": "[Extreme | Moderate | Mild]"
      }
    ],

    "gains": [
      {
        "gain": "[gain description]",
        "relevance": "[Required | Expected | Desired | Unexpected]"
      }
    ]
  },

  "valueMap": {
    "productsAndServices": [
      {
        "item": "[product or service]",
        "importance": "[Essential | Nice to have]"
      }
    ],

    "painRelievers": [
      {
        "reliever": "[how you relieve pain]",
        "painsAddressed": ["[pain from customerProfile]"]
      }
    ],

    "gainCreators": [
      {
        "creator": "[how you create gain]",
        "gainsEnabled": ["[gain from customerProfile]"]
      }
    ]
  },

  "fitScore": null
}
```

**5. Write markdown file** (`.planning/business/vpc/{id}.md`):

```markdown
# Value Proposition Canvas: {name}

{description}

---

## Customer Profile

### Customer Jobs
- **{job}** ({type}) - {importance}

### Pains
- {pain} (severity: {severity})

### Gains
- {gain} ({relevance})

## Value Map

### Products & Services
- {item} ({importance})

### Pain Relievers
- **{reliever}** -> addresses: {addressed pain}

### Gain Creators
- **{creator}** -> creates: {addressed gain}

## Fit Score
**{score}/10** (if assessed)

---
*Created: {date}*
```

**Formatting rules:**
- Omit optional fields with no value
- Bold primary field names in lists
- Use parentheses for categorical data
- Link pain relievers to specific pains
- Link gain creators to specific gains
</step>

<step name="update_project">
Add entity reference to project.json:

**1. Read current project.json**

**2. Add to entities array:**
```json
{
  "id": "[generated-id]",
  "type": "vpc",
  "name": "[entity name]",
  "createdAt": "[ISO timestamp]"
}
```

**3. Update project.json updatedAt timestamp**

**4. Write updated project.json**
</step>

<step name="confirm">
Display confirmation:

```
Value Proposition Canvas created.

**{name}**
ID: {id}
Location: .planning/business/vpc/

Files created:
- .planning/business/vpc/{id}.json
- .planning/business/vpc/{id}.md

Customer Profile:
- {X} customer jobs
- {Y} pains identified
- {Z} gains desired

Value Map:
- {A} products/services
- {B} pain relievers
- {C} gain creators

---

Next steps:
- `biz:view {id}` — View full canvas details
- `biz:persona` — Create a persona for this customer segment
- `biz:bmc` — Create a Business Model Canvas that incorporates this value proposition
- `biz:link {id} <other-id>` — Link to related frameworks
```
</step>

</process>

<output>
**Created:**
- `.planning/business/vpc/{id}.json` — Structured VPC data
- `.planning/business/vpc/{id}.md` — Human-readable summary

**Updated:**
- `.planning/business/project.json` — Entity reference added
</output>

<success_criteria>
- `.planning/business/vpc/{id}.json` contains valid JSON with all required fields
- `.planning/business/vpc/{id}.md` is readable and follows formatting conventions
- `project.json` entities array includes new VPC reference
- User received confirmation with entity details and next steps
- Customer Profile has at least one customer job
- Value Map has at least one product/service
</success_criteria>
