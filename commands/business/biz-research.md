---
name: biz:research
description: Research business topics using AI-powered WebSearch
allowed-tools: [Read, Write, Bash, Glob, WebSearch]
---

<objective>
Research business topics, competitors, and market trends using Claude's built-in WebSearch tool. Synthesize findings into actionable business insights.

Usage:
- `biz:research [topic]` - Conduct new research on a topic
- `biz:research --list` - List saved research files
- `biz:research --view [file]` - View saved research
</objective>

<context>
**Storage patterns:** See lib/storage.md
**Output formatting:** See lib/formatting.md
</context>

<process>

<step name="parse_mode">
Determine the operation mode from arguments:

**If argument is `--list`:**
Go to step `list_research`

**If argument is `--view {filename}`:**
Go to step `view_research`

**Otherwise:**
Treat argument as topic (or prompt for topic if empty)
Go to step `gather_topic`
</step>

<step name="list_research">
List all saved research files:

```bash
ls -1 .planning/business/research/*.md 2>/dev/null | sort -r
```

**If no files found:**
```
No saved research found.

Run `biz:research <topic>` to conduct new research.
```

**If files found:**
Display table of saved research:

```
Saved Research

| Date | Topic |
|------|-------|
| 2026-01-15 | ai-in-healthcare |
| 2026-01-14 | subscription-pricing |

View research: `biz:research --view {filename}`
New research: `biz:research <topic>`
```

**Filename parsing:**
- Format: `{YYYYMMDD-HHMMSS}-{slug}.md`
- Extract date: First 8 characters, format as YYYY-MM-DD
- Extract topic: Everything after timestamp, before .md, replace hyphens with spaces

Example: `20260115-143022-ai-in-healthcare.md` -> Date: 2026-01-15, Topic: ai in healthcare

Stop execution.
</step>

<step name="view_research">
View a saved research file:

**1. Construct file path:**
User may provide partial filename (just slug) or full filename:

```bash
# Try exact match first
ls .planning/business/research/{filename}.md 2>/dev/null

# If not found, try pattern match
ls .planning/business/research/*{filename}* 2>/dev/null | head -1
```

**If file not found:**
```
Research file not found: {filename}

Run `biz:research --list` to see available files.
```
Stop execution.

**If file found:**
Read and display the file content using the Read tool.

Display hint at end:
```
---
Edit: Open .planning/business/research/{filename}
Delete: rm .planning/business/research/{filename}
List all: `biz:research --list`
```

Stop execution.
</step>

<step name="gather_topic">
Get the research topic from user.

**If topic provided as argument:**
Continue with that topic.

**If no topic provided:**
```
Business Research

What topic would you like to research?
(e.g., "AI in healthcare", "subscription pricing models", "remote work trends")
```

Wait for user response.
</step>

<step name="select_focus">
Ask for research focus:

```
Research Focus

What aspect of "{topic}" should I focus on?

1. **market** - Market trends and industry analysis
2. **competitors** - Competitive landscape research
3. **customers** - Target audience and user behavior
4. **technology** - Technology trends and solutions
5. **general** - Open-ended business research
```

Wait for user selection.
</step>

<step name="construct_query">
Construct search query based on topic and focus:

**Focus-specific query patterns:**

- **market**: "{topic} market trends 2025 2026 industry analysis growth"
- **competitors**: "{topic} competitors market share companies comparison"
- **customers**: "{topic} customer behavior target audience demographics"
- **technology**: "{topic} technology solutions software tools innovation"
- **general**: "{topic} business analysis insights trends"

Enhance query with business context (e.g., "B2B", "enterprise", "startup" if relevant from topic).
</step>

<step name="execute_search">
Use WebSearch tool to gather information:

```
Researching: {topic}
Focus: {focus}

Searching for relevant business insights...
```

Execute WebSearch with constructed query.

Gather results from multiple searches if needed to build comprehensive picture.
</step>

<step name="synthesize_results">
Analyze search results and synthesize into business-relevant insights.

Structure findings as:

**Key Findings (3-5 bullet points):**
- Most important discoveries relevant to business decision-making
- Quantitative data points when available
- Recent developments and trends

**Market Insights (if focus is market or general):**
- Market size and growth projections
- Key trends shaping the industry
- Regulatory or economic factors

**Competitive Insights (if focus is competitors or general):**
- Key players and their positioning
- Competitive advantages observed
- Market gaps or opportunities

**Customer Insights (if focus is customers or general):**
- Target audience characteristics
- Pain points and needs
- Behavioral patterns

**Technology Insights (if focus is technology or general):**
- Relevant tools and solutions
- Innovation trends
- Implementation considerations

**Recommendations (2-3 action items):**
- Specific next steps based on findings
- Opportunities to explore
- Risks to consider

**Sources:**
- List all URLs used in research with titles
</step>

<step name="display_results">
Present research findings:

```markdown
# Business Research: {topic}

**Focus:** {focus}
**Date:** {current date}

---

## Key Findings

- {finding 1}
- {finding 2}
- {finding 3}

{Include relevant sections based on focus}

## Recommendations

1. {recommendation 1}
2. {recommendation 2}
3. {recommendation 3}

---

### Sources

- [{title}]({url})
- [{title}]({url})
```
</step>

<step name="offer_save">
Ask if user wants to save the research:

```
Save this research for future reference?

1. **Save** - Store in .planning/business/research/
2. **Skip** - Don't save, just keep in conversation
```

Wait for user response.

**If Save:**
Go to step `save_research`

**If Skip:**
Display completion message and stop.
</step>

<step name="save_research">
Save research to file:

**1. Create directory if needed:**
```bash
mkdir -p .planning/business/research
```

**2. Generate filename:**
- Timestamp: `date +"%Y%m%d-%H%M%S"`
- Slug: Convert topic to lowercase, replace spaces with hyphens, remove special characters
- Format: `{timestamp}-{slug}.md`

**3. Generate file content:**
```markdown
---
topic: {topic}
focus: {focus}
date: {ISO timestamp}
sources:
  - {url1}
  - {url2}
---

# Business Research: {topic}

**Focus:** {focus}
**Researched:** {date}

---

## Key Findings

{findings}

{relevant sections}

## Recommendations

{recommendations}

---

### Sources

{sources list}

---
*Research conducted: {date}*
```

**4. Write file using Write tool**

**5. Confirm save:**
```
Research saved.

File: .planning/business/research/{filename}

View later: `biz:research --view {filename-without-extension}`
List all: `biz:research --list`
```
</step>

</process>

<output>
**Display:** Research summary with findings, insights, and recommendations

**Optional save:**
- `.planning/business/research/{timestamp}-{slug}.md` - Research document with metadata
</output>

<success_criteria>
- WebSearch tool used to gather information
- Results synthesized into business-relevant insights
- Key findings clearly presented (3-5 bullet points)
- Recommendations are actionable (2-3 items)
- Sources always cited with URLs
- `biz:research <topic>` conducts new research
- `biz:research --list` lists saved research with date and topic
- `biz:research --view <file>` displays saved research content
- Research can be saved to .planning/business/research/{timestamp}-{slug}.md
</success_criteria>
