# Type Definitions

Reference document defining data structures for all business frameworks. Serves as the canonical reference for command implementations.

## Common Fields

All entities share these base fields:

```json
{
  "id": "string",              // Timestamp-based ID (YYYYMMDD-HHMMSS)
  "projectId": "string",       // Parent project ID
  "type": "string",            // Framework type (bmc, lean, vpc, swot, persona, competitive, market)
  "name": "string",            // Human-readable entity name
  "description": "string?",    // Optional description
  "createdAt": "string",       // ISO 8601 timestamp
  "updatedAt": "string",       // ISO 8601 timestamp
  "linkedEntities": "array?"   // Optional entity relationships
}
```

## 1. Business Model Canvas (BMC)

The Business Model Canvas describes how an organization creates, delivers, and captures value through 9 building blocks.

### Fields

```json
{
  "customerSegments": [
    {
      "segment": "string",           // Segment name (required)
      "characteristics": "string?",  // Key characteristics
      "size": "string?"              // Market size estimate
    }
  ],

  "valuePropositions": [
    {
      "proposition": "string",       // Value proposition (required)
      "customerPains": "string?",    // Pains addressed
      "customerGains": "string?"     // Gains created
    }
  ],

  "channels": [
    {
      "channel": "string",           // Channel name (required)
      "phase": "string?"             // Awareness, Evaluation, Purchase, Delivery, After Sales
    }
  ],

  "customerRelationships": [
    {
      "relationship": "string",      // Relationship description (required)
      "type": "string?"              // Personal, Self-service, Automated, Communities, Co-creation
    }
  ],

  "revenueStreams": [
    {
      "stream": "string",            // Revenue stream (required)
      "type": "string?",             // Asset sale, Usage fee, Subscription, Lending, Licensing, Advertising
      "pricing": "string?"           // Pricing mechanism
    }
  ],

  "keyResources": [
    {
      "resource": "string",          // Resource name (required)
      "type": "string?"              // Physical, Intellectual, Human, Financial
    }
  ],

  "keyActivities": [
    {
      "activity": "string",          // Activity name (required)
      "type": "string?"              // Production, Problem Solving, Platform/Network
    }
  ],

  "keyPartnerships": [
    {
      "partner": "string",           // Partner name (required)
      "type": "string?",             // Strategic Alliance, Coopetition, Joint Venture, Supplier
      "motivation": "string?"        // Optimization, Risk reduction, Resource acquisition
    }
  ],

  "costStructure": [
    {
      "cost": "string",              // Cost item (required)
      "type": "string?"              // Fixed, Variable
    }
  ]
}
```

## 2. Lean Canvas

Startup-focused adaptation of the Business Model Canvas, emphasizing problem-solution fit and key metrics.

### Fields

```json
{
  "problem": [
    {
      "problem": "string",                  // Problem statement (required)
      "existingAlternatives": "string?"     // Current solutions customers use
    }
  ],

  "customerSegments": [
    {
      "segment": "string",           // Segment name (required)
      "earlyAdopters": "string?"     // Early adopter characteristics
    }
  ],

  "uniqueValueProposition": {
    "proposition": "string",         // Single clear message (required)
    "highLevelConcept": "string?"    // X for Y analogy (e.g., "Uber for X")
  },

  "solution": [
    {
      "feature": "string",           // Feature name (required)
      "description": "string?"       // How it solves the problem
    }
  ],

  "channels": ["string"],            // Array of channel names

  "revenueStreams": [
    {
      "stream": "string",            // Revenue source (required)
      "amount": "string?"            // Expected amount or range
    }
  ],

  "costStructure": [
    {
      "cost": "string",              // Cost item (required)
      "amount": "string?"            // Expected amount or range
    }
  ],

  "keyMetrics": [
    {
      "metric": "string",            // Metric name (required)
      "target": "string?"            // Target value
    }
  ],

  "unfairAdvantage": "string"        // Competitive moat that cannot be easily copied
}
```

## 3. Value Proposition Canvas (VPC)

Maps customer-value fit by analyzing customer profile against value map.

### Fields

```json
{
  "customerProfile": {
    "customerJobs": [
      {
        "job": "string",             // Job to be done (required)
        "type": "string?",           // Functional, Social, Emotional
        "importance": "string?"      // High, Medium, Low
      }
    ],

    "pains": [
      {
        "pain": "string",            // Customer pain (required)
        "severity": "string?"        // Extreme, Moderate, Mild
      }
    ],

    "gains": [
      {
        "gain": "string",            // Desired gain (required)
        "relevance": "string?"       // Required, Expected, Desired, Unexpected
      }
    ]
  },

  "valueMap": {
    "productsAndServices": [
      {
        "item": "string",            // Product or service (required)
        "importance": "string?"      // Essential, Nice to have, etc.
      }
    ],

    "painRelievers": [
      {
        "reliever": "string",        // How you relieve pain (required)
        "painsAddressed": ["string"] // Which pains from customerProfile
      }
    ],

    "gainCreators": [
      {
        "creator": "string",         // How you create gain (required)
        "gainsEnabled": ["string"]   // Which gains from customerProfile
      }
    ]
  },

  "fitScore": "number?"              // Optional 1-10 fit assessment
}
```

## 4. SWOT Analysis

Strategic positioning analysis examining internal strengths/weaknesses and external opportunities/threats.

### Fields

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

## 5. User Persona

Customer archetype capturing demographics, behaviors, goals, and frustrations.

### Fields

```json
{
  "demographics": {
    "age": "string?",                // Age or age range
    "gender": "string?",             // Gender identity
    "location": "string?",           // Geographic location
    "occupation": "string?",         // Job title or field
    "income": "string?",             // Income range
    "education": "string?",          // Education level
    "familyStatus": "string?"        // Single, Married, Children, etc.
  },

  "psychographics": {
    "personality": "string?",        // Personality traits
    "values": ["string"],            // Core values
    "interests": ["string"],         // Hobbies and interests
    "lifestyle": "string?"           // Lifestyle description
  },

  "behavior": {
    "goals": ["string"],             // Primary goals (required)
    "frustrations": ["string"],      // Pain points (required)
    "motivations": ["string"],       // What drives them (required)
    "preferredChannels": ["string"], // Communication preferences
    "buyingBehavior": "string?"      // Decision-making style
  },

  "quote": "string?",                // Representative quote
  "bio": "string?",                  // Brief biography
  "scenario": "string?"              // Typical use case scenario
}
```

## 6. Competitive Analysis

Market landscape analysis of competitors and positioning.

### Fields

```json
{
  "competitors": [
    {
      "name": "string",              // Competitor name (required)
      "website": "string?",          // Website URL
      "description": "string?",      // Brief description
      "strengths": ["string"],       // Key strengths (required)
      "weaknesses": ["string"],      // Key weaknesses (required)
      "pricing": "string?",          // Pricing model/range
      "marketShare": "string?",      // Market share estimate
      "targetAudience": "string?"    // Primary target market
    }
  ],

  "ourPosition": {
    "differentiators": ["string"],   // How we differ (required)
    "gaps": ["string"],              // Gaps in our offering (required)
    "opportunities": ["string"]      // Competitive opportunities (required)
  }
}
```

## 7. Market Sizing

Opportunity quantification with TAM, SAM, SOM methodology.

### Fields

```json
{
  "tam": {
    "value": "number",               // Total Addressable Market value (required)
    "currency": "string",            // Currency code (required)
    "unit": "string",                // Annual, Monthly, etc. (required)
    "methodology": "string?",        // How calculated
    "sources": ["string"],           // Data sources
    "assumptions": ["string"]        // Key assumptions
  },

  "sam": {
    "value": "number",               // Serviceable Addressable Market (required)
    "currency": "string",            // Currency code (required)
    "unit": "string",                // Annual, Monthly, etc. (required)
    "constraints": ["string"],       // What limits addressable market
    "targetSegments": ["string"]     // Which segments we can reach
  },

  "som": {
    "value": "number",               // Serviceable Obtainable Market (required)
    "currency": "string",            // Currency code (required)
    "unit": "string",                // Annual, Monthly, etc. (required)
    "timeframe": "string?",          // When we expect to capture
    "captureStrategy": "string?",    // How we'll capture
    "assumptions": ["string"]        // Key assumptions
  },

  "growthRate": {
    "rate": "number?",               // Growth rate percentage
    "period": "string?",             // Annual, 5-year CAGR, etc.
    "drivers": ["string"]            // Growth drivers
  }
}
```

## Type Constants

### Framework Types

```
bmc          Business Model Canvas
lean         Lean Canvas
vpc          Value Proposition Canvas
swot         SWOT Analysis
persona      User Persona
competitive  Competitive Analysis
market       Market Sizing
```

### Relationship Types

```
targets      Entity targets the linked entity
informs      Entity is informed by linked entity
complements  Complementary analyses
validates    Entity validates linked entity
```

## Validation Rules

### Required Fields

Each framework type has minimum required fields:

| Type | Required Fields |
|------|-----------------|
| BMC | At least one entry in customerSegments, valuePropositions |
| Lean | problem, customerSegments, uniqueValueProposition |
| VPC | customerProfile.customerJobs, valueMap.productsAndServices |
| SWOT | At least one entry in any quadrant |
| Persona | behavior.goals, behavior.frustrations, behavior.motivations |
| Competitive | At least one competitor with strengths and weaknesses |
| Market | tam.value, sam.value, som.value with currency and unit |

### Field Formats

- Timestamps: ISO 8601 (`2026-01-15T14:30:22Z`)
- Currency codes: ISO 4217 (`USD`, `EUR`, `GBP`)
- Percentages: Decimal (`0.15` for 15%) or string (`"15%"`)
