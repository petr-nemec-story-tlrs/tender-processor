# Story TLRS Offer — Design Rules

Complete reference for fonts, colors, spacing, and positioning in the brand template.

---

## Slide Dimensions

- Width: 9144000 EMU (= 24 cm / ~9.45 in)
- Height: 5143500 EMU (= 13.59 cm / ~5.35 in)
- All coordinates below are in EMU

---

## Color Palette

| Name | Hex | Usage |
|------|-----|-------|
| Black | `000000` | Primary text, borders, lines |
| Blue | `33B6FF` | Key insight highlights, price tag highlight |
| Green | `57DC64` | Strategy bubble (cover), section label highlights, next-step service names |
| Purple | `9795F9` | Discovery bubble (cover) |
| Orange | `FF8000` | Brand bubble (cover) |
| Gray (border) | `595959` | Info box stroke |
| Gray (fill) | `E9E9E9` | Info box background fill |
| White | `FFFFFF` | Text on dark backgrounds |

### Applying Highlights in OOXML

Blue inline highlight:
```xml
<a:highlight><a:srgbClr val="33B6FF"/></a:highlight>
```

Green inline highlight:
```xml
<a:highlight><a:srgbClr val="57DC64"/></a:highlight>
```

Highlight wraps a `<a:rPr>` element inside a run `<a:r>`.

---

## Typography

### Font Families (embedded in template)
- `Red Hat Display` — primary sans-serif
- `Young Serif` — accent serif
- `Manrope` — UI / auxiliary (used sparingly)

### Font Sizes & Weights by Role

| Role | Font | Size (pt) | Weight | Notes |
|------|------|-----------|--------|-------|
| Primary headline line 1 | Red Hat Display | 38 | Black (900) | Text-Heavy slides |
| Primary headline line 2 | Young Serif | 38 | Bold | Text-Heavy slides |
| Secondary headline line 1 | Red Hat Display | 20 | Black (900) | Service/Detailed slides |
| Secondary headline line 2 | Young Serif | 20 | Bold | Service/Detailed slides |
| What's Next headline | Young Serif | 20 | Bold | Single font, no Red Hat Display |
| Pricing service name | Young Serif | 17 | Bold | All caps |
| Chapter label / subtitle | Red Hat Display | 10 | Bold | All caps, always |
| Cover STORY | Young Serif | 160 | Regular | Giant cover text |
| Cover TLRS | Red Hat Display | 160 | ExtraBold | Giant cover text |
| Cover client text | Young Serif | 19 | Bold | "TODAY WE TELL YOU A STORY ABOUT" |
| Cover client name | Red Hat Display | 19 | Black (900) | Client name in all caps |
| Cover bubble labels | Red Hat Display | 14 | ExtraBold | Service bubble text |
| Chapter divider headline | Young Serif | 51 | Bold | Full-bleed slide |
| Body text (main) | Red Hat Display | 13 | Medium | 115% line spacing |
| Body text (info box) | Red Hat Display | 12 | Medium | Standard line spacing |
| Body text (bullets) | Red Hat Display | 11 | Medium | Bullet list items |
| Section label headers | Red Hat Display | 12 | Bold | "Why:", "Timeline:", etc. |
| TOC chapter list | Red Hat Display | 15 | Medium | 115% line spacing |
| Pricing price tag | Red Hat Display | 24 | Black (900) | Right-aligned, blue highlight |

### OOXML Size Encoding
Font size in OOXML = pt × 100. Examples:
- 10pt → `sz="1000"`
- 13pt → `sz="1300"`
- 38pt → `sz="3800"`
- 160pt → `sz="16000"`

### Font Weight in OOXML
- Bold: `b="1"`
- Not bold: `b="0"` (or omit)
- "Black" weight = `b="1"` with typeface `Red Hat Display`
- "ExtraBold" weight = `b="1"` with typeface `Red Hat Display`
- Note: PPTX doesn't have a separate attribute for weight grades — Black/ExtraBold/Medium all use `b="1"` or `b="0"` with the embedded font doing the weight rendering

### Line Spacing
```xml
<!-- 115% line spacing -->
<a:lnSpc><a:spcPct val="115000"/></a:lnSpc>

<!-- Space before paragraphs: 12pt -->
<a:spcBef><a:spcPts val="1200"/></a:spcBef>

<!-- Space before paragraphs: 700pt (What's Next slide) -->
<a:spcBef><a:spcPts val="70000"/></a:spcBef>
```

---

## Layout Coordinates

### Header Zone (present on all content slides)

```
Horizontal rule line:
  x=362400  y=570989  cx=8419200  cy=22860
  1pt line (w=12700), solid black

Chapter label (top-left):
  x=709942  y=195862  cx=2087925  cy=285750
  Font: Red Hat Display Bold 10pt, left-aligned
  Content: "CHAPTER 01", "CHAPTER 02", etc.

Center subtitle (top-center):
  x=3693367  y=195862  cx=2257225  cy=285750
  Font: Red Hat Display Bold 10pt, center-aligned (algn="ctr")
  Content: slide topic in ENGLISH ALL CAPS
  Note: Some slides have wider subtitle box (cx=3152400 or cx=3173700)
  — use whichever fits the text without truncation
```

### Text-Heavy Layout (slides 5, 6, 7, 12)

```
Headline box (left):
  x=677175  y=1227425  cx=3402300  cy=2444100

Body text box (right):
  x=4079400  y=1227425  cx=3908700  cy=3315300
```

### Detailed Service Layout (slides 8, 9)

```
Headline box (left-top):
  x=677175  y=1227425  cx=2935800  cy=1234500

Info box (left-bottom, rounded rect):
  x=677325  y=2592775  cx=2935800  cy=1782300
  Fill: E9E9E9  |  Stroke: 595959

Right top section:
  x=4079400  y=1227425  cx=4702200  cy=1234500

Right bottom section (with bullets):
  x=4079400  y=2461925  cx=4856700  cy=2447700
```

### Investment / Pricing Layout (slide 10)

```
Outer border box (rounded rect, no fill):
  x=415500  y=1370550  cx=8313000  cy=2402400
  Border: black, w=9525

Service description text box (inside border):
  x=620338  y=1892455  cx=5067300  cy=1354500

Price text box (inside border, right):
  x=6134850  y=2692850  cx=2196300  cy=554100
  Alignment: right (algn="r")
```

### What's Next Layout (slide 11)

```
Headline box (left):
  x=677175  y=1227425  cx=2935800  cy=2444100

Body text box (right):
  x=4079400  y=1227425  cx=3908700  cy=3315300
```

---

## Bullet List Formatting

```xml
<a:pPr marL="457200" indent="-298450">
  <a:buFont typeface="Arial" panose="020B0604020202020204" pitchFamily="34" charset="0"/>
  <a:buChar char="●"/>
</a:pPr>
```

---

## Info Box (Rounded Rectangle)

```xml
<p:sp>
  <p:nvSpPr>
    <p:cNvPr id="[ID]" name="[NAME]"/>
    <p:cNvSpPr><a:spLocks noGrp="1"/></p:cNvSpPr>
    <p:nvPr/>
  </p:nvSpPr>
  <p:spPr>
    <a:xfrm>
      <a:off x="677325" y="2592775"/>
      <a:ext cx="2935800" cy="1782300"/>
    </a:xfrm>
    <a:prstGeom prst="roundRect"><a:avLst/></a:prstGeom>
    <a:solidFill><a:srgbClr val="E9E9E9"/></a:solidFill>
    <a:ln w="12700">
      <a:solidFill><a:srgbClr val="595959"/></a:solidFill>
    </a:ln>
  </p:spPr>
  <!-- txBody with content -->
</p:sp>
```

---

## Pricing Border Box

```xml
<p:sp>
  <p:nvSpPr>
    <p:cNvPr id="[ID]" name="PricingBox"/>
    <p:cNvSpPr><a:spLocks noGrp="1"/></p:cNvSpPr>
    <p:nvPr/>
  </p:nvSpPr>
  <p:spPr>
    <a:xfrm>
      <a:off x="415500" y="1370550"/>
      <a:ext cx="8313000" cy="2402400"/>
    </a:xfrm>
    <a:prstGeom prst="roundRect"><a:avLst/></a:prstGeom>
    <a:noFill/>
    <a:ln w="9525">
      <a:solidFill><a:srgbClr val="000000"/></a:solidFill>
    </a:ln>
  </p:spPr>
  <p:txBody>
    <a:bodyPr/>
    <a:lstStyle/>
    <a:p><a:endParaRPr lang="cs-CZ" dirty="0"/></a:p>
  </p:txBody>
</p:sp>
```

---

## Text Run Templates

### Mixed-font headline (38pt)
```xml
<p:txBody>
  <a:bodyPr wrap="square" rtlCol="0"/>
  <a:lstStyle/>
  <!-- Line 1: Red Hat Display Black -->
  <a:p>
    <a:r>
      <a:rPr lang="cs-CZ" sz="3800" b="1" dirty="0">
        <a:latin typeface="Red Hat Display"/>
      </a:rPr>
      <a:t>Where is</a:t>
    </a:r>
  </a:p>
  <!-- Line 2: Young Serif Bold -->
  <a:p>
    <a:r>
      <a:rPr lang="cs-CZ" sz="3800" b="1" dirty="0">
        <a:latin typeface="Young Serif"/>
      </a:rPr>
      <a:t>the gap.</a:t>
    </a:r>
  </a:p>
</p:txBody>
```

### Mixed-font headline (20pt)
Same pattern, change `sz="3800"` → `sz="2000"`.

### Body text with blue highlight
```xml
<a:p>
  <a:pPr><a:lnSpc><a:spcPct val="115000"/></a:lnSpc><a:spcBef><a:spcPts val="1200"/></a:spcBef></a:pPr>
  <a:r>
    <a:rPr lang="cs-CZ" sz="1300" b="0" dirty="0">
      <a:latin typeface="Red Hat Display"/>
    </a:rPr>
    <a:t>Normal body text here. Then </a:t>
  </a:r>
  <a:r>
    <a:rPr lang="cs-CZ" sz="1300" b="0" dirty="0">
      <a:latin typeface="Red Hat Display"/>
      <a:highlight><a:srgbClr val="33B6FF"/></a:highlight>
    </a:rPr>
    <a:t>highlighted key term</a:t>
  </a:r>
  <a:r>
    <a:rPr lang="cs-CZ" sz="1300" b="0" dirty="0">
      <a:latin typeface="Red Hat Display"/>
    </a:rPr>
    <a:t"> continues here.</a:t>
  </a:r>
</a:p>
```

### Chapter label text run
```xml
<a:p>
  <a:pPr>
    <a:lnSpc><a:spcPct val="100000"/></a:lnSpc>
  </a:pPr>
  <a:r>
    <a:rPr lang="cs-CZ" sz="1000" b="1" dirty="0">
      <a:latin typeface="Red Hat Display"/>
    </a:rPr>
    <a:t>CHAPTER 01</a:t>
  </a:r>
</a:p>
```

### Pricing price tag
```xml
<a:p>
  <a:pPr algn="r"/>
  <a:r>
    <a:rPr lang="cs-CZ" sz="2400" b="1" dirty="0">
      <a:latin typeface="Red Hat Display"/>
      <a:highlight><a:srgbClr val="33B6FF"/></a:highlight>
    </a:rPr>
    <a:t>55 000 CZK</a:t>
  </a:r>
</a:p>
```
