# Internal Brief — Document Template

The internal brief is a comprehensive strategy document for the pitch team. It compiles all research findings and proposes a strategic approach.

## Document Structure

```
Cover Page (Story TLRS branding)
├── Title: "Interní brief"
├── Subtitle: "{Client} — Social Media Tender"
└── Date + deadline

Section 1: About the Client
Section 2: Project Goals
Section 3: Target Audience
Section 4: Scope of Work
Section 5: Client Social Media Analysis
Section 6: Competitor Analysis
Section 7: Paid Media Analysis
Section 8: Media & Community Coverage
Section 9: SWOT Analysis
Section 10: Strategic Recommendations

Total: 8-12 pages
```

## Section Details

### 1. About the Client (1 page)
Company background distilled from research. Include:
- Company name, origin, founding story
- Product/service description
- Key USP and brand positioning
- Market presence (distribution, availability)
- Investment/funding info (if public)
- Current marketing approach

**Components:** `bodyText`, `bodyHighlight`, `infoBox` for key stats

### 2. Project Goals (0.5 page)
Objectives extracted from the tender brief + our interpretation:
- Brand awareness goals
- Social media growth targets
- Content production objectives
- Message/positioning priorities

**Components:** `bulletItem` for each goal, `infoBox` for priority ranking

### 3. Target Audience (1 page)
Audience segments with descriptions. Use 2-column table format:

```javascript
B.twoColumnTable([
  ["Segment name", "Description including demographics, psychographics, behavior"],
  // ... more segments
], "Segment", "Popis", 0.25, false)
```

Include sub-segments if relevant (e.g., "Potential newcomers" vs. "Brand switchers")

### 4. Scope of Work (0.5 page)
What we're pitching for, broken into service areas:
- Strategic support (strategy, planning, reporting)
- Content production (copy, design, video)
- Community management (response, moderation)
- Influencer marketing (selection, management, campaigns)
- Paid media (campaign management, optimization)

**Components:** `bulletItem` for each area with brief description

### 5. Client Social Media Analysis (1.5-2 pages)
Current state of client's social media, per platform. This is where research from Agent 1 + Agent 2 is compiled.

For each platform with presence:
- Profile URL, follower count
- Posting frequency
- Content types and themes
- Engagement metrics
- Visual and copy style assessment
- Key strengths and weaknesses

For the primary platform (usually Instagram), include:
- Content mix breakdown (% static, video, carousel)
- Top-performing vs. worst-performing content
- Engagement rate calculation
- Comment sentiment summary
- Influencer content performance vs. brand content

**Components:** `twoColumnTable` for platform overview, `bodyHighlight` for key metrics, `infoBox` for standout findings

### 6. Competitor Analysis (1.5-2 pages)
Comparative analysis from Agent 3 research.

Include:
- Competitor comparison table (all competitors)
- Deep profiles of top 3 competitors
- Market positioning map (where each brand sits)

```javascript
// Comparison table format
B.twoColumnTable([
  ["MAGU (@magukombucha)", "67K followers | 5x/week | Aggressive lifestyle content, heavy influencer use, controversial health claims"],
  // ... more competitors
], "Brand", "Social Media přehled", 0.3, false)
```

### 7. Paid Media Analysis (0.5-1 page)
From Agent 5 research:
- Client's current ad activity (if any)
- Competitor ad spending comparison
- Common ad formats and messages in the category
- Meta Ad Library findings

**Components:** `bodyText` with `bodyHighlight` for spending levels

### 8. Media & Community Coverage (0.5 page)
From Agent 4 research:
- Number of mentions found
- Key articles/features
- Reddit/forum discussions
- Sentiment summary
- Media visibility score

**Components:** `infoBox` for visibility score, `bulletItem` for key mentions

### 9. SWOT Analysis (0.5 page)
Based on ALL research findings:

```javascript
B.twoColumnTable([
  ["Strengths", "List of strengths..."],
  ["Weaknesses", "List of weaknesses..."],
  ["Opportunities", "List of opportunities..."],
  ["Threats", "List of threats..."],
], "Kategorie", "Analýza", 0.2, false)
```

### 10. Strategic Recommendations (1-1.5 pages)
Our proposed approach, organized by pillar:

1. **Channel Strategy** — Platform priorities, launch sequence
2. **Content Strategy** — Content pillars, formats, frequency
3. **Influencer Strategy** — Tier mix, collaboration models, target profiles
4. **Paid Media Strategy** — Budget allocation, campaign types, targeting
5. **Growth Benchmarks** — 3-month and 6-month targets per platform

**Components:** `sectionLabel` for each pillar, `bulletItem` for specifics, `infoBox` for benchmarks

## Generation Notes

**Using brand.js:**
```javascript
const B = require("./brand");
const { Document, Packer } = require("docx");
const fs = require("fs");

const doc = new Document({
  numbering: B.defaultNumbering(["b1","b2","b3","b4","b5","b6","b7","b8","b9","b10"]),
  sections: [{
    properties: B.sectionProps(),
    headers: B.stdHeader("Interní brief", "{Client} — Social Media Tender"),
    footers: B.stdFooter(),
    children: [
      ...B.coverPage("INTERNÍ", "BRIEF.", "{Client} — Social Media Tender", "Datum: {date}"),
      B.pageBreak(),
      B.sectionLabel("1. About the Client"),
      ...B.mixedHeadline("O klientovi", "— {Client}."),
      B.spacer(40, 40),
      B.bodyText("Content..."),
      // ... continue with sections
    ]
  }]
});

Packer.toBuffer(doc).then(buf => fs.writeFileSync("output-brief.docx", buf));
```

**Key rules:**
- Czech content throughout (with proper diacritics)
- Each section starts with `sectionLabel` + `mixedHeadline`
- Data-heavy sections use tables
- Insights and standout findings use `infoBox`
- Strategic recommendations are action-oriented, not descriptive
