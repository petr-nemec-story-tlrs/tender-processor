# Debrief Questions — Document Template

The debrief questionnaire is sent to the prospective client to fill gaps in the tender brief. Questions should be informed by research findings, not generic.

## Document Structure

```
Cover Page (Story TLRS branding)
├── Title: "Debrief otázky"
├── Subtitle: "{Client} — Tender"
└── Date

Section 1: Brand & Positioning (4-6 questions)
Section 2: Cílová skupina (4-6 questions)
Section 3: Content & Social Media (5-8 questions)
Section 4: Paid Media & Rozpočet (4-6 questions)
Section 5: Influencer Marketing (4-5 questions)
Section 6: Community Management (3-5 questions)
Section 7: Logistika & Proces (4-6 questions)
Section 8: Měření & Reporting (3-5 questions)
Section 9: Konkurence & Trh (3-5 questions)

Total: 35-50 questions
```

## Section Details

### 1. Brand & Positioning
Questions about brand identity, values, tone of voice, positioning vs. competitors.
- Brand guidelines availability
- Key messages and claims
- Restrictions (what NOT to communicate)
- Approval workflow for brand content

**Research-informed examples:**
- "V briefu zmiňujete claim '{claim}'. Jak přesně chcete tento claim komunikovat na sociálních sítích?"
- "Na základě naší analýzy vidíme, že {finding}. Je to záměr, nebo chcete změnit směr?"

### 2. Cílová skupina (Target Audience)
Questions about demographics, psychographics, existing customer data.
- Primary vs. secondary audience
- Customer personas (if they exist)
- Purchase behavior data
- Audience overlap across platforms

### 3. Content & Social Media
Questions about content expectations, platform priorities, existing assets.
- Platform priority ranking
- Content volume expectations (posts per week/month)
- Existing content assets (photo bank, video footage)
- Content approval process and turnaround time
- Hashtag strategy preferences
- UGC policy

### 4. Paid Media & Rozpočet
Questions about budget allocation, campaign objectives, past performance.
- Total monthly/annual budget for social
- Budget split: organic vs. paid
- Campaign objectives (awareness, traffic, conversions)
- Past campaign results (if available)
- Performance benchmarks they care about

### 5. Influencer Marketing
Questions about influencer experience, preferences, restrictions.
- Past influencer collaborations
- Preferred influencer tiers (micro, mid, macro)
- Category preferences/restrictions
- Long-term ambassador vs. one-off preference
- Influencer content usage rights

### 6. Community Management
Questions about community management scope and expectations.
- Response time expectations
- Languages supported
- Crisis management protocol
- Tone for responses (formal/informal)
- Escalation process

### 7. Logistika & Proces
Questions about practical workflow details.
- Primary contact person(s)
- Approval chain and timeline
- Meeting cadence preference
- Reporting frequency
- Tool preferences (PM tools, asset sharing)
- Contract period and notice terms

### 8. Měření & Reporting
Questions about KPIs and success metrics.
- Primary KPIs they track
- Reporting format preferences
- Access to analytics tools
- Benchmark expectations (growth targets)

### 9. Konkurence & Trh
Questions about how they see their competitive landscape.
- Who they consider their main competitors
- What competitor content they admire
- Market trends they want to leverage
- Regional vs. international ambitions

## Generation Notes

**Using brand.js components:**
```javascript
const B = require("./brand");

// Section pattern:
B.pageBreak()
B.sectionLabel("1. Brand & Positioning")
...B.mixedHeadline("Brand", "& positioning.")
B.spacer(40, 40)
B.bodyText("Introductory context for this section...")
B.bulletItem("Question text here?", "b1")  // b1 = numbering reference
B.bulletItem("Follow-up question?", "b1")
```

**Key rules:**
- Each section gets its own numbering reference (b1, b2, b3...)
- Use `bodyText` for section intro, `bulletItem` for questions
- Add `infoBox` when a research finding informs the question
- Include `spacer(40, 40)` between section intro and questions
- Never use `\n` in TextRun — use separate Paragraph elements
