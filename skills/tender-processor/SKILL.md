---
name: tender-processor
description: "End-to-end processing of social media tender briefs (RFPs/pitches/výběrová řízení). Orchestrates research, competitive analysis, and generates 3 branded deliverables: debrief questions DOCX, internal brief DOCX, and pitch presentation PPTX. Use this skill whenever the user shares a tender brief PDF, mentions a pitch/výběrové řízení/nabídka they want to win, or asks to prepare materials for a new business opportunity. Also triggers when the user says 'zpracuj tender', 'připrav nabídku', 'nové výběrko', or similar."
---

# Tender Processor

An orchestration skill that takes a tender brief PDF and produces three branded deliverables through a structured research-first workflow. This skill coordinates multiple sub-skills and parallel research agents to maximize quality and speed.

## Why this workflow exists

Winning pitches requires deep research before any creative work begins. Agencies that jump straight to "pretty slides" lose to those who demonstrate they actually understand the client's situation, competitors, and market. This workflow enforces that discipline: research first, then insights, then deliverables.

## Prerequisites (other skills this depends on)

| Skill | Purpose | Required |
|-------|---------|----------|
| `story-tlrs-docx` | Branded DOCX generation (brand.js library) | Yes |
| `story-tlrs-offer` | Branded PPTX presentation template | Yes |
| `docx` | Generic DOCX utilities (validation, reading) | Yes |
| `pptx` | Generic PPTX utilities (XML editing, validation) | Yes |
| `clickup-manager` | Task creation and tracking | Yes |
| `prospect-prep` | Research methodology and social audit guide | Recommended |

## Workflow Overview

```
┌─────────────────────────────────────────────────────────┐
│  INPUT: Tender Brief PDF                                │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  STEP 1: Analyze tender brief                           │
│  ├── Extract requirements, scope, deadlines             │
│  └── Create compliance checklist                        │
│                                                         │
│  STEP 2: Parallel research (5 agents)                   │
│  ├── Agent 1: Client social media profiles              │
│  ├── Agent 2: Deep-dive primary platform (IG/TikTok)    │
│  ├── Agent 3: Competitor social media analysis           │
│  ├── Agent 4: Internet + Reddit mentions                 │
│  └── Agent 5: Paid ads library research                 │
│                                                         │
│  STEP 3: Compile research → structured data             │
│  ├── Save to memory/prospects/                          │
│  └── Identify 3 key insights                            │
│                                                         │
│  STEP 4: Generate deliverables (can be parallel)        │
│  ├── Deliverable 1: Debrief Questions DOCX              │
│  ├── Deliverable 2: Internal Brief DOCX                 │
│  └── Deliverable 3: Pitch Presentation PPTX             │
│                                                         │
│  STEP 5: Compliance check + ClickUp task                │
│  ├── Verify all tender requirements are addressed        │
│  └── Create backlog task for gaps                       │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

## Step 1: Analyze the Tender Brief

Read the tender PDF using the `pdf` skill or Read tool. Extract and organize:

1. **Client info**: Company name, brand, product/service, market position
2. **Scope of work**: What services are being tendered (content, community, influencer, paid, strategy)
3. **Requirements**: Mandatory deliverables, format requirements, evaluation criteria
4. **Timeline**: Submission deadline, contract period, key dates
5. **Budget indicators**: Any budget ranges or constraints mentioned
6. **Evaluation criteria**: How the pitch will be scored

Save this as a structured checklist. You will use it in Step 5 to verify completeness.

**Output:** Mental model of requirements + compliance checklist (kept in context, not saved to file yet)

## Step 2: Parallel Research

This is the most critical step. Use the `dispatching-parallel-agents` pattern to launch 5 research agents simultaneously. Each agent uses Apify tools for data collection.

Read `references/research-agents.md` for the exact agent prompts and Apify tool configurations.

**Key principle:** Every research agent must save its findings to a structured format. Raw data is useless without interpretation. Each agent should conclude with "Key findings" and "Strategic implications" sections.

### Agent Dispatch Template

Launch all 5 agents in a single message using the Agent tool:

1. **Profile Scanner** — Scrapes client's social media profiles across all platforms (Instagram, TikTok, Facebook, LinkedIn, YouTube, X/Twitter). Uses `apify-slash-rag-web-browser` and platform-specific scrapers.

2. **Deep Platform Analyst** — Deep-dives into the client's primary platform (usually Instagram). Scrapes last 50-100 posts, analyzes content types, engagement rates, posting frequency, audience sentiment from comments. Uses `call-actor` with Instagram scrapers.

3. **Competitor Mapper** — Identifies and analyzes 5-10 competitors on social media. Compares follower counts, content strategies, posting frequency, engagement. Uses web search + platform scrapers.

4. **Media & Community Scanner** — Searches for client mentions across web, news, Reddit, forums. Evaluates brand awareness and sentiment. Uses `apify-slash-rag-web-browser` for web search and Reddit-specific scrapers.

5. **Paid Ads Researcher** — Checks Meta Ad Library and any available ad transparency tools for client and competitor advertising activity. Uses `apify-slash-rag-web-browser`.

### Research Data Storage

Save compiled research to two files:
- `memory/prospects/YYYY-MM-DD-{client}-research.md` — Full research findings
- `memory/prospects/YYYY-MM-DD-{client}-competitor-analysis.md` — Competitive landscape

Follow the structure in `references/research-output-template.md`.

## Step 3: Synthesize Insights

After all research agents complete, synthesize findings into exactly 3 strategic insights. An insight is not a data point — it's a tension, contradiction, or opportunity that the data reveals.

**Good insight formula:** `[Surprising fact] + [Why it matters] + [What to do about it]`

Example: "Influencer content generates 100-300x more engagement than brand content, yet the client posts 90% brand content. Flip the ratio."

For each insight, develop:
1. One **creative concept** (a named idea that addresses the insight)
2. Three **sample social media posts** per concept (with format, copy, creative description, video script if applicable)
3. **Influencer collaboration options** (3 total across all insights, with specific profile recommendations)

Read `references/insight-framework.md` for the detailed structure.

## Step 4: Generate Deliverables

Generate all three documents. These can be done in parallel using sub-agents if the research is complete.

### Deliverable 1: Debrief Questions DOCX

**Purpose:** A questionnaire to send to the client to fill gaps in the brief.

**Skill to invoke:** `story-tlrs-docx`

**Structure:** Read `references/debrief-template.md` for the section structure. Typical sections:
1. Brand & Positioning
2. Target Audience
3. Content & Social Media
4. Paid Media & Budget
5. Influencer Marketing
6. Community Management
7. Logistics & Process
8. Measurement & Reporting
9. Competitive & Market Context

**Generation approach:**
- Use the `brand.js` library from `story-tlrs-docx` skill
- Components: `packageHeader`, `sectionLabel`, `mixedHeadline`, `bulletItem`, `bodyText`, `infoBox`, `spacer`, `pageBreak`
- Include Story TLRS header/footer
- Questions should be informed by the research (not generic)
- Target: 35-50 questions across all sections

**Output:** `{Client}-Debrief-Otazky.docx` in Claude Folder

### Deliverable 2: Internal Brief DOCX

**Purpose:** Internal strategy document for the pitch team with all research findings and strategic recommendations.

**Skill to invoke:** `story-tlrs-docx`

**Structure:** Read `references/brief-template.md` for the full template. Core sections:
1. About the Client (background, USP, positioning)
2. Project Goals (objectives from the brief + our interpretation)
3. Target Audience (segments, personas from research)
4. Scope of Work (what we're pitching for)
5. Client Social Media Analysis (current state per platform)
6. Competitor Analysis (comparative table + key findings)
7. Paid Media Analysis (ad spending, formats, targeting)
8. Media & Community Coverage (mentions, sentiment, PR)
9. SWOT Analysis (based on all research)
10. Strategic Recommendations (our proposed approach with benchmarks)

**Output:** `{Client}-Interni-Brief.docx` in Claude Folder

### Deliverable 3: Pitch Presentation PPTX

**Purpose:** Client-facing offer presentation.

**Skill to invoke:** `story-tlrs-offer`

**Structure:** Follow the 4-chapter Story TLRS offer template:
- **Chapter 1 — THE LANDSCAPE:** Current situation analysis, key data points, competitive positioning
- **Chapter 2 — {CONCEPT NAME}:** Insights, creative concepts, sample posts, influencer strategy
- **Chapter 3 — THE INVESTMENT:** Pricing (may need Excel template or manual completion)
- **Chapter 4 — WHAT COMES NEXT:** Implementation timeline, next steps after approval

**Key rules from story-tlrs-offer:**
- Fonts: Red Hat Display, Young Serif, Manrope
- Colors: Black, Blue (#33B6FF), Green (#57DC64), Purple, Orange
- Fixed slide size: 9144000 x 5143500 EMU
- XML-based manipulation of template PPTX

**Output:** `{Client}-Nabidkova-Prezentace.pptx` in Claude Folder

## Step 5: Compliance Check + ClickUp Task

### Compliance Verification

Compare the three deliverables against the checklist from Step 1:

1. Are all mandatory sections from the tender brief addressed?
2. Does the presentation cover all evaluation criteria?
3. Are there any explicit requirements we haven't met?
4. Is the pricing section complete or does it need manual input?
5. Are case studies or references required but missing?

### ClickUp Task

Create a task in your default Backlog list (configure the list ID in the `clickup-manager` skill) with:
- **Title:** `[Client] - Tender gaps & next steps`
- **Description:** List all identified gaps, missing items, and manual actions needed
- **Priority:** `high`
- **Due date:** 2 days before submission deadline (or 1 week from now if no deadline)
- **Tags:** `tender`, `new-business`

## Czech Language & Diacritics

All documents are generated in Czech. This is a known pain point — generation pipelines sometimes drop háčky and čárky (producing "Cesky" instead of "Český").

**Prevention:**
- Include explicit Czech text in the generation scripts (not transliterated)
- When using XML manipulation for PPTX, scope regex replacements to `<a:t>` tags only
- After generation, validate Czech characters in output files
- If diacritics are broken, run a targeted fix pass (replace within text nodes only)

**Writing style for Czech content:**
- Must sound natively Czech, not translated from English
- Use hovorové tvary where appropriate: "fakt", "můžou", "výběrko"
- Czech quotes: „ " not " "
- Prefer Czech terminology: "výběrové řízení" not "pitch", "obor" not "průmysl"

## Timing Expectations

| Step | Expected Duration |
|------|-------------------|
| Step 1: Brief analysis | 2-3 minutes |
| Step 2: Parallel research | 5-10 minutes (parallel) |
| Step 3: Insight synthesis | 3-5 minutes |
| Step 4: Document generation | 5-10 minutes (can parallelize) |
| Step 5: Compliance + ClickUp | 2-3 minutes |
| **Total** | **~20-30 minutes** |

## Error Handling

- **Apify scraper fails:** Retry once. If still failing, use `apify-slash-rag-web-browser` as fallback for that data source. Note the gap in research.
- **DOCX generation fails:** Check npm install, verify brand.js is accessible, check for `\n` in TextRun (common bug — use separate Paragraph elements instead).
- **PPTX generation fails:** Verify template file exists, check XML structure integrity, validate Content_Types.xml and _rels after adding slides.
- **ClickUp task creation fails:** Retry once with verified list ID. If still failing, output the task details for manual creation.
- **Czech diacritics broken:** Run targeted replacement pass scoped to text content nodes only.
