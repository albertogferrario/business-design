# Formatting Utilities Reference

Reference document defining how to format business framework outputs as markdown.

## General Structure

### Document Layout

```markdown
# {Framework Type}: {Name}

{Description if present}

**Tags:** {tag1, tag2, tag3}

---

## {Section 1}
{content}

## {Section 2}
{content}

---

### Linked Entities
- **{Type}**: {name} ({relationship})

---
*Created: {date}*
```

### Hierarchy Rules

- H1: Framework type and entity name
- H2: Major sections (canvas blocks, analysis categories)
- H3: Subsections (when needed for grouping)
- H4: Used sparingly, only for deep nesting like VPC's Customer Profile/Value Map

---

## List Formatting

### Primary Format

Bold the main field, follow with secondary info, parenthesize categorical data:

```markdown
- **{main field}** - {secondary info} ({categorical data})
```

### Examples

```markdown
- **Enterprise customers** - Large organizations with 500+ employees (B2B)
- **Save time on invoicing** - Reduces invoice creation from 30 minutes to 2 minutes (high impact)
- **Stripe** - Payment processing partner: enables revenue collection
```

### Nested Details

Use indented sub-bullets for additional context:

```markdown
- **{main item}**
  - {detail 1}
  - {detail 2}
```

---

## Framework-Specific Formats

### Business Model Canvas

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

---

### Lean Canvas

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

---

### Value Proposition Canvas

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
- {item} ({type})

### Pain Relievers
- **{reliever}** -> addresses: {addressed pain}

### Gain Creators
- **{creator}** -> creates: {addressed gain}

## Fit Score
**{score}%**

---
*Created: {date}*
```

---

### SWOT Analysis

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

---

### User Persona

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

---

### Competitive Analysis

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

---

### Market Sizing

```markdown
# Market Sizing: {name}

{description}

---

## TAM (Total Addressable Market)
**{currency} {value}** {unit}

Methodology: {methodology}

Sources: {source1}, {source2}

Assumptions:
- {assumption1}
- {assumption2}

## SAM (Serviceable Addressable Market)
**{currency} {value}** {unit}

Constraints: {constraint1}, {constraint2}

Target Segments: {segment1}, {segment2}

## SOM (Serviceable Obtainable Market)
**{currency} {value}** {unit}

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

---

## Linked Entities Section

When an entity has links to other entities:

```markdown
### Linked Entities
- **Business Model Canvas**: Acme Corp Canvas (informs)
- **User Persona**: Tech-Savvy Startup Founder (targets)
- **SWOT Analysis**: Market Entry SWOT (analyzes)
```

Format: `**{Entity Type}**: {entity name} ({relationship})`

---

## Currency Formatting

For market sizing and financial data:

```
$1.2B   (billions)
$450M   (millions)
$25K    (thousands)
$1,234  (raw numbers under 10K)
```

Always include currency symbol and use standard abbreviations.

---

## Date Formatting

Use ISO-style dates for consistency:

```markdown
*Created: 2026-01-15*
```

---

## Empty/Optional Fields

Omit fields that have no value rather than showing empty placeholders:

```markdown
# Good
## Demographics
- **Age:** 25-34
- **Occupation:** Software Engineer

# Bad
## Demographics
- **Age:** 25-34
- **Gender:**
- **Location:**
- **Occupation:** Software Engineer
```
