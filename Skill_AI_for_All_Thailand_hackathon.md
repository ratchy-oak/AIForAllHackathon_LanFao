---
name: ai-for-all-thailand-ideation
description: "Run the AI-DLC Ideation phase — a structured 9-stage workflow that takes a project idea from raw intent to an approved initiative brief. Begins with pillar selection and free-text idea input to frame initiatives around Thailand's strategic development priorities. Use this skill whenever the user wants to start a new project, validate an idea, frame an initiative, do ideation, scope a feature, assess feasibility, or produce an initiative brief before development begins. Also triggers on: 'structural reform ideation', 'start ideation', 'new project ideation', 'frame this initiative', 'scope this project', 'validate this idea'."
display_name: "AI for All Thailand: Ideation"
icon: "🇹🇭"
trigger: structural reform ideation start ideation new project ideation frame initiative scope project validate idea feasibility assessment intent capture ai-dlc ideation thailand reform
inputs:
  - name: intent
    description: "The user's raw idea or problem statement (optional — if not provided, Step 0 will guide them through pillar selection and idea input first)"
    type: string
    required: false

---

# AI-DLC Ideation Workflow — AI for All Thailand

## Overview

This skill implements the **Ideation phase (Phase 1)** of the AI-DLC v2 methodology, customized for the **AI for All Thailand Innovation Sprint**. It guides the user through 9 structured stages — starting with pillar selection and free-text idea articulation, then through intent capture to producing an approved initiative brief ready for the Inception phase.

**Step 0 (Pillar Selection & Idea Input)** presents 6 strategic initiative areas as broad theme areas. The user selects a pillar via decision card, then **must articulate both the problem they want to address AND their proposed idea in free text** — no background data, problem statements, or statistics are provided by the system. This ensures participants think critically, do their own research, and bring original perspectives grounded in their own understanding.

The workflow produces artifacts in English by default and respects approval gates at key decision points.

> **Source:** This skill implements the Ideation phase from the [AI-DLC Workflows 2.0 Specification](https://github.com/awslabs/aidlc-workflows/blob/v2/assets/AI-DLC-Workflows-2.0-Specification.pdf) and the [AI-DLC methodology blog post](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/).

---

## How artifacts work in this workflow (read this first)

**This workflow runs in a chat session with no file system.** You cannot write files, you cannot read files back, and you cannot check whether a file exists.

Therefore:

1. **Every artifact is produced as a titled document in the conversation**, which the user downloads themselves. Refer to artifacts by their **title**, never by a file path. Do not claim to have "saved" anything.
2. **Do not rely on recall of earlier prose.** There are no files to reopen, so anything a later stage needs must still be present in the conversation as something you can point to. To make that reliable, **every stage ends with a Carry-Forward Block** (see below), and later stages compose from those blocks.
   **One exception, and it is deliberate:** Stage 8b composes from the **full text of the Content Document produced in Stage 8a**, not from a Carry-Forward Block. That document is the immediately preceding artifact in the same conversation and it is the record of the work, so 8b reads it directly. Stage 8b is required to prove it did — see the 8b gate.
3. **Never assert that a file exists.** A verification step may only check things visible in the conversation — a heading, a count, a named section, a quoted value.
4. **A check must compare two things you can both see.** If a verification step can be satisfied by writing "yes" or "none" from memory, it is not a check. Every gate in this workflow therefore asks you to reproduce a *value* or a *count*, so the user can compare it themselves.

### The Carry-Forward Block

At the end of each stage, after the artifact, emit a short block in exactly this shape:

```
**Carry-Forward (Stage N):**
- <fact, decision, number or constraint a later stage needs> [source: ...]
- <…>
```

Keep it to 3–8 bullets. Include every number, name, date, target and constraint that a later stage or the final summary will need, and keep the `[source: ...]` tag attached to each one. This block is the working memory of the workflow — it is what Stage 8a composes from.

### The two deliverables

The workflow ends with **two artifacts, and both are submitted**:

| Artifact | Contains | Role |
|---|---|---|
| **Content Document** (Markdown) | All substantive content — every claim, number, table, decision and source | The record of the thinking |
| **Summary Presentation** (HTML) | The same content, presented as slides | The presentation of that thinking |

The Markdown is produced **first**, from the Carry-Forward Blocks. The HTML is produced **second, from the Markdown**. That order is deliberate and is covered in Stage 8.

---

## Reference: AI-DLC v2 Ideation Phase

The Ideation phase validates an initiative through these stages:

| # | Stage | Purpose | Condition |
| --- | --- | --- | --- |
| 1 | Intent Capture & Framing | Capture what the user wants and why | ALWAYS |
| 2 | Market Research | Competitive analysis, build-vs-buy | CONDITIONAL |
| 3 | Feasibility & Constraints | Technical feasibility, constraints, risks | CONDITIONAL |
| 4 | Scope Definition | Define boundaries, backlog, success criteria | ALWAYS |
| 5 | Team Formation | Assess team needs and composition | CONDITIONAL |
| 6 | Rough Mockups | Wireframes, user flows (UI projects only) | CONDITIONAL |
| 7 | Approval & Handoff | Produce initiative brief, get sign-off | ALWAYS |
| 8a | Content Document | Consolidate all content into Markdown | ALWAYS |
| 8b | Summary Presentation | Render that content as an HTML deck | ALWAYS |

**Auto-skip rules:**

- **bugfix / refactor / infra / security-patch**: Skip 2, 3 — go straight to 4
- **poc / bugfix / refactor**: Skip 5 — go straight to 7
- **No UI component**: Skip 6
- **8a and 8b are never skipped.**

## AI-DLC v2 Governance Patterns

The following patterns from the AI-DLC v2 specification are enforced throughout this workflow:

### 1. Source Citation (Inline Source Tags)

All claims in artifacts must carry inline source tags indicating where the information came from. Format: `[source: <origin>]` — e.g., `[source: user interview]`, `[source: web research]`, `[source: assumption]`.

**Source tags must survive into the final Content Document.** They are how a reader tells a researched claim from an assumed one. Stage 8a has a dedicated Evidence & Sources section for this purpose. Stripping source tags at any stage is a defect.

### 2. Assumption Confirmation Gate

Retained assumptions flagged in Stage 1 **require explicit user confirmation** before the approval gate in Stage 7 can pass. Unconfirmed assumptions must be resolved or explicitly accepted with rationale.

### 3. Product Lead Reviewer Pass

Before the final approval gate (Stage 7), the workflow runs a quality review that checks:

- Completeness: Are all required artifact sections present?
- Consistency: Do artifacts reference each other correctly?
- Traceability: Can every scope item be traced back to the intent statement?
- Feasibility alignment: Do scope commitments respect identified constraints?

### 4. Learning Loop

When the user corrects or revises any artifact, record the correction pattern. These corrections should be noted in the Decision Log (Stage 1.7) so that future runs of this workflow can learn from past feedback. Pattern: `[correction: <what was wrong> → <what was fixed>]`.

## Artifact Language Convention

All artifacts and outcomes are produced in **English** by default.

If the user explicitly requests another language (e.g., Thai), produce bilingual output with the requested language first, English below.

## Workflow Steps

> ⚠️ **CRITICAL: ALWAYS start with Step 0.** Regardless of whether the user provides an idea upfront, you MUST present the pillar selection (Step 0) FIRST. Do NOT skip to Step 1. Even if the user says "I want to build X", begin by asking them to select a pillar. Their idea will be used to enrich the framing in Step 0, not to bypass it.

### Decision Card Input Rule — Steps 1–8

For every decision card, choice list, approval choice, or other multi-option selection **after Step 0**, the first option MUST be an editable free-text option.

- **Step 0 is excluded.** Keep the six fixed strategic initiative areas exactly as defined in Step 0.
- From **Step 1 onward**, place **Option 1** as: `✏️ Other / Type your own input`.
- The user must be able to type a custom answer instead of selecting one of the predefined options.
- Treat the user's typed input as a valid selection and carry it forward exactly as user-provided, with `[source: participant]` where source tags apply.
- Keep all existing predefined options after the custom-input option unless a specific step already has a different required structure.
- If the user selects the custom-input option, ask only the minimum clarification needed to interpret it, then continue the workflow using that input.
- Do not reinterpret a custom answer into one of the predefined options unless the user explicitly asks for that.
- For approval gates such as **Approve / Revise / Reject**, Option 1 must also be the editable custom-input option; the predefined approval choices follow it.
- If a step does not use a decision card or multi-option selection, this rule does not require adding one.

This rule changes only the interaction format. It does not change the substantive purpose, validation criteria, artifact requirements, or approval logic of any step.

### Step 0: Pillar Selection & Idea Articulation (Pre-Stage)

- **Mode**: `agentic`
- **Input**: None (this is the entry point)
- **Output**: Selected pillar context + user's original idea passed to Step 1
- **Condition**: ALWAYS — this step runs before the formal workflow begins

**What to do:**

1. **Present the 6 Strategic Initiative Areas** as a decision card for the user to select ONE theme area. **Do not add a custom/free-text Option 1 here; Step 0 is explicitly excluded from the Decision Card Input Rule above.**

Present this exact decision card markup to the user:

DECISION_CARD_START
question: "Which strategic initiative area would you like to innovate in?"

Option 1:
  label: "📚 Education & Future Skills"
  description: "Enable equitable learning, stronger teaching support, and future-ready skills for every learner (Learning equity · Teachers · Career readiness)"

Option 2:
  label: "🏥 Health & Aging Society"
  description: "Make healthcare more accessible, preventive, and supportive for an aging society (Access · Prevention · Independent living)"

Option 3:
  label: "🏪 SME & Local Economy"
  description: "Empower Thailand's SMEs, local merchants, tourism operators, and community enterprises to compete in the digital economy (Productivity · Commerce · Tourism · Creative economy)"

Option 4:
  label: "🚍 Smart Mobility & Cities"
  description: "Create safer, more efficient, and more sustainable urban and transportation systems (Safer journeys · Traffic · Accessible transport)"

Option 5:
  label: "🌾 Agriculture & Food"
  description: "Build AI-powered agriculture that increases farmer income, food security, and climate resilience (Farm intelligence · Yield · Supply-chain resilience)"

Option 6:
  label: "🌪️ Resilience Communities"
  description: "Protecting communities against disasters, climate change, and environmental risks through predictive and proactive AI solutions (Disaster · Climate · Environment · Safety)"
DECISION_CARD_END

> ⚠️ **CRITICAL: Do NOT provide any background data, statistics, research findings, or problem context to the user.** The decision card above shows ONLY the pillar name and a brief one-line description. Do NOT present reference tables, PISA scores, GDP figures, debt-to-GDP ratios, or any other supporting data. The participant must bring their own understanding of the problem.

2. **Once the user selects a pillar**, ask the user to **type both the problem they see AND their idea in free text**. Do NOT present any background data, statistics, pre-written problem statements, or "How Might We" questions. Instead, prompt the user:

> **Now it's your turn.** You've selected a pillar — but we want to hear YOUR perspective.
>
> Please describe:
> 1. **The problem as you see it** — What specific challenge within this pillar concerns you most? What evidence or observations support your view?
> 2. **Your idea** — What approach or solution do you envision? Who would benefit? How would it work?
>
> Type your response below — be as specific or as broad as you like. There are no wrong answers, but the more detail you provide, the stronger your initiative brief will be.

> ⚠️ **CRITICAL: Do NOT supplement the user's input with your own knowledge of the problem.** Do NOT add statistics, research findings, or background context that the user did not provide. The user's own words and data are the ONLY input. If the user's problem description is thin or lacks evidence, that is acceptable — the follow-up questions (step 3b) are the place to probe deeper, NOT to inject system-provided data.

3. **Once the user types their idea**, do the following:

   a. **Validate relevance**: Check that the idea relates to the selected pillar. If it's clearly unrelated, gently point this out and ask the user to refine or confirm they want to proceed anyway.

   b. **Ask 2-3 targeted follow-up questions** to deepen the idea. These should be dynamically generated based on what the user wrote — NOT generic templates. Focus on drawing out MORE from the user, not providing information TO the user. Examples of the *types* of questions (adapt to context):
      - What specific population segment or geography within Thailand are you targeting?
      - What evidence or data have you seen that supports this problem framing?
      - What existing systems, behaviors, or infrastructure would your solution build on or disrupt?
      - What would success look like in 12 months? In 3 years?
      - What's the biggest risk or obstacle you foresee?

   c. **Synthesize** the user's idea and follow-up answers into a structured intent statement that feeds into Step 1. Use ONLY information the user provided. Include the selected pillar name as context for all subsequent stages.

4. **Do NOT generate a "How Might We" question for the user.** The user's own words are the primary input. You may reframe their idea into clearer language in the intent statement, but always preserve the user's original framing and intent.

5. **Do NOT provide any reference data, statistics, or background research at any point during Step 0.** The system's role is to LISTEN, PROBE, and SYNTHESIZE — not to TEACH or INFORM.

**After completing Step 0**, carry the following forward to Step 1:

```
**Carry-Forward (Step 0):**
- Selected pillar: <name only>
- User's problem description, verbatim: "<…>" [source: participant]
- User's idea, verbatim: "<…>" [source: participant]
- Follow-up answers: <…> [source: participant]
- Synthesized intent statement: <reframed, using ONLY user-provided information>
```

Preserving the participant's **verbatim** words matters: it is the only record of what the participant brought, as distinct from what this workflow added.

---

### Step 1: Intent Capture & Framing (Stage 1)

- **Mode**: `agentic`
- **Input**: `{{intent}}` — the user's raw idea or problem statement
- **Output**: A titled document — **"01 — Intent Capture"**
- **Validate**: Document contains intent statement, stakeholder map, assumptions, and open questions
- **On failure**: Ask clarifying questions to fill gaps

**What to do:**

1. If the user provides only a brief sentence, ask targeted questions to expand:
   - What problem or opportunity are you addressing?
   - Who are the stakeholders?
   - Are there critical constraints or timelines?
   - What does success look like?

2. Produce the artifact with these sections:
   - **Intent Statement**: Clear 1-2 sentence summary of what and why. Include `[source: ...]` tags for each claim.
   - **Stakeholder Map**: Who's affected, who decides, who benefits. Include `[source: ...]` tags.
   - **Assumptions**: What we're assuming to be true — each marked as `[status: unconfirmed]` for later validation
   - **Open Questions**: Things we need to find out — prioritized by impact
   - **Project Type Detection**: Classify as greenfield/brownfield/bugfix/refactor/poc/infra/security-patch
   - **Customer Journey — Current State**: A lightweight summary of how things work today — key pain points, friction, and inefficiencies the user/customer experiences. Keep it concise (3-5 bullet points or a simple flow). This sets the "before" baseline for measuring improvement.
   - **Supporting Agents**: aidlc-architect-agent provides technical input on feasibility signals

3. Based on the detected project type, recommend which stages to include/skip and present as a decision card for the user to confirm. The user's choice is the final word — auto-skip is a suggestion, not a mandate.

Present the artifact titled **"01 — Intent Capture"**, then emit the **Carry-Forward (Stage 1)** block: the intent statement, project type, the current-state pain points, and every unconfirmed assumption.

### Step 2: Market Research (Stage 2)

- **Mode**: `agentic`
- **Input**: Intent from Step 1
- **Output**: A titled document — **"02 — Market Research"**
- **Research Method**: Use **Deep Research** (load `deep_research` skill) for comprehensive, multi-track, cited analysis. Only fall back to basic web search if Deep Research is unavailable. Deep Research produces better output by running parallel research tracks with proper source citations.
- **Validate**: Contains competitive landscape and build-vs-buy analysis
- **On failure**: Note gaps and proceed with available information
- **Condition**: SKIP if project_type is bugfix, refactor, infra, or security-patch

**What to do:**

**Preferred approach (Deep Research):**

1. Load `deep_research` skill
2. Design 3-5 research tracks covering: (a) Local competitive landscape, (b) Global best practices/benchmarks, (c) Partnership/vendor feasibility, (d) Economic/pricing analysis
3. Run all tracks in parallel via `deep_research_information_gathering`
4. Compose final report with `deep_research_finalize_deliverable`
5. Present that report as the artifact **"02 — Market Research"**

This produces a report with 15-20+ cited sources, structured analysis, and evidence-backed recommendations — far superior to ad-hoc web searches.

**Fallback approach (basic web search):**

1. Research the competitive landscape using web search:
   - Who else is doing something similar?
   - Are there existing solutions?
   - What's our differentiator?

2. Produce the artifact with:
   - **Competitive Landscape**: Key players and their approaches `[source: web research]`
   - **Build vs Buy Analysis**: Pros/cons of each path
   - **Market Gaps**: Opportunities identified
   - **Recommendation**: Suggested direction with rationale and confidence level

Present the artifact titled **"02 — Market Research"**, then emit the **Carry-Forward (Stage 1.2)** block.

> **Carry the sources, not just the conclusions.** This is the only stage that produces external evidence, and it is the stage whose findings are most often reduced to an uncited assertion by the time the final summary is written. In the Carry-Forward Block, keep each named competitor, each benchmark figure and each market number **with its source attached**. If a market-gap claim such as "no existing solution does X" is the participant's inference rather than a research finding, tag it `[source: assumption]` — do not let it graduate into a fact.

### Step 3: Feasibility & Constraints (Stage 3)

- **Mode**: `agentic`
- **Input**: Intent + Market Research
- **Output**: A titled document — **"03 — Feasibility & Constraints"**
- **Validate**: Contains feasibility verdict, constraint register, and initial RAID log
- **On failure**: Flag areas needing further investigation
- **Condition**: SKIP if project_type is bugfix, refactor, infra, or security-patch

**What to do:**

1. Assess feasibility across dimensions:
   - Technical feasibility
   - Resource & capability feasibility
   - Timeline feasibility
   - Budget/cost feasibility

2. Produce the artifact with:
   - **Feasibility Assessment**: Go/No-Go/Conditional for each dimension
   - **Constraint Register**: Hard constraints that cannot be changed
   - **RAID Log**: Risks, Assumptions, Issues, Dependencies
   - **Verdict**: Overall feasibility rating with confidence level
   - **Supporting Agent Input**: aidlc-aws-platform-agent (cloud/infra feasibility) + aidlc-compliance-agent (regulatory/compliance constraints)

Present the artifact titled **"03 — Feasibility & Constraints"**, then emit the **Carry-Forward (Stage 1.3)** block. Keep **every dimension you assessed and its rating** — a later stage must not silently reduce a five-dimension assessment to three.

### Step 4: Scope Definition (Stage 4)

- **Mode**: `agentic`
- **Input**: All prior Carry-Forward Blocks
- **Output**: A titled document — **"04 — Scope Definition"**
- **Validate**: Contains clear in-scope/out-of-scope boundaries, success criteria, and backlog
- **On failure**: Ask user to clarify ambiguous boundaries

**What to do:**

1. Define clear boundaries:
   - What's IN scope?
   - What's OUT of scope?
   - What are the success criteria?

2. Produce the artifact with:
   - **Scope Statement**: Clear description of what will and won't be delivered
   - **Success Criteria**: Measurable outcomes that define "done"
   - **Intent Backlog**: High-level features/capabilities prioritized (MoSCoW). Each item must trace back to the intent statement from Stage 1.
   - **Out of Scope**: Explicitly excluded items (prevents scope creep)
   - **Assumptions & Dependencies**: Validated/unvalidated

3. Present scope summary to user for approval before proceeding (approval gate).

Present the artifact titled **"04 — Scope Definition"**, then emit the **Carry-Forward (Stage 1.4)** block, including **the full success-criteria list with its targets** and the complete out-of-scope list.

### Step 5: Team Formation (Stage 5)

- **Mode**: `agentic`
- **Input**: Scope definition + project complexity
- **Output**: A titled document — **"05 — Team Formation"**
- **Validate**: Contains team assessment and composition plan
- **On failure**: Suggest default team structure
- **Condition**: SKIP if project_type is poc, bugfix, or refactor

**What to do:**

1. Assess team needs based on scope:
   - What skills are needed?
   - How many people?
   - Any team constraints?

2. Produce the artifact with:
   - **Skills Assessment**: Required capabilities mapped to scope
   - **Team Composition**: Recommended roles and responsibilities (mob composition plan)
   - **RACI Matrix**: Responsible, Accountable, Consulted, Informed
   - **Gaps & Risks**: Missing capabilities and mitigation

Present the artifact titled **"05 — Team Formation"**, then emit the **Carry-Forward (Stage 1.5)** block with the **full role list**, not just a headcount.

### Step 6: Rough Mockups (Stage 6)

- **Mode**: `agentic`
- **Input**: Scope + user stories from intent
- **Output**: A titled document — **"06 — Rough Mockups"** (plus optional inline HTML wireframes)
- **Validate**: Contains visual flowcharts (HTML/Mermaid) and interactive screen mockups (HTML)
- **On failure**: Produce text-based flow descriptions instead
- **Condition**: SKIP if the project has no UI/UX component

**What to do:**

1. Create lightweight conceptual designs:

**Preferred approach (Visual HTML):**

**A. Flowcharts — Mermaid rendered in HTML:**

- Load `html_design` skill
- Produce an HTML document using Mermaid.js CDN (`https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js`) to render flowcharts
- Color-code nodes by function: 🔵 Entry, 🟡 Decision, 🟠 Payment, 🟢 Success, 🔴 Error
- Use emoji in node labels for readability
- Separate each flow into clear sections (Mermaid code can be extracted for other documentation)
- Present it inline as a titled artifact, e.g. **"06a — Flowchart: <flow name>"**

**B. Screen Mockups — HTML/CSS phone-style wireframe:**

- Produce interactive HTML mockups displayed as phone-frame (375px width, rounded corners)
- Use realistic UI elements (buttons, cards, pricing tables, navigation)
- Show actual pricing, copy, and proposed UX patterns
- Include hover states and clear visual hierarchy
- Present inline as a titled artifact, e.g. **"06b — Mockup: <screen name>"**

**Benefits:** Easy to understand, presentable to stakeholders, Mermaid code can be exported to Confluence/Notion/Docs, HTML mockups viewable in browser immediately

**Fallback (text-only):** If html_design skill is unavailable, use text-based descriptions instead

2. Produce the artifact with:
   - **User Flows**: Step-by-step user journeys (text/mermaid diagrams)
   - **Key Screens**: Descriptions of primary interfaces (with aidlc-product-agent supporting prioritization)
   - **Design Principles**: Guiding aesthetic and UX principles
   - **Customer Journey — To-Be State**: A detailed visual journey map showing the proposed future experience. Use a mermaid flowchart or table comparing Current → To-Be at each touchpoint. Include: touchpoints, emotions/satisfaction, actions, pain points removed, and new value delivered. Cross-reference pain points from the Current State (Stage 1) and show how each is resolved.
   - **Concept Notes**: Rationale for design choices

Present the artifact titled **"06 — Rough Mockups"**, then emit the **Carry-Forward (Stage 1.6)** block. Because mockups are visual, the Carry-Forward Block must describe each screen **in words** — a later stage cannot re-read the mockup markup.

### Step 7: Approval & Handoff (Stage 7)

- **Mode**: `agentic`
- **Input**: All prior Carry-Forward Blocks
- **Output**: A titled document — **"07 — Initiative Brief"**
- **Validate**: Contains complete initiative brief with decision log and clear next-steps
- **On failure**: Identify missing sections and ask user to fill gaps

**What to do:**

1. **Assumption Confirmation Gate**: Before proceeding, review all assumptions from Stage 1 that are still marked `[status: unconfirmed]`. Present them to the user and require explicit confirmation or rejection for each.
2. **Product Lead Reviewer Pass**: Run the quality review checklist (completeness, consistency, traceability, feasibility alignment). Flag any issues.
3. Synthesize all prior work into a comprehensive initiative brief:
   - Executive summary
   - Key decisions made
   - What happens next

4. Produce the artifact with:
   - **Executive Summary**: 1-page overview of the initiative
   - **Decision Log**: All decisions made during Ideation with rationale
   - **Corrections Log**: Any corrections made during the workflow `[correction: ... → ...]`
   - **Stages Completed**: Which stages ran, which were skipped and why
   - **Next Steps**: What needs to happen in Inception (Phase 2)
   - **Go/No-Go Recommendation**: Final recommendation

5. Present the brief to the user with a clear approval gate. Because this is a multi-option choice after Step 0, Option 1 must be editable:
   - Option 1: ✏️ Other / Type your own input — type a custom decision or instruction
   - Option 2: ✅ Approve — proceed to the summary artifacts
   - Option 3: 🔄 Revise — go back to a specific stage
   - Option 4: ❌ Reject — archive and stop

   If the user types a custom decision, treat it as the user's explicit instruction and determine the next workflow action from that input. If it is ambiguous, ask one focused clarification question.

Present the artifact titled **"07 — Initiative Brief"**, then emit the **Carry-Forward (Stage 7)** block with the decision log and the Go/No-Go verdict plus its conditions.

---

## Stage 8 — The two submission artifacts

Stage 8 produces the two documents that get submitted. It runs as **two separate stages in two separate turns**:

| Stage | Produces | Built from |
|---|---|---|
| **1.8a** | **Content Document** (Markdown) | The Carry-Forward Blocks of every completed stage |
| **1.8b** | **Summary Presentation** (HTML) | **The Content Document from 1.8a** |

**Do not attempt 8a and 8b in the same response.** Complete 8a, show its gate, and stop. Produce 1.8b in the next turn. Two reasons: a single turn has a finite output budget and the second artifact is what gets truncated or dropped when that budget runs out; and 1.8b cannot faithfully render content that has not been written down yet.

**The direction of dependency is fixed: content first, presentation second.** The HTML is a rendering of the Markdown. It may reformat, condense wording and add visual structure. It may **not** introduce a substantive fact that is absent from the Markdown, because the Markdown is the record of the work.

---

### Stage 1.8a — Content Document (Markdown)

- **Mode**: `agentic`
- **Input**: Every Carry-Forward Block produced so far, plus the Stage 7 brief
- **Output**: A titled Markdown artifact — **"`<ProjectName>` — Content Document"**
- **Condition**: ALWAYS

**What to do:**

Consolidate the workflow into one self-contained Markdown document. Work from the **Carry-Forward Blocks**, which is where the numbers and sources live.

Use a level-2 Markdown heading (`##`) for each section, numbered, in this order. Each section is marked with the stage that feeds it:

| # | Section | Fed by | If that stage did not run |
|---|---|---|---|
| 1 | **Overview** — project name, one-line vision, pillar, owner, date, status | Step 0, 1 | always present |
| 2 | **Problem Statement** — including the participant's own framing, verbatim | Step 0, 1 | always present |
| 3 | **Solution Summary** | Step 0, 1 | always present |
| 4 | **Confirmed Parameters** — the decisions locked during the workflow | 1, 4, 7 | always present |
| 5 | **Competitive Landscape** — with sources | 12 | **omit the section** |
| 6 | **Feasibility Assessment** — every dimension assessed, with its rating | 3 | **omit the section** |
| 7 | **Screens & Flows** — each screen and each user flow described **in words** | 6 | **omit the section** |
| 8 | **Architecture / How It Works** | 1, 3, 6 | keep, but see the rule below |
| 9 | **Scope** — P0/P1/P2 and explicit out-of-scope | 4 | always present |
| 10 | **Success Criteria** — the full list with targets | 4 | always present |
| 11 | **Team Composition** — the full role list | 5 | **omit the section** |
| 12 | **Timeline** — phases with durations | 4, 7 | keep, but see the rule below |
| 13 | **Key Risks & Mitigations** | 3, 7 | keep, but see the rule below |
| 14 | **Open Assumptions** — each with how it will be validated | 1, 7 | always present |
| 15 | **Decision Log** — every decision with its rationale, plus corrections | 7 | always present |
| 16 | **Go / No-Go** — verdict, rationale, conditions | 7 | always present |
| 17 | **Evidence & Sources** — every external claim with its source; every unverified claim marked as an assumption | all | always present |

Rules:

- **Omit, do not invent.** Where the table says "omit the section", leave it out entirely and record it under *Stages skipped* in the gate. Where it says "keep, but see the rule below": if no stage produced that material, keep the heading and write a single line — `Not assessed in this run (Stage 1.3 was skipped).` **Do not invent content to fill a required heading.** An honest gap is worth more than a fabricated section, and fabricating one also corrupts the assumption count in the gate.
- **Number the sections you actually include, consecutively from 1**, closing any gaps left by omitted sections. The count in the gate is the number of sections present.
- **Preserve numbers, names, dates and targets exactly** as established in the Carry-Forward Blocks. Do not round, re-estimate or re-word a figure.
- **Keep the `[source: ...]` tags** inline in sections 1–16, and summarise them in the Evidence & Sources section. A document with no sources reads as though nothing was researched.
- **Mark unverified claims as assumptions.** In particular, a claim that no competitor or precedent exists is an assumption unless Stage 1.2 actually established it.
- **Describe visuals in words.** Any mockup, screen or flow from Stage 1.6 belongs in *Screens & Flows* as prose — layout, fields, states, and what the user does. Stage 1.8b will render these descriptions as diagrams and wireframes, so anything missing here cannot legitimately appear in the deck.
- Plain Markdown only: headings, lists, tables, bold. **No HTML, no CSS, no JavaScript, no Mermaid source.**

**Stage 1.8a gate.** After the document, output the block below as **normal Markdown, not inside a code fence** (the fence here only shows you the shape):

```
## Stage 1.8a check
- Sections present: <N>
- Section headings, in order: <the exact text of every `##` heading, comma-separated>
- Sections omitted, and which stage was skipped: <…, or "none">
- Sections kept but not assessed: <…, or "none">
- Inline source tags in sections 1 to N-1: <count>
- Claims tagged `[source: assumption]`: <count>   ·   Items still `[status: unconfirmed]`: <count>
```

Counting rules, so that two runs are comparable:

- **N** is the number of sections you actually included, after omissions.
- **Source tags** counts `[source: ...]` occurrences in the numbered sections **excluding** the final Evidence & Sources section, which restates them by design.
- Count `[source: assumption]` and `[status: unconfirmed]` **separately**; they mean different things.

**If Stage 1.2 ran and the source-tag count is 0**, the document is incomplete — research was done and has not been carried through. Restore it before continuing. If Stage 1.2 was skipped, a low or zero count is expected and correct; report it and move on rather than manufacturing citations.

Then stop and tell the user: *"Content Document ready. Say **continue** to generate the Summary Presentation."*

---

### Stage 1.8b — Summary Presentation (HTML)

- **Mode**: `agentic`
- **Input**: **The Content Document from Stage 1.8a** — this is the only source of substantive content
- **Output**: A titled HTML artifact — **"`<ProjectName>` — Summary Presentation"**
- **Condition**: ALWAYS

**What to do:**

**Step 1 — re-read the Content Document and index it.** Before writing any HTML, output the section list of the Stage 1.8a Content Document, copying each `##` heading verbatim. This is not ceremony: it is how you load the document you are rendering, and the 1.8b gate is checked against it.

**Step 2 — load the `html_design` skill** for theme tokens and design guidelines. If it is unavailable, proceed with plain semantic HTML and your own inline CSS, and say that you did.

**Step 3 — render the Content Document as a multi-slide HTML presentation.** Every section in the list from Step 1 must appear as at least one slide. Sections may share a slide, and a long section may span several, but none may be dropped.

Design guidelines:

- Clean, professional slide-style layout — roughly one section per "slide"
- Include navigation (prev/next, or scroll-based with section indicators). Inline JavaScript is permitted for navigation and for rendering diagrams
- Use data visualisations where they carry the content better than prose (e.g. feasibility radar, scope MoSCoW chart)
- Present tables as tables, with values identical to the Content Document
- **Render the *Screens & Flows* section visually**: turn each described flow into a Mermaid diagram (via the Mermaid.js CDN, rendered inside the deck) and each described screen into a phone-frame wireframe, 375px wide. This is a *visual rendering of content the Content Document already describes in words*, which is exactly what this stage is for — it is not new content. If a screen is not described there, it does not belong in the deck
- Self-contained: inline CSS, inline JS, no external dependencies except CDN libraries
- Sources must appear somewhere — a compact per-slide `Sources: …` line or a dedicated Sources slide both work

**Step 4 — condense wording, not substance.** A slide may shorten a sentence. It may not drop a number, a named competitor, a risk, a decision, a role, a success criterion or a feasibility dimension the Content Document contains. When a section is too dense for one slide, use two slides rather than cutting content.

**Step 5 — add nothing substantive.** If you find yourself introducing a fact, figure or decision absent from the Content Document, stop and either drop it or go back to Stage 1.8a (see *Revising after the fact* below).

**Stage 1.8b gate.** After the deck, output the following as normal Markdown, not inside a code fence.

The third column is the part that matters. For each section, **quote a distinctive verbatim value that appears in both the Content Document and the deck** — a number, a target, a proper noun, a date. Copy it character-for-character. Writing "yes" or "no" here is not a check; a quoted value the user can search for in both files is.

```
## Stage 1.8b coverage check
| # | Content Document section | Slide(s) | Verbatim value carried into the slide |
|---|---|---|---|
| 1 | Overview | 1 | "<exact string>" |
| 2 | Problem Statement | 2 | "<exact string>" |
| … | … | … | … |

- Sections in the Content Document: <N>   ·   Sections with at least one slide: <M>
- Sections where no verbatim value could be quoted: <list, or "none">
- Substantive content in the deck that is NOT in the Content Document: <list, or "none">
```

How to fail this gate honestly:

- **If M < N**, the deck is incomplete. Add the missing slides and redo the table.
- **If a section has no quotable value**, that section was paraphrased away rather than rendered. Put a real value back into the slide.
- For a section that is only a heading plus `Not assessed in this run`, quote that line — it is the content, and the slide must carry it rather than silently dropping the gap.

**Revising after the fact.** If Step 5 or the gate forces a change to the Content Document, do not patch it in place. Re-issue the **complete** Content Document titled *"`<ProjectName>` — Content Document (v2)"*, re-run the Stage 1.8a check on it, then redo this gate against v2. Tell the user plainly that **v2 is the version to submit** and the earlier download should be discarded. Two versions in the conversation with no stated winner is how the wrong file gets submitted.

---

### Submission checklist

Present this to the user once 1.8b passes:

```
## Ready to submit
- [ ] Content Document — downloaded as Markdown (the latest version, if it was revised)
- [ ] Summary Presentation — downloaded as HTML
- [ ] Every Content Document section has at least one slide (1.8b gate showed M = N)
- [ ] Every section carried a quoted verbatim value into the deck
- [ ] Sources are present in both
```

Remind the user: **both documents are submitted.** The Markdown is the record of the content; the HTML is the presentation of that same content. A deck missing content the Markdown contains, or a Markdown missing content the deck shows, means the pair disagrees about what the project is.

If the user has only one of the two, Stage 1.8 is not finished — return to the missing stage and produce it.

---

## Verification Gate: Ideation → Inception

After Step 7 approval, run a quick traceability check:

- All ALWAYS stages produced an artifact
- Skipped stages have documented reasons
- No orphaned assumptions (all flagged assumptions have been addressed or explicitly accepted)
- Intent backlog items trace back to the intent statement
- Decision log is complete

Report any gaps to the user before handoff.

## Session Management

- If the user stops mid-workflow, note the current stage in the conversation
- The user can resume by saying "continue ideation"
- The user can jump to any stage with "go to stage 1.4"
- The user can re-run a stage with "redo stage 1.2"
- If the session is lost, the Carry-Forward Blocks are the recovery point — ask the user to paste back the blocks they still have, and resume from the last one

## Tips for Best Results

- Provide as much context as possible when typing your idea — the more detail, the richer the artifacts
- Think about who specifically would benefit from your solution and how their lives would change
- For customer-facing projects, mention the customer segment and their current pain points
- For brownfield projects, mention the existing system/codebase
- The skill works best when you iterate — don't expect perfect artifacts on the first pass
- Challenge yourself: the best ideas often come from unexpected angles on familiar problems
- Download each artifact as it is produced. There is no file system — if the session is lost, an undownloaded artifact is gone

---

## Changelog

### V7 — content-first submission artifacts, no file system assumed

1. **Removed every file-system instruction.** V6 contained 24 references to `artifacts/ideation/…` paths, `Save to`, and existence checks. The web version of the host chat has no file system, so those instructions could not be executed and their verification steps could only be asserted, never performed. Artifacts are now titled documents in the conversation, which the user downloads.
2. **Added Carry-Forward Blocks.** With no files to re-read, later stages were relying on recall of earlier prose. Each stage now ends with a compact block of the facts, numbers and sources later stages need — including a standing instruction to carry *every* feasibility dimension, role and success criterion rather than a summary of them.
3. **Inverted Stage 1.8 and split it in two.** V6 generated the HTML first and specified the Markdown as a transcription of it. In practice that produced two independently written documents that each contained content the other lacked. Markdown is now produced first from the Carry-Forward Blocks (1.8a), and the HTML is rendered from that Markdown (1.8b), in a separate turn.
4. **Replaced the self-reported equivalence checklist with countable gates.** V6 asked the model to tick "Content equivalence confirmed", which it could assert without comparing anything. 1.8a now reports its section list and source-tag count; 1.8b reports a section-to-slide coverage table with an explicit `M = N` test and a declaration of any content the deck introduced on its own.
5. **Made source tags survive to the end.** Citations were mandated in stages 1.1–1.7 but nothing required them in the submitted artifacts, and they were being lost. The Content Document now has a required **Evidence & Sources** section, 1.8a fails on a zero source count, and a "no competitor exists" claim must be tagged as an assumption unless Stage 1.2 established it.
6. **Removed the emphasis escalation in Stage 1.8.** V6 added `MUST`, `MANDATORY`, `Hard Gate`, `KNOWN FAILURE MODE` and repeated ⚠️ warnings to stop the Markdown being dropped. The cause was output budget and dependency direction, not comprehension, so the structural fix in (3) replaces the shouting.
