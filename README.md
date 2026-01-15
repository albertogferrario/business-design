# Business Design

Markdown-based slash commands for business framework design and analysis. Use with Claude Code as `/dev` commands.

## Overview

This package provides structured prompts for creating business frameworks directly within your development workflow. All frameworks are stored locally in `.planning/business/` alongside your project code.

**Supported Frameworks:**
- Business Model Canvas (9 building blocks)
- Lean Canvas (startup validation)
- Value Proposition Canvas (customer-value fit)
- SWOT Analysis (strategic positioning)
- User Personas (customer archetypes)
- Competitive Analysis (market landscape)
- Market Sizing (TAM/SAM/SOM)

## Installation

```bash
npm install @albertogferrario/business-design
```

After installation, the commands are available as `/dev` commands in Claude Code.

## Quick Start

1. **Initialize a project:**
   ```
   /dev biz:new-project
   ```

2. **Create a framework:**
   ```
   /dev biz:bmc
   ```
   The command guides you through a conversational flow to capture your business model.

3. **View your frameworks:**
   ```
   /dev biz:list
   ```

4. **Link related frameworks:**
   ```
   /dev biz:link persona-20260115-143022 bmc-20260115-140512
   ```

## Commands Reference

### Project Commands

| Command | Description |
|---------|-------------|
| `biz:new-project` | Initialize a business design project in `.planning/business/` |
| `biz:list` | List all frameworks in the current project |
| `biz:view <id>` | View a specific framework by ID |
| `biz:link <source> <target>` | Create relationship between frameworks |

### Canvas Frameworks

| Command | Description |
|---------|-------------|
| `biz:bmc` | Create a Business Model Canvas (9 building blocks) |
| `biz:lean` | Create a Lean Canvas (startup-focused) |
| `biz:vpc` | Create a Value Proposition Canvas (customer-value fit) |
| `biz:swot` | Create a SWOT Analysis (strategic positioning) |

### Analysis Frameworks

| Command | Description |
|---------|-------------|
| `biz:persona` | Create a User Persona (customer archetype) |
| `biz:competitive` | Create a Competitive Analysis (market landscape) |
| `biz:market-sizing` | Create a Market Sizing estimate (TAM/SAM/SOM) |

### Research

| Command | Description |
|---------|-------------|
| `biz:research <topic>` | Research a business topic using WebSearch |

## Framework Details

### Business Model Canvas

The standard 9-block framework for describing how an organization creates, delivers, and captures value:

- **Customer Segments** — Who you serve
- **Value Propositions** — What value you deliver
- **Channels** — How you reach customers
- **Customer Relationships** — How you interact
- **Revenue Streams** — How you earn money
- **Key Resources** — What you need to deliver
- **Key Activities** — What you must do well
- **Key Partnerships** — Who helps you
- **Cost Structure** — What it costs to operate

### Lean Canvas

Startup-focused adaptation emphasizing problem-solution fit:

- Problem statements with existing alternatives
- Customer segments with early adopter characteristics
- Unique value proposition with high-level concept
- Solution features
- Channels, revenue streams, cost structure
- Key metrics and unfair advantage

### Value Proposition Canvas

Maps customer profile against value map:

**Customer Profile:**
- Customer jobs (functional, social, emotional)
- Pains with severity levels
- Gains with relevance ratings

**Value Map:**
- Products and services
- Pain relievers mapped to customer pains
- Gain creators mapped to customer gains

### SWOT Analysis

Strategic positioning through four quadrants:

- **Strengths** — Internal advantages with impact ratings
- **Weaknesses** — Internal limitations with mitigation strategies
- **Opportunities** — External possibilities with required actions
- **Threats** — External risks with contingency plans

### User Personas

Customer archetypes capturing:

- Demographics (age, location, occupation, income)
- Psychographics (values, interests, lifestyle)
- Behavior (goals, frustrations, motivations, channels)
- Representative quote and scenario

### Competitive Analysis

Market landscape documentation:

- Competitor profiles (strengths, weaknesses, pricing, target audience)
- Your positioning (differentiators, gaps, opportunities)

### Market Sizing

TAM/SAM/SOM methodology:

- **TAM** — Total Addressable Market with methodology and sources
- **SAM** — Serviceable Addressable Market with constraints
- **SOM** — Serviceable Obtainable Market with capture strategy
- Growth rate drivers

## Storage

All frameworks are stored in `.planning/business/` within your project:

```
.planning/business/
  project.json           # Project metadata
  bmc/
    bmc-20260115-143022.json    # Framework data
    bmc-20260115-143022.md      # Human-readable summary
  persona/
    persona-20260115-144500.json
    persona-20260115-144500.md
  ...
```

**File formats:**
- JSON files contain structured data for programmatic access
- Markdown files provide human-readable summaries

**ID format:** `{type}-{YYYYMMDD}-{HHMMSS}` for natural chronological ordering

## Usage with Claude Code

These commands are designed as `/dev` commands for the builder app. After installation:

1. Commands appear in your project's command palette
2. Run commands with `/dev biz:command-name`
3. Claude guides you through conversational information gathering
4. Frameworks are saved automatically to `.planning/business/`

**Relationship types for linking:**
- `targets` — Entity targets the linked entity
- `informs` — Entity is informed by linked entity
- `complements` — Complementary analyses
- `validates` — Entity validates linked entity

## Type Definitions

For complete field structures of all frameworks, see `lib/types.md` in the package. Key types:

| Type | Required Fields |
|------|-----------------|
| BMC | customerSegments, valuePropositions |
| Lean | problem, customerSegments, uniqueValueProposition |
| VPC | customerProfile.customerJobs, valueMap.productsAndServices |
| SWOT | At least one entry in any quadrant |
| Persona | behavior.goals, behavior.frustrations, behavior.motivations |
| Competitive | At least one competitor with strengths and weaknesses |
| Market | tam.value, sam.value, som.value with currency and unit |

## License

MIT
