# Interactive Prompt Optimization Examples

Full examples showing the gap analysis → questions → optimized prompt workflow.

---

## Example 1: Vague Analysis Request

### User's Input
```
look at this data and tell me what's interesting
```

### Gap Analysis
```
TASK CLARITY: ~ - "look at" is vague, "interesting" undefined
CONTEXT: ✗ - No purpose, don't know what data is for
INPUT: ✗ - No idea what kind of data
OUTPUT FORMAT: ✗ - No structure specified
AUDIENCE: ✗ - Unknown who will use this
CONSTRAINTS: ✗ - No guidance on focus areas
SUCCESS CRITERIA: ✗ - "Interesting" is subjective
EXAMPLES: ✗ - None provided
REUSABILITY: ✗ - Unknown

CRITICAL GAPS: Context, Input, Output Format, Audience, Success Criteria
```

### Clarifying Questions Asked
```markdown
I can help you build a prompt for data analysis. A few questions:

1. **What kind of data is this?** (sales figures, survey responses, logs, etc.)

2. **What will you use this analysis for?** (report to leadership, identify problems, make a decision)

3. **Who's the audience?** (technical analysts, executives, yourself)

4. **What format works best?** 
   - Executive summary (key insights + recommendations)
   - Detailed breakdown (metrics, trends, anomalies)
   - Quick bullet points

5. **Any specific things you're hoping to find or areas to focus on?**
```

### User's Answers
```
1. It's monthly sales data by region and product category
2. Quarterly business review presentation for the leadership team
3. C-suite executives, not technical
4. Executive summary with key insights
5. Looking for growth opportunities and underperforming areas
```

### Confirmation
"Got it - you need an executive summary of monthly sales data (by region/product) for a QBR presentation to C-suite. Focus on growth opportunities and underperformers, keep it non-technical. Ready to generate?"

### Optimized Prompt
```xml
<role>
You are a senior data analyst specializing in identifying actionable insights from business data.
</role>

<context>
I'm preparing a quarterly business review for stakeholders. I need to identify the most significant patterns and anomalies in our data.
</context>

<data>
{{data}}
</data>

<instructions>
Analyze the provided data following these steps:

1. Data Overview: Summarize the dataset structure (rows, columns, data types, time range)

2. Key Metrics: Identify and calculate the most important metrics:
   - Totals and averages for numeric fields
   - Growth rates period-over-period
   - Distribution patterns

3. Notable Findings: Highlight the top 3-5 insights, prioritized by business impact:
   - Significant trends (growth/decline >10%)
   - Anomalies or outliers
   - Correlations between variables

4. Recommendations: For each finding, suggest one specific action

Structure your analysis with clear headers. Support each insight with specific numbers from the data.
</instructions>

<output_format>
## Data Overview
[Brief summary]

## Key Metrics
[Metric table or list]

## Top Insights
### 1. [Insight title]
- Finding: [What you observed]
- Evidence: [Specific numbers]
- Recommendation: [Suggested action]

[Repeat for each insight]

## Summary
[2-3 sentence executive summary]
</output_format>
```

---

## Example 2: Simple Email Request

### User's Input
```
write me an email about the meeting
```

### Gap Analysis
```
TASK CLARITY: ~ - "email about meeting" but what kind?
CONTEXT: ✗ - What meeting? Past or upcoming?
INPUT: ✗ - No meeting details provided
OUTPUT FORMAT: ~ - Email implied but no specifics
AUDIENCE: ✗ - Who receives it?
CONSTRAINTS: ✗ - Tone? Length?
SUCCESS CRITERIA: ✗ - Unknown

CRITICAL GAPS: Context (what meeting), Input (details), Audience
```

### Clarifying Questions Asked
```markdown
Happy to help with that email. Quick questions:

1. **What type of email?**
   - Meeting invite/scheduling
   - Follow-up after a meeting
   - Recap/summary to share
   - Cancellation/reschedule

2. **What meeting is this about?** (topic, who attended, key outcomes if it already happened)

3. **Who's receiving this email?** (colleagues, clients, executives)

4. **Tone preference?** (formal, friendly professional, casual)
```

### User's Answers
```
1. Follow-up after a meeting
2. Product roadmap review with engineering leads, we decided on Q1 priorities
3. The attendees plus their managers who couldn't join
4. Professional but not stiff
```

### Optimized Prompt
```xml
<context>
I need to send a follow-up email after a business meeting.
</context>

<meeting_details>
{{meeting_details}}
</meeting_details>

<instructions>
Write a professional follow-up email that:

1. Thanks attendees for their time
2. Summarizes the key discussion points (3-5 bullets)
3. Lists action items with owners and deadlines
4. Proposes next steps or follow-up meeting if applicable
5. Ends with a clear call-to-action
</instructions>

<constraints>
- Tone: Professional but warm
- Length: 150-250 words
- Format: Ready to send (include subject line)
</constraints>

<output_format>
Subject: [Subject line]

[Email body]
</output_format>
```

---

## Example 3: Classification Task

### User's Input
```
categorize these support tickets
```

### Gap Analysis
```
TASK CLARITY: ✓ - Categorize is clear
CONTEXT: ~ - Support tickets, but for what product/service?
INPUT: ~ - "Tickets" mentioned but format unknown
OUTPUT FORMAT: ✗ - Just categories? With reasoning?
AUDIENCE: ✗ - Support team? Management?
CONSTRAINTS: ✗ - Predefined categories or create them?
SUCCESS CRITERIA: ✗ - Accuracy requirements?
EXAMPLES: ✗ - None provided

CRITICAL GAPS: Output Format, Constraints (categories), Examples
```

### Clarifying Questions Asked
```markdown
I can build a ticket categorization prompt. Need to know:

1. **Do you have predefined categories?** If yes, list them. If no, should Claude create them?

2. **What should the output include?**
   - Just the category
   - Category + priority level
   - Category + reasoning for the classification

3. **How will this be used?** (auto-routing, reporting, analysis)

4. **Can you share 2-3 example tickets and how you'd categorize them?** This dramatically improves accuracy.
```

### User's Answers
```
1. Yes: Billing, Technical, Account, Feature Request, General Inquiry
2. Category + priority (High/Medium/Low) + one-line reasoning
3. Auto-routing to the right team
4. Sure:
   - "I was charged twice" → Billing, High, duplicate charge needs refund
   - "Can't log in" → Account, High, blocking user access
   - "Would be nice to have dark mode" → Feature Request, Low, enhancement idea
```

### Optimized Prompt
```xml
<role>
You are a customer support triage specialist. Your job is to accurately categorize incoming tickets so they reach the right team quickly.
</role>

<categories>
- BILLING: Payment issues, refunds, subscription changes, invoices
- TECHNICAL: Bugs, errors, performance issues, integration problems  
- ACCOUNT: Login issues, password resets, profile changes, permissions
- FEATURE: Feature requests, product suggestions, feedback
- GENERAL: Questions, how-to inquiries, documentation requests
</categories>

<examples>
<example>
<ticket>I was charged twice this month and need a refund</ticket>
<classification>
Category: BILLING
Priority: HIGH
Reasoning: Duplicate charge requires immediate financial correction
</classification>
</example>

<example>
<ticket>The export button doesn't work in Chrome</ticket>
<classification>
Category: TECHNICAL
Priority: MEDIUM  
Reasoning: Feature malfunction in specific browser, workaround may exist
</classification>
</example>

<example>
<ticket>Can you add dark mode to the mobile app?</ticket>
<classification>
Category: FEATURE
Priority: LOW
Reasoning: Enhancement request, not blocking current usage
</classification>
</example>
</examples>

<tickets>
{{tickets}}
</tickets>

<instructions>
For each ticket:
1. Read the ticket content carefully
2. Identify the primary issue type
3. Match to the most appropriate category
4. Assess priority based on user impact and urgency
5. Provide brief reasoning

Output as a structured list.
</instructions>

<output_format>
| Ticket # | Category | Priority | Reasoning |
|----------|----------|----------|-----------|
| 1 | [CAT] | [HIGH/MEDIUM/LOW] | [Brief reason] |
</output_format>
```

---

## Example 4: Decision Support

### User's Input
```
help me decide which vendor to pick
```

### Gap Analysis
```
TASK CLARITY: ~ - "decide" but what's the decision framework?
CONTEXT: ✗ - What kind of vendor? For what purpose?
INPUT: ✗ - What info about vendors is available?
OUTPUT FORMAT: ✗ - Recommendation format unknown
AUDIENCE: ✗ - Personal decision or presenting to others?
CONSTRAINTS: ✗ - Budget? Timeline? Requirements?
SUCCESS CRITERIA: ✗ - What makes a vendor "best"?

CRITICAL GAPS: Context, Input (vendor info), Constraints, Success Criteria
```

### Clarifying Questions Asked
```markdown
I'll help you build a vendor evaluation prompt. Need to understand:

1. **What type of vendor/service?** (software, contractor, supplier, etc.)

2. **What information do you have about each vendor?** (pricing, features, reviews, proposals)

3. **What are your key criteria?** Rank these or add your own:
   - Price/cost
   - Features/capability
   - Reliability/uptime
   - Support quality
   - Integration ease
   - Scalability
   - Security/compliance

4. **Any deal-breakers?** (budget ceiling, must-have features, required certifications)

5. **Who makes the final call?** (just you, or presenting to a committee)
```

### User's Answers
```
1. CRM software for our sales team
2. Have pricing sheets, feature lists, and some G2 reviews for 3 options
3. Most important: Integration with our existing tools, then ease of use, then price
4. Must integrate with Slack and HubSpot. Budget max $50/user/month
5. Presenting recommendation to VP of Sales
```

### Optimized Prompt
```xml
<role>
You are a procurement analyst with expertise in vendor evaluation and risk assessment.
</role>

<context>
I need to select a vendor for {{service_type}}. This decision will impact our operations for the next {{contract_length}}.
</context>

<vendor_information>
{{vendor_details}}
</vendor_information>

<evaluation_criteria>
{{criteria_if_any}}
</evaluation_criteria>

<instructions>
Conduct a systematic vendor evaluation:

<thinking>
Work through this analysis step by step:

1. Criteria Definition
   - If criteria provided, validate completeness
   - If not provided, recommend standard criteria for this service type
   - Assign weights based on business importance (must total 100%)

2. Vendor Assessment
   - Score each vendor on each criterion (1-5 scale)
   - Document evidence/reasoning for each score
   - Calculate weighted totals

3. Risk Analysis
   - Identify top 3 risks for each vendor
   - Assess likelihood and impact
   - Note any red flags or concerns

4. Total Cost of Ownership
   - Direct costs (pricing)
   - Indirect costs (implementation, training, support)
   - Switching costs and lock-in considerations

5. Recommendation
   - Synthesize findings
   - Consider both quantitative scores and qualitative factors
   - Identify the winning vendor with clear justification
</thinking>

Provide your final recommendation with supporting analysis.
</instructions>

<output_format>
## Evaluation Criteria & Weights
[Table of criteria with weights]

## Vendor Scoring Matrix
[Comparison table]

## Risk Assessment
### Vendor A
- Risk 1: [Description] | Likelihood: [H/M/L] | Impact: [H/M/L]
[Continue for each vendor]

## Cost Comparison
[Total cost breakdown]

## Recommendation
**Selected Vendor:** [Name]
**Confidence Level:** [High/Medium/Low]

**Key Reasons:**
1. [Reason 1]
2. [Reason 2]
3. [Reason 3]

**Caveats:**
- [Any concerns or conditions]
</output_format>
```

---

## Example 5: Code Generation

### User's Input
```
write a function to process user data
```

### Gap Analysis
```
TASK CLARITY: ~ - "process" is vague - validate? transform? export?
CONTEXT: ✗ - What application? What problem?
INPUT: ✗ - What user data? Format?
OUTPUT FORMAT: ✗ - Return type? Side effects?
CONSTRAINTS: ✗ - Language? Framework? Error handling?
EDGE CASES: ✗ - Invalid data? Missing fields?

CRITICAL GAPS: Task Clarity (what "process" means), Input, Constraints (language), Edge Cases
```

### Clarifying Questions Asked
```markdown
I'll help build a code generation prompt. Quick questions:

1. **What does "process" mean specifically?**
   - Validate data against rules
   - Transform/clean data
   - Filter/search data
   - Export/format data
   - Something else?

2. **What programming language?**

3. **What does user data look like?** (fields, types, example structure)

4. **What should happen with invalid/malformed data?**
   - Throw error
   - Skip and log
   - Return partial result with warnings

5. **Any framework or style preferences?** (typing, async, specific patterns)
```

### User's Answers
```
1. Validate and sanitize - check required fields, trim strings, normalize email format
2. TypeScript
3. { name: string, email: string, phone?: string, age: number }
4. Return a result object with { valid: boolean, data?: CleanUser, errors?: string[] }
5. Use strict typing, no any. Pure function, no side effects.
```

### Optimized Prompt
```xml
<context>
Writing a TypeScript utility function for user data validation and sanitization.
</context>

<requirements>
Input type:
interface RawUser {
  name: string;
  email: string;
  phone?: string;
  age: number;
}

Output type:
interface ValidationResult {
  valid: boolean;
  data?: CleanUser;
  errors?: string[];
}

Processing rules:
1. Required fields: name, email, age (phone optional)
2. Trim all string fields
3. Normalize email to lowercase
4. Validate email format (basic regex acceptable)
5. Age must be positive integer
</requirements>

<constraints>
- Language: TypeScript with strict typing
- No 'any' type
- Pure function (no side effects)
- Return errors array if invalid, cleaned data if valid
</constraints>

<instructions>
Write the function following these steps:
1. Define the input and output interfaces
2. Implement validation for each field
3. Implement sanitization (trim, lowercase email)
4. Return appropriate result object
5. Add JSDoc comment explaining usage

Include a brief usage example as a comment.
</instructions>
```

---

## Key Takeaway

Every example above required **3-5 clarifying questions** to transform a vague request into an effective prompt. The questions:
- Filled critical gaps in the completeness framework
- Gathered user-specific constraints and preferences  
- Collected examples that dramatically improve output quality
- Confirmed understanding before generating

Never skip the questions step.
