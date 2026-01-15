---
name: biz:persona
description: Create a User Persona
allowed-tools: [Read, Write, Bash, Glob, AskUserQuestion]
---

<objective>
Create a User Persona through guided conversation, storing the data as JSON with a markdown summary for human readability.

User Personas are customer archetypes capturing demographics, behaviors, goals, and frustrations. They help teams develop empathy for customers and make user-centered decisions.
</objective>

<context>
**Type definitions:** See lib/types.md section "5. User Persona"
**Prompting patterns:** See lib/prompts.md section "User Persona"
**Output formatting:** See lib/formatting.md section "User Persona"
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
Gather persona information through conversation.

**Entry point:**
```
Create User Persona

Describe a typical user's day. What do they do? What challenges do they face?
```

Wait for user response.

**Follow-up sequence:** Behavior first, then demographics:

1. **Behavior** — "What are their main goals? What frustrates them most? What motivates them?"
2. **Demographics** — "What's their age range? Occupation? Location? Income level?"
3. **Psychographics** — "What do they value? What are their interests? How would you describe their personality?"
4. **Quote** — "What's one sentence they might say that captures who they are?"

**Probing questions (use as needed):**
- For channels: "What channels do they prefer — email, social, phone?"
- For decisions: "How do they make buying decisions — price-driven, quality-driven, research-heavy?"
- For scenarios: "What's a scenario where they'd encounter your product?"
- For lifestyle: "How would you describe their lifestyle?"

**Decision gate:**
After gathering sufficient information, present:

```
Ready to create your User Persona?

Options:
1. **Create now** — I have enough to build a solid persona
2. **Ask more questions** — Probe deeper on specific areas
3. **Let me add context** — I want to share more details first
```

Wait for user decision before proceeding.
</step>

<step name="create_entity">
Create the persona entity files.

**1. Generate entity ID:**
```bash
date +"%Y%m%d-%H%M%S"
```

**2. Create directory if needed:**
```bash
mkdir -p .planning/business/persona
```

**3. Generate timestamp:**
```bash
date -u +"%Y-%m-%dT%H:%M:%SZ"
```

**4. Write JSON file** (`.planning/business/persona/{id}.json`):

Structure per lib/types.md:
```json
{
  "id": "{generated-id}",
  "projectId": "{from project.json}",
  "type": "persona",
  "name": "{user-provided persona name}",
  "description": "{optional description}",
  "createdAt": "{ISO timestamp}",
  "updatedAt": "{ISO timestamp}",
  "linkedEntities": [],

  "demographics": {
    "age": "{age or age range}",
    "gender": "{gender identity}",
    "location": "{geographic location}",
    "occupation": "{job title or field}",
    "income": "{income range}",
    "education": "{education level}",
    "familyStatus": "{Single|Married|Children|etc.}"
  },

  "psychographics": {
    "personality": "{personality traits}",
    "values": ["{value1}", "{value2}"],
    "interests": ["{interest1}", "{interest2}"],
    "lifestyle": "{lifestyle description}"
  },

  "behavior": {
    "goals": ["{goal1}", "{goal2}"],
    "frustrations": ["{frustration1}", "{frustration2}"],
    "motivations": ["{motivation1}", "{motivation2}"],
    "preferredChannels": ["{channel1}", "{channel2}"],
    "buyingBehavior": "{decision-making style}"
  },

  "quote": "{representative quote}",
  "bio": "{brief biography}",
  "scenario": "{typical use case scenario}"
}
```

**5. Write markdown summary** (`.planning/business/persona/{id}.md`):

Format per lib/formatting.md:
```markdown
# User Persona: {name}

> "{quote}"

{bio}

---

## Demographics
- **Age:** {age}
- **Gender:** {gender}
- **Location:** {location}
- **Occupation:** {occupation}
- **Income:** {income}
- **Education:** {education}
- **Family Status:** {family status}

## Psychographics
- **Personality:** {trait1}, {trait2}
- **Values:** {value1}, {value2}
- **Interests:** {interest1}, {interest2}
- **Lifestyle:** {lifestyle}

## Behavior

### Goals
- {goal}

### Frustrations
- {frustration}

### Motivations
- {motivation}

**Preferred Channels:** {channel1}, {channel2}
**Buying Behavior:** {buying behavior}

## Scenario
{scenario text}

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
  "type": "persona",
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
User Persona created.

Name: {name}
ID: {id}
Location: .planning/business/persona/

Files:
- {id}.json — Structured data
- {id}.md — Human-readable summary

Your persona includes:
- Demographics: {present/absent}
- Psychographics: {present/absent}
- Goals: {count} defined
- Frustrations: {count} defined
- Motivations: {count} defined
- Quote: {present/absent}
- Scenario: {present/absent}

Next steps:
- `biz:bmc` — Create Business Model Canvas
- `biz:vpc` — Create Value Proposition Canvas (customer fit)
- `biz:competitive` — Create Competitive Analysis
- `biz:list` — View all frameworks in this project
```
</step>

</process>

<output>
**Created:**
- `.planning/business/persona/{id}.json` — Persona entity data
- `.planning/business/persona/{id}.md` — Human-readable summary

**Updated:**
- `.planning/business/project.json` — Added entity reference
</output>

<success_criteria>
- `.planning/business/project.json` exists (prerequisite)
- Persona JSON file created with valid structure per lib/types.md
- Persona markdown summary created per lib/formatting.md
- Project.json updated with entity reference
- Entity ID follows timestamp format (YYYYMMDD-HHMMSS)
- Required fields present: behavior.goals, behavior.frustrations, behavior.motivations
</success_criteria>
