# Prompting Patterns Reference

Reference document defining how business framework commands should gather information from users.

## General Principles

### Start Open-Ended

Begin with broad questions that let users describe their business in their own words:
- "Tell me about your business/product/idea"
- "What are you working on?"

### Follow the Thread

Probe what they mentioned rather than following a rigid script. If they mention "customers," ask about those customers. If they mention "revenue," explore that direction.

### Use AskUserQuestion for Structured Choices

When presenting options or gathering categorical information:
- Provide clear, distinct options
- Include an escape hatch: "Something else" or "Let me explain"
- Don't overwhelm: 3-5 options is ideal

### Know When to Stop

Use the decision gate pattern when enough information has been gathered:
- "Ready to create [framework], or want to explore more?"
- Options: Create now, Ask more questions, Let me add context

---

## Framework-Specific Prompts

### Business Model Canvas

**Entry point:** "What's the core value you deliver to customers?"

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

**Probing questions:**
- For segments: "What size is this segment?" "What characterizes them?"
- For channels: "What phase — awareness, evaluation, purchase, delivery, after-sales?"
- For partnerships: "What's the motivation — optimization, economy of scale, risk reduction?"

---

### Lean Canvas

**Entry point:** "What problem are you solving?"

**Follow-up sequence:** Focus on validation mindset:

1. **Problem** — "Who has this problem? What are they doing about it today?"
2. **Customer Segments** — "Who has this problem most acutely?" (early adopters)
3. **Unique Value Proposition** — "In one sentence, why would someone switch to you?"
4. **Solution** — "What features directly address the problem?"
5. **Channels** — "How will you reach early adopters?"
6. **Revenue Streams** — "How will you make money? What will people pay?"
7. **Cost Structure** — "What are your fixed and variable costs?"
8. **Key Metrics** — "What numbers will tell you you're succeeding?"
9. **Unfair Advantage** — "What makes you hard to copy?"

**Probing questions:**
- For problem: "How painful is this? What do they sacrifice by not solving it?"
- For UVP: "What's the high-level concept? Twitter for X, Uber for Y?"
- For unfair advantage: "Is this truly defensible? Could a competitor acquire it?"

---

### Value Proposition Canvas

**Entry point:** "What job is your customer trying to get done?"

**Follow-up sequence:** Map customer first, then solution:

**Customer Profile:**
1. **Customer Jobs** — "What tasks are they trying to complete? Functional? Social? Emotional?"
2. **Pains** — "What frustrates them? What risks do they face? What's going wrong?"
3. **Gains** — "What would delight them? What outcomes do they want? What would make their life easier?"

**Value Map:**
4. **Products & Services** — "What are you offering?"
5. **Pain Relievers** — "How does your offering eliminate or reduce pains?"
6. **Gain Creators** — "How does your offering create the gains they want?"

**Probing questions:**
- For jobs: "Which jobs are most important? Most frequent? Most underserved?"
- For pains: "How severe is this? Essential or nice-to-have to address?"
- For fit: "How well do your pain relievers match their actual pains?"

---

### SWOT Analysis

**Entry point:** "What's the strategic decision you're analyzing?"

**Context first:** Get the context before diving into quadrants:
- "What decision or direction are you evaluating?"
- "What timeframe are we considering?"

**Follow-up sequence:** Internal then external:

1. **Strengths** — "What internal capabilities give you an advantage?"
2. **Weaknesses** — "What internal limitations hold you back? What do competitors do better?"
3. **Opportunities** — "What external trends or changes could you benefit from?"
4. **Threats** — "What external factors could harm your position?"

**Probing questions:**
- For each item: "What's the impact — high, medium, low?"
- For weaknesses: "What could mitigate this?"
- For opportunities: "What action would be required?"
- For threats: "What's the likelihood? What's your contingency?"

---

### User Persona

**Entry point:** "Describe a typical user's day"

**Follow-up sequence:** Behavior first, then demographics:

1. **Behavior** — "What are their goals? What frustrates them? What motivates them?"
2. **Demographics** — "Age range? Occupation? Location? Income level?"
3. **Psychographics** — "What do they value? What are their interests? How would you describe their personality?"
4. **Quote** — "What's one sentence they might say that captures who they are?"

**Probing questions:**
- "What channels do they prefer — email, social, phone?"
- "How do they make buying decisions — price-driven, quality-driven, research-heavy?"
- "What's a scenario where they'd encounter your product?"

---

### Competitive Analysis

**Entry point:** "Who are your main competitors?"

**For each competitor:**
1. **Basics** — "What's their name? Website?"
2. **Strengths** — "What do they do well?"
3. **Weaknesses** — "Where do they fall short?"
4. **Positioning** — "Who do they target? How do they price?"

**Then for your position:**
5. **Differentiators** — "How are you different?"
6. **Gaps** — "What do competitors have that you don't?"
7. **Opportunities** — "Where can you win?"

**Probing questions:**
- "Do you have market share estimates?"
- "What's their pricing model — subscription, one-time, freemium?"
- "Who's their ideal customer compared to yours?"

---

### Market Sizing

**Entry point:** "What's the total market for what you're offering?"

**Follow-up sequence:** TAM → SAM → SOM:

1. **TAM (Total Addressable Market)** — "If you had 100% of the market, what would that be worth?"
2. **SAM (Serviceable Addressable Market)** — "What portion can you realistically address with your current offering?"
3. **SOM (Serviceable Obtainable Market)** — "What can you capture in the next [timeframe]?"

**Probing questions:**
- "What methodology — top-down (industry reports) or bottom-up (customer count × price)?"
- "What are your sources?"
- "What assumptions are you making?"
- "What's the market growth rate? What's driving growth?"

---

## Decision Gate Pattern

After gathering sufficient information, present the decision gate:

```
Ready to create your [Framework Name]?

Options:
1. **Create now** — I have enough to build a solid [framework]
2. **Ask more questions** — Probe deeper on [specific area]
3. **Let me add context** — I want to share more details first
```

This gives users control over depth while providing a clear off-ramp to artifact creation.

---

## Escape Hatches

Always provide ways for users to:
- Redirect the conversation: "Actually, let me tell you about..."
- Skip sections: "I don't have that information yet"
- Add nuance: "It's more complicated than that"
- Go back: "Can we revisit [previous topic]?"

The goal is conversation, not interrogation.
