---
name: prompt-optimizer
description: Transform rough prompts into production-quality meta prompts through interactive gap analysis. Analyzes user's prompt against a completeness framework, identifies missing information, asks targeted clarifying questions, then generates an optimized prompt. Use when user wants to improve prompts, convert rough ideas into effective instructions, or says things like "optimize this prompt", "make this better", "help me write a prompt for X".
---

# Prompt Optimizer

Transform rough prompts into optimized meta prompts through **interactive gap analysis** - never assume, always ask.

## Workflow

```
User's rough prompt → Gap Analysis → Clarifying Questions → Gather Answers → Generate Optimized Prompt
```

**Do not skip clarifying questions.** Extract real requirements, don't guess.

## Step 1: Gap Analysis

Analyze against the **Prompt Completeness Framework**. Assess each: ✓ Present, ~ Partial, ✗ Missing.

### Prompt Completeness Framework

| Dimension | What to Look For | Why It Matters |
|-----------|------------------|----------------|
| **Task Clarity** | Specific verb + object ("summarize report" vs "look at this") | Exact action needed |
| **Context/Purpose** | Why this task? What's it for? Where does output go? | Shapes tone, depth, focus |
| **Input Specification** | What data/content? Format? Size? | How to structure prompt |
| **Output Format** | Structure, length, format (JSON, prose, list) | Prevents mismatched expectations |
| **Audience** | Who reads output? Expertise level? | Affects vocabulary, detail |
| **Constraints** | What to avoid? Must-haves? | Quality guardrails |
| **Success Criteria** | What makes output "good"? | Target to hit |
| **Edge Cases** | Unusual inputs? Error handling? | Production robustness |
| **Examples** | Good/bad output samples available? | Best way to show expectations |
| **Reusability** | One-time or repeated template? | Determines if variables needed |

### Internal Assessment Format

```
TASK CLARITY: [✓/~/✗] - [note]
CONTEXT: [✓/~/✗] - [note]
INPUT: [✓/~/✗] - [note]
OUTPUT FORMAT: [✓/~/✗] - [note]
AUDIENCE: [✓/~/✗] - [note]
CONSTRAINTS: [✓/~/✗] - [note]
SUCCESS CRITERIA: [✓/~/✗] - [note]
EDGE CASES: [✓/~/✗] - [note]
EXAMPLES: [✓/~/✗] - [note]
REUSABILITY: [✓/~/✗] - [note]

CRITICAL GAPS: [✗ items - must fill]
NICE-TO-HAVE: [~ items - would improve]
```

## Step 2: Ask Clarifying Questions

### Question Guidelines

1. **Prioritize**: Critical gaps (✗) first
2. **Batch**: 3-5 questions max per round
3. **Be specific**: Not "tell me more" → "what format should output be?"
4. **Offer options**: Speed up answers with choices
5. **Explain why**: Brief context helps users answer better

### Question Bank by Gap Type

**Task Clarity:**
- "What specific action should Claude take? (summarize, analyze, extract, rewrite, compare, etc.)"
- "What exactly should happen with [X]?"

**Context/Purpose:**
- "What will you use this output for?"
- "Where does this fit in your workflow?"

**Input:**
- "What content will you provide? (documents, data, conversation)"
- "Typical size/length of input?"
- "What format is input in?"

**Output Format:**
- "What format needed? (prose, bullets, JSON, table)"
- "Length requirements? (word count, number of items)"
- "Should it have headers/sections?"

**Audience:**
- "Who reads this? (executives, technical team, customers)"
- "Their expertise level on this topic?"

**Constraints:**
- "Anything to specifically avoid?"
- "Must-have elements?"
- "Style/tone requirements?"

**Success Criteria:**
- "How will you judge if output is good?"
- "What would make you say 'exactly what I needed'?"

**Examples:**
- "Have an example of good output for similar tasks?"
- "Can you show what you DON'T want?"

**Reusability:**
- "One-time task or repeated with different inputs?"
- "What parts change between uses?"

### Example Question Round

```markdown
I've analyzed your prompt. A few questions to make it effective:

1. **Output format**: Structured (bullets, table) or prose? Length target?

2. **Audience**: Who reads this - technical team, executives, customers?

3. **Success criteria**: What would make you say "this is exactly right"?

4. **Examples** (optional): Have a sample of good output from similar task?
```

## Step 3: Gather and Confirm

After answers:
1. **Summarize understanding** in 2-3 sentences
2. **Identify remaining gaps** - follow up if critical info missing
3. **Confirm**: "Does this capture what you need? Ready to generate?"

## Step 4: Generate Optimized Prompt

### Prompt Assembly Order

1. Role/persona (if domain expertise helps)
2. Context and purpose
3. Input placeholder with XML tags
4. Step-by-step instructions
5. Output format specification
6. Constraints and guardrails
7. Examples (if provided/needed)
8. Variables for templates

### Technique Selection

| Situation | Apply |
|-----------|-------|
| Complex reasoning | Chain-of-thought structure |
| Specific format required | Output format + example |
| Domain expertise helps | Role assignment |
| Repeated use | Template variables `{{var}}` |
| Multiple input types | XML tags |
| Classification task | 2-3 multishot examples |

### XML Structure Template

```xml
<role>
[If applicable: expertise, style, behaviors]
</role>

<context>
[Purpose, background, workflow position]
</context>

<input>
{{input_variable}}
</input>

<instructions>
[Numbered steps]
</instructions>

<output_format>
[Exact structure expected]
</output_format>

<constraints>
[What to avoid, must-haves]
</constraints>

<examples>
[If applicable: input/output pairs]
</examples>
```

## Step 5: Deliver and Iterate

```markdown
## Optimized Prompt

[Complete prompt in code block]

## What This Prompt Does
[2-3 sentence summary]

## Variables to Fill
- `{{variable}}`: [what to substitute]

## Want to Refine?
[Offer adjustments]
```

**Always offer iteration**: "Want me to adjust tone, add examples, or modify any section?"

## Minimum Viable Prompt

Even simple prompts need:
- ✓ Clear task verb
- ✓ Output format  
- ✓ One constraint or quality indicator

## References

For detailed examples, see `references/examples.md`
For XML tag patterns, see `references/xml-patterns.md`
