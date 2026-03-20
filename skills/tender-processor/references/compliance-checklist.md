# Compliance Checklist — Verification Guide

After generating all three deliverables, verify each one against the tender brief requirements.

## Verification Process

### Step 1: Extract requirements from tender brief

Go through the original PDF and list EVERY explicit requirement:

```markdown
## Mandatory Requirements
- [ ] {requirement_1} — covered in: {document}
- [ ] {requirement_2} — covered in: {document}
...

## Evaluation Criteria
- [ ] {criterion_1} — addressed in: {slide/section}
...

## Format Requirements
- [ ] {format_req_1} — status: {met/not met}
...

## Deliverable Requirements
- [ ] {deliverable_1} — status: {created/missing}
...
```

### Step 2: Cross-check each document

**Debrief Questions DOCX:**
- [ ] Covers all scope areas mentioned in the brief
- [ ] Questions are specific to the client (not generic)
- [ ] Research findings are reflected in questions
- [ ] Story TLRS branding is correct (header, footer, fonts, colors)
- [ ] Czech language with proper diacritics
- [ ] File opens correctly in Word

**Internal Brief DOCX:**
- [ ] All 10 sections are present
- [ ] Research data is accurately cited
- [ ] Competitor analysis includes all major players
- [ ] SWOT is based on actual research (not generic)
- [ ] Strategic recommendations are specific and actionable
- [ ] Growth benchmarks are realistic
- [ ] Story TLRS branding is correct
- [ ] Czech language with proper diacritics
- [ ] File opens correctly in Word

**Pitch Presentation PPTX:**
- [ ] Follows 4-chapter Story TLRS offer template
- [ ] Chapter 1 (THE LANDSCAPE) has data-backed analysis
- [ ] Chapter 2 has insights, concepts, sample posts, influencer strategy
- [ ] Chapter 3 (THE INVESTMENT) has pricing structure (or placeholder)
- [ ] Chapter 4 (WHAT COMES NEXT) has clear next steps
- [ ] All tender evaluation criteria are addressed
- [ ] Correct fonts (Red Hat Display, Young Serif, Manrope)
- [ ] Correct colors (Black, Blue #33B6FF, Green #57DC64)
- [ ] Czech language with proper diacritics
- [ ] File opens correctly in PowerPoint

### Step 3: Identify gaps

Common gaps to check for:
1. **Pricing** — Often requires Excel template or manual input
2. **Case studies** — May need to be selected from Story TLRS portfolio
3. **Team bios** — Specific team members assigned to the account
4. **Technical specifications** — Platform-specific requirements
5. **Legal/contractual** — Terms, NDA, contract templates
6. **Timeline** — Detailed implementation timeline
7. **References** — Client testimonials or references

### Step 4: Create ClickUp task

Use `clickup-manager` skill to create a task:

```
List: your default Backlog list (configure in clickup-manager skill)
Title: [{Client}] Tender — gaps & next steps
Priority: high
Due date: {submission_deadline - 2 days}
Tags: tender, new-business
Description:
## Splněné požadavky
{list of met requirements}

## Chybějící položky
{list of gaps with specific actions needed}

## Akce potřebné před odevzdáním
- [ ] {action_1} — odpovědná osoba: {who}
- [ ] {action_2} — odpovědná osoba: {who}
...
```

Also add a comment summarizing what was generated and where files are saved.
