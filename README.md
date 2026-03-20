# Tender Processor Plugin

End-to-end processing of social media tender briefs (RFPs / pitches / výběrová řízení). Takes a tender brief PDF and produces three branded Story TLRS deliverables through a structured research-first workflow.

## What it does

1. **Analyzes the tender PDF** — extracts requirements, scope, evaluation criteria, deadlines
2. **Dispatches 5 parallel research agents** — client social profiles, deep platform analysis, competitor mapping, media mentions, paid ads research (via Apify)
3. **Synthesizes 3 strategic insights** — each with a creative concept and sample posts
4. **Generates 3 branded deliverables:**
   - `{Client}-Debrief-Otazky.docx` — research-informed questions for the client (35-50)
   - `{Client}-Interni-Brief.docx` — internal strategy document for the pitch team (8-12 pages)
   - `{Client}-Nabidkova-Prezentace.pptx` — client-facing offer presentation
5. **Runs compliance check** — verifies all tender requirements are addressed
6. **Creates a ClickUp task** — flags any gaps for manual follow-up

Total time: ~20-30 minutes per tender.

## Bundled skills

| Skill | Purpose |
|-------|---------|
| `tender-processor` | Main orchestration workflow |
| `story-tlrs-docx` | Story TLRS branded Word documents (includes brand.js) |
| `story-tlrs-offer` | Story TLRS branded PPTX template (includes Template PPTX) |
| `docx` | Generic DOCX utilities — validation, XML editing, tracked changes |
| `pptx` | Generic PPTX utilities — editing, QA, image conversion |
| `clickup-manager` | ClickUp task creation and management |
| `prospect-prep` | Research methodology and social audit guide |

## Required MCP connectors

Connect these before using the plugin:

| Connector | Purpose | Required |
|-----------|---------|----------|
| **Apify** | Social media scraping, web research, ad library | Yes |
| **ClickUp** | Task creation for gaps and follow-up | Yes |

## Usage

Share a tender brief PDF with Claude and say:

- `"Zpracuj tento tender"` — Czech trigger
- `"Připrav nabídku pro tohle výběrko"` — Czech trigger
- `"Process this RFP and prepare pitch materials"` — English trigger
- `"Mám nové výběrové řízení"` — Czech trigger

Claude will guide you through the full workflow automatically.

## Installation

### Via Claude Code / Cowork

```bash
claude plugin install tender-processor.plugin
```

Or install from GitHub:

```bash
claude plugin install https://github.com/petrnemec/tender-processor
```

### Manual

Copy the plugin directory to your Claude plugins folder and restart.

## Prerequisites (system)

- `npm` — for docx generation (`npm install docx`)
- `python3` — for PPTX/DOCX manipulation and validation
- `LibreOffice` — for PDF/image conversion (auto-configured in Claude sandboxed environments)
- `pandoc` — for DOCX text extraction

## Customization

### Different agency brand

Replace `story-tlrs-docx` and `story-tlrs-offer` with your own branded skills. Update:
1. `skills/story-tlrs-docx/assets/brand.js` — your color tokens and component library
2. `skills/story-tlrs-offer/assets/` — your PPTX template
3. `skills/tender-processor/SKILL.md` — update skill references and chapter structure

### Different industry

The research agents are configured for social media tenders. To adapt:
1. Edit `skills/tender-processor/references/research-agents.md` — change platforms and data sources
2. Edit `skills/tender-processor/references/debrief-template.md` — industry-relevant questions
3. Edit `skills/tender-processor/references/brief-template.md` — relevant analysis sections

## ClickUp configuration

Before using, configure the following in `skills/clickup-manager/SKILL.md`:
- Your default **Backlog list ID** (replace the `*(doplň své ID)*` placeholders)
- Your **ClickUp workspace ID** and space/list structure

The prospect-prep skill creates tasks in the **Meeting Preps list** (`901522289076`) — update this in `skills/prospect-prep/SKILL.md` if needed.

## License

MIT
