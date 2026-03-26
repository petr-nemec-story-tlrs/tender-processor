---
name: story-tlrs-offer
description: >-
  Generate Story TLRS offer/nabídka presentations in the exact brand layout and template.
  Use this skill ALWAYS when Petr wants to create, build, or generate an offer presentation
  for a client. Trigger on: vytvoř nabídku, udělej prezentaci, nová nabídka, offer pro klienta,
  create offer, new offer presentation, make a deck for, udělej deck, nabídka pro, připrav
  prezentaci. Also trigger whenever a client name + service scope is provided as a brief for
  a new presentation.
---

# Story TLRS Offer Skill

Generate client offer presentations using the Story TLRS brand template. This skill handles the full PPTX workflow — unpacking the template, writing new content into the correct XML structure, and repacking — while preserving all fonts, colors, and layout rules exactly.

## Quick Start

When triggered, gather the brief (or ask for it if missing):

```
- Client name (for title slide)
- Service bubbles on cover (which of: discovery, brand, strategy — or custom)
- Language: CZ or EN
- Chapter names + content outline (or raw notes to structure)
- Pricing per service (one price box per chapter)
```

Then execute the full workflow below.

---

## Required Reference Files

Before starting, read these files:

1. **`references/design-rules.md`** — Full font specs, colors, spacing values, EMU coordinates. Read once at the start.
2. **`references/slide-catalog.md`** — XML patterns for every slide type. Reference throughout as you write each slide.

---

## Workflow

### Step 1 — Set Up Workspace

Před spuštěním příkazů urči správné cesty dynamicky — nepoužívej hardcoded session jméno:

- **SKILL_DIR**: Adresář, ze kterého jsi četl tento SKILL.md (např. `.../mnt/.skills/skills/story-tlrs-offer`)
- **TEMPLATE**: `$SKILL_DIR/assets/Template- Story TLRS offer.pptx`
- **CLAUDE_FOLDER**: O tři úrovně výš od SKILL_DIR, pak do `Claude Folder` — tedy `$SKILL_DIR/../../../Claude Folder`
- **WORK**: `/tmp/offer-work` (dočasný prostor, vždy dostupný)

```bash
# Doplň SKILL_DIR skutečnou cestou, ze které jsi četl tento soubor
SKILL_DIR="[doplň skutečnou cestu ke skill adresáři]"
TEMPLATE="$SKILL_DIR/assets/Template- Story TLRS offer.pptx"
CLAUDE_FOLDER="$SKILL_DIR/../../../Claude Folder"
WORK="/tmp/offer-work"
mkdir -p "$WORK/extracted"
cp "$TEMPLATE" "$WORK/offer.pptx"
cd "$WORK"
unzip -q offer.pptx -d extracted
```

The template has 12 slides. You will build the new offer by **replacing all slides** with fresh content. Do not keep any template slide's content text — only reuse the XML structure and formatting.

### Step 2 — Plan the Slide Deck

Fixed offer structure:
1. **Cover** (always slide 1) — title slide with client name + service bubbles
2. **TOC/Prolog** (always slide 2) — numbered chapter list
3. For each chapter: **Chapter Divider** + content slides (2–5 per chapter)

Always follow this chapter order:
- Chapter 1: Analysis & Gap Identification (market context, current state, gaps)
- Chapter 2: Proposed Solution (services, approach, deliverables)
- Chapter 3: Investment/Pricing (pricing slides — one bordered box per service)
- Chapter 4: Next Steps (what comes after)

Chapter names are always in ENGLISH (all caps on chapter divider slides).

Petr will manually append "Stories We Told" and "(Y)our Protagonists" sections — never generate those.

### Mandatory Slide Type Planning Table

**Before writing any XML, produce a complete slide plan in this format:**

| # | Type | Chapter | Content summary |
|---|------|---------|-----------------|
| 1 | Cover | — | Client name + service bubbles |
| 2 | TOC/Prolog | — | Chapter list |
| 3 | Chapter Divider | Ch1 | Chapter title |
| 4 | [type] | Ch1 | [content] |
| … | | | |

**Then validate against these rules — DO NOT proceed to Step 3 until all pass:**

☐ **Max 3 consecutive Text-Heavy slides.** Count consecutive Text-Heavy runs. Any run > 3 = insert a visual break type before continuing.

☐ **Data/numbers → Stats slide, not bullet text.** If any slide lists 3+ metrics in body text, convert to Stats (3-column pills or 2×2 grid).

☐ **Competitor comparison → Table/Comparison Grid.** If a slide describes competitors in prose, convert to Table.

☐ **3 parallel process steps/tips → Tips/Callout Box Row.** If a slide lists exactly 3 parallel items, convert to Tips row.

☐ **Strong single claim → Big Quote or Pull Quote.** If a slide exists only to make one statement, convert to Quote type.

☐ **Section transitions.** After 2+ dense content slides, consider a Colored Section Divider or Subchapter Divider to reset visual rhythm.

☐ **Implementation phases → Timeline/Gantt Strip.** If a slide describes a timeline in prose, convert.

Only after all checkboxes pass: proceed to Step 3 (Write Slide XMLs).

### Step 3 — Write Slide XMLs

For each slide, modify or create the XML based on the slide type. Use patterns from `references/slide-catalog.md`.

**CRITICAL rules:**
- Never change font families — all fonts are embedded in the template (`Red Hat Display`, `Young Serif`, `Manrope`)
- Never change slide dimensions (9144000 × 5143500 EMU)
- Chapter labels in top-left must increment: `CHAPTER 01`, `CHAPTER 02`, etc.
- Content language follows the brief (CZ or EN) — chapter labels/subtitles always ENGLISH
- Blue highlight (`#33B6FF`) for key insights and price tags
- Green highlight (`#57DC64`) for section labels and next-step service names
- Cover bubbles: keep 3 bubbles, update text to match services in this offer

**Slide types and when to use them:**

| Slide Type | When to use |
|------------|-------------|
| Cover | Slide 1 only |
| TOC/Prolog | Slide 2 only |
| Chapter Divider | First slide of every chapter (dark photo background) |
| Text-Heavy (38pt) | Main argument slides, context, strategy content (most slides) |
| Detailed Service | When describing a specific service with process + deliverables |
| Investment/Pricing | One per priced service in Chapter 3 |
| What's Next | Last content slide of Chapter 4 |
| **Colored Section Divider** | Major section separator — 4 color variants: blue (strategy/data), green (results/growth), orange (creative/campaigns), purple (innovation/concepts) |
| **Subchapter Divider — Dark** | Concept reveals, sub-chapter intros inside a chapter, single-page section statements — dark bg with photo + keyword tags |
| **Big Quote — White bg** | Client testimonial, memorable statement, single powerful quote — 3 accent color variants (purple, orange, green highlight) |
| **Pull Quote — Inline (25pt)** | Secondary quote embedded in content flow, fits alongside other text |
| **Big Quote — Dark/Full Bleed** | Most powerful quote moments — full-bleed black bg with large white Young Serif text |
| **Full-Bleed Image** | Visual pause, atmosphere slide, product/campaign imagery — no text or minimal caption |
| **Two-Photo Side-by-Side** | Before/after comparisons, dual campaign examples, two personas |
| **Half-Image + Content** | Service description with visual support — image takes left or right half, content the other |
| **Stats — 3-Column (Pills)** | 3 key metrics with colored pill backgrounds — highlight platform performance, audience size, engagement |
| **Stats — 2×2 Grid** | 4 key metrics in a grid — overview of broader performance data |
| **Tag Cloud / Keyword Grid** | Content pillars, topic clusters, keyword mapping, brand attributes |
| **Tips / Callout Box Row** | 3 practical tips or process steps in a row — gray rounded boxes |
| **Influencer / People Slide** | Team intro, 3 profiles with photo + stats — influencer campaign roster, key stakeholders |
| **Timeline / Gantt Strip** | Project schedule, campaign phases, onboarding timeline |
| **Table / Comparison Grid** | Feature comparison, service tier breakdown, competitor analysis |

**Slide selection strategy — beyond the default:**

Text-Heavy (38pt) is the default for argument slides, but a good offer varies its rhythm. Use the expanded types deliberately:

- **Data exists?** → Stats slide (3-column or 2×2) instead of text listing numbers in body copy
- **Strong claim that needs credibility?** → Big Quote or Pull Quote (testimonial, research finding, client's own words)
- **Service has a visual component?** → Half-Image + Content instead of pure text
- **Showing content pillars or campaign themes?** → Tag Cloud / Keyword Grid
- **3 steps or principles?** → Tips / Callout Box Row (cleaner than 3 paragraphs)
- **Need a strong pause between two dense sections?** → Full-Bleed Image or Colored Section Divider
- **Presenting influencer strategy?** → Influencer / People Slide with actual profiles
- **Implementation has phases?** → Timeline / Gantt Strip, not a bullet list
- **Comparing tiers or options?** → Table / Comparison Grid

**Presentation rhythm rule:** No more than 3 consecutive Text-Heavy slides without a visual break (stats, image, quote, or divider).

### Step 4 — Write `slide*.xml` Files

Write each slide's XML by editing the corresponding file under `extracted/ppt/slides/slide*.xml`. Build slides in order.

For slides beyond the template count (if deck needs more than 12 slides), copy an existing slide XML and update its content. Make sure `extracted/ppt/presentation.xml` references any new slides.

### Step 5 — Validate XML

**MANDATORY before packing.** Run XML validation on every slide file. If any file fails, fix it before proceeding.

```bash
cd "$WORK/extracted"
python3 -c "
import xml.etree.ElementTree as ET
import os, sys

errors = []
for root_dir, dirs, files in os.walk('ppt/slides'):
    for f in sorted(files):
        if f.endswith('.xml') and not root_dir.endswith('_rels'):
            path = os.path.join(root_dir, f)
            try:
                ET.parse(path)
            except ET.ParseError as e:
                print(f'ERROR: {path} — {e}')
                errors.append((path, str(e)))

for p in ['ppt/presentation.xml', '[Content_Types].xml', 'ppt/_rels/presentation.xml.rels']:
    try:
        ET.parse(p)
    except ET.ParseError as e:
        print(f'ERROR: {p} — {e}')
        errors.append((p, str(e)))

if errors:
    print(f'\n{len(errors)} XML error(s) found. FIX BEFORE PACKING.')
    sys.exit(1)
else:
    print(f'All XML files valid.')
"
```

If errors are found:
1. Read the broken file around the reported position
2. Fix the XML (common issues: missing `<` in closing tags, unclosed elements, unescaped `&` in text)
3. Re-run validation until all files pass

**Do NOT proceed to Step 6 until validation passes with zero errors.**

### Step 6 — Pack and Save

```bash
cd "$WORK/extracted"
OUTPUT_NAME="Nabidka-[ClientName]-[YYYYMMDD].pptx"
zip -r "../$OUTPUT_NAME" . -x "*.DS_Store"
cp "../$OUTPUT_NAME" "$CLAUDE_FOLDER/$OUTPUT_NAME"
```

Present the file to the user with a `present_files` call.

---

## Slide Content Rules

### Cover Slide
- Bubbles: Update text in the 3 colored circles to reflect this offer's services
  - Purple bubble (#9795F9): discovery / research / audit
  - Orange bubble (#FF8000): brand / content / creative
  - Green bubble (#57DC64): strategy / performance / growth
- Client text: "TODAY WE TELL YOU A STORY ABOUT" + CLIENT NAME (all caps)
- Background image: Keep from template (do not replace)

### TOC / Prolog
- Left headline: Keep "Today's story comes together in these chapters" structure
- Right: Numbered list of chapter names (one per chapter, in English, match chapter order)
- Auto-numbered paragraphs use `arabicPeriod` list format

### Chapter Divider
- Large headline = chapter title in English, all caps, Young Serif 51pt bold
- Keep the background image fill from template slide 3 (copy it)
- Story TLRS icon stays in top-left

### Text-Heavy Slides (most content slides)
- Left box: 2-line mixed headline (Red Hat Display Black 38pt line 1 + Young Serif bold 38pt line 2)
- Right box: Body text, Red Hat Display Medium 13pt
- Use blue highlights for key terms/insights
- Keep paragraphs tight: 2–3 paragraphs max, separated by 12pt spcBef
- One strong insight per slide — not a data dump

### Detailed Service Slides
- Smaller 20pt headline (same two-font split)
- Left bottom: grey rounded-rect info box (E9E9E9 fill) for "Why" or "Timeline"
- Right: Two sections — top with green-highlighted label, bottom with green label + bullet list
- Bullet char: ● (U+25CF), indent=-298450, marL=457200

### Investment / Pricing Slides
- One slide per service
- Large bordered roundRect (no fill, 9525pt black border) wrapping all content
- Left inside: Service name (Young Serif bold 17pt, all caps) + deliverables (Medium 12pt) + timeline (Medium 11pt)
- Right inside: Price (Red Hat Display Black 24pt, right-aligned, BLUE HIGHLIGHT #33B6FF)
- Subtitle (center header): "INVESTMENT"

### What's Next
- Young Serif bold 20pt headline (single font — no Red Hat Display Black on line 1)
- Body: Red Hat Display Medium 13pt, green highlights for next-step service names
- Use 700pt spcBef between body paragraphs (wider than standard)

---

## Content Guidelines

**Czech language notes:**
- Formal "vy" form (not informal "ty") when addressing client
- Section labels (CHAPTER 01, INVESTMENT, etc.) always stay in English

**Headline writing:**
- Line 1 (Red Hat Display Black): short, concrete — "Where is", "Why it", "Start with"
- Line 2 (Young Serif): completes the thought, slightly abstract — "the gap", "matters now.", "discovery."
- Together they form one thought, split for visual rhythm

**Body text:**
- Short paragraphs, 2–4 sentences
- One key insight per paragraph, highlighted in blue
- Avoid bullet points in text-heavy slides — use them only in Detailed Service slides

**Pricing slide:**
- Price format: `XX 000 CZK` (no comma, space as thousands separator)
- If price is TBD: write "dle rozsahu" (CZ) or "upon scope" (EN) with no blue highlight

---

## Example Brief → Deck Mapping

**Brief:** NovaBrand — consumer lifestyle brand, new market entry, services: discovery + brand + content strategy, CZ, price 60k + 90k/month

**Deck structure:**
1. Cover — bubbles: "discovery", "brand", "content strategy" | client: "NOVABRAND"
2. TOC — 4 chapters
3. Ch1 divider: "THE LANDSCAPE"
4. Slide: Market context (lifestyle category, Czech consumer trends)
5. Slide: What NovaBrand has today vs. gap
6. Ch2 divider: "YOUR STORY, TOLD RIGHT"
7. Slide: Discovery Workshop overview
8. Slide: Brand + content strategy approach
9. Slide: Social media execution plan
10. Ch3 divider: "THE INVESTMENT"
11. Pricing: Discovery Workshop — 60 000 CZK
12. Pricing: Ongoing Brand & Content — 90 000 CZK / měsíc
13. Ch4 divider: "WHAT COMES NEXT"
14. What's Next slide

---

## Error Prevention

- **Font not rendering**: Ensure font name exactly matches embedded names: `Red Hat Display`, `Young Serif` (not "YoungSerif" or "RedHatDisplay")
- **Missing slide in presentation.xml**: If you add slides, add corresponding `<p:sldId>` entries in `presentation.xml` and update `[Content_Types].xml` + `_rels/presentation.xml.rels`
- **Broken XML**: Always validate that tags are properly closed before saving
- **EMU coordinates**: Slide = 9144000 × 5143500. Header line at y=570989. Content starts y=1227425.
