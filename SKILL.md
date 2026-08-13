---
name: iui-evaluator
description: >
  Evaluate UI screenshots against Intelligent User Interface (IUI) criteria from academic literature. Trigger when a user uploads screenshots and asks for an IUI evaluation, UX intelligence audit, or AI interface review. Also trigger for: "how intelligent is this UI?", "does this qualify as an IUI?", "evaluate this design for intelligence", "does this use AI well?", "is this a good AI product?", "review the AI UX of this", "what IUI improvements would you suggest?". Conducts structured elicitation (domain, audience, persona), scores across seven research-grounded dimensions, produces a scored report with improvement recommendations and before/after wireframe sketches as a PDF. Works purely from screenshots and supporting product documentation.
---

# IUI Evaluator Skill

Evaluate UI screenshots and prototypes against Intelligent User Interface (IUI) criteria drawn from the academic literature (Maybury & Wahlster 1998; Jameson 2003; Höök 1999; Brdnik et al. 2022; Springer 2015).

Read `/references/iui-rubric.md` before scoring — it contains the full dimension anchors, domain weighting guidance, and band thresholds.

---

## Workflow overview

**Step 1 → 2 → 3 → 4 → 5 → 6**

1. Elicit domain / audience / persona from the user
2. Present a pre-filled context card and wait for confirmation or edits
3. Confirm screenshots are uploaded and note what flows they cover
4. Evaluate against the seven IUI dimensions
5. Present the scores table inline — invite the user to confirm before continuing
6. Generate the full report (markdown inline + downloadable PDF with sketches)

Do not skip or merge steps — each is a distinct turn that requires the user to respond.

**Input requirement:** This skill works purely from screenshots and supporting product documentation (persona docs, market notes, PRDs). Live URLs, design-tool links, and automated capture are out of scope — if a user provides a URL, ask for screenshots instead (see Step 3b).

---

## Step 1: Elicitation — interactive prompt first, prose fallback

If an interactive question tool is available in the environment (e.g. `ask_user_input` — renders tappable option cards), **always use it for elicitation instead of asking in prose.** Construct exactly two questions in a single call:

1. **Domain** (single select). Infer the three most likely domains from the screenshots and conversation, and offer those three plus "Something else" as the fourth option. Never present a generic unordered domain list — the inferred options show the user you've already looked at their frames.
2. **Why you're evaluating this** (single select): "My own product" / "Competitive benchmark" / "Client audit" / "Just exploring".

Interactive option tools cannot take typed answers, so **never put the persona or audience in the pop-up.** Instead, after the pop-up response arrives, draft them yourself — a one-line audience description and a one-line persona (name, age, role, goal, one key frustration) inferred from the domain, the screenshots, and any attached documentation — and present both in the Step 2 context card for confirmation. Turning persona capture from a writing task into a confirm-or-edit decision is the intended experience.

If no interactive tool is available, fall back to a single conversational message. Do not use a numbered list or form — keep the tone direct and professional:

> "Before I evaluate this interface, I need a little context. Which domain does it belong to — health, education, finance, productivity, consumer, or something else? Who are the primary users, and do you have a quick persona sketch (even just a name, role, and one key frustration)?"

If the user has already provided any of this (earlier in the conversation, or in their initial message), extract it silently, skip the corresponding question, and pre-fill Step 2 without asking again. The 3c evidence-gap questions are free-text by nature — always ask those conversationally, never through the option tool.

---

## Step 2: Pre-filled context card — confirm before proceeding

Once you have enough information (from Step 1 or inferred from the conversation), present a context card for confirmation **before** running the evaluation. This surfaces your interpretation and lets the user correct any misread before you invest in scoring.

Format the card exactly like this:

---

**Before I run the evaluation, here's what I'll be working from:**

| Field | Value |
|-------|-------|
| Interface | [inferred name / description] |
| Input type | [screenshots / screenshots + documentation / description] |
| Domain | [inferred domain] |
| Target audience | [inferred audience] |
| Persona | [inferred or user-provided persona] |

Does this look right? Let me know if you'd like to adjust anything — otherwise I'll proceed with the evaluation.

Where the interactive option tool is available, pair the card with a tappable confirm — "Looks right — proceed" / "Let me adjust something" — instead of leaving confirmation as an open question. Edits arrive as a normal typed message.

---

Wait for the user to confirm or correct before moving to Step 3. If they confirm, proceed. If they edit, update silently and confirm once more. Do not begin scoring until the context card is approved.

---

## Step 3: Receive and resolve the UI input

Accept input in any of these forms, then follow the resolution path below before proceeding to Step 4.

### 3a: Uploaded screenshots — the primary input

The required input. Note how many frames were provided and what flows they cover (onboarding, core task, feedback, return). If coverage is thin — a single hero screen, or no post-task screens — say so and invite more frames before scoring, but do not block on it.

If the UI was uploaded before Step 1, proceed directly from Step 2 to the 3c evidence check once the context card is confirmed.

**Supporting documentation is a first-class input alongside screenshots.** If the user attaches a persona document, target-market notes, a PRD, or the companion capture template with its "What happened next?" fields filled in, read it fully and treat its contents as user-confirmed evidence. Proactively invite it once: documentation calibrates scoring to the product's actual users and resolves transition behaviour that screenshots cannot show.

---

### 3b: Other inputs

**URLs are out of scope.** If the user provides a live URL or any design-tool link, do not attempt to fetch or analyse it. Explain briefly that the skill evaluates from screenshots, and ask them to capture 6–8 frames covering the main flow (`Cmd + Shift + 4` on Mac, `Windows + Shift + S` on Windows) — landing, onboarding, core task, any adaptive moment, results, and return state — then upload them here.

**Description-only evaluation** is accepted as a last-resort fallback when the user cannot provide screenshots. Flag clearly that all scores are inferred from description alone and conservative scoring applies throughout.

---

### 3c: State your evidence base and surface evidence gaps before scoring

Before moving to Step 4, always state in one sentence what evidence you are working from:

- *"Evaluating from 8 screenshots covering onboarding through feedback screens."*
- *"Evaluating from 6 screenshots plus the PRD and persona document."*
- *"Evaluating from user description — all scores are inferred and conservatively applied."*

**Then run the evidence-gap check.** Screenshots capture states; adaptive behaviour lives in the transitions *between* states. Review the frames for moments where the system announces or implies a behaviour you cannot actually see — a difficulty change, a personalised result, an adaptive redirect, a recommendation whose follow-on is off-screen. For each such moment that would materially affect a dimension score or a recommendation, ask the user what actually happened rather than inferring it.

Rules for the gap check:

- Ask **at most 3–5 questions**, in a single conversational message, ordered by how much the answer affects scoring. Skip the check entirely if there are no material gaps.
- Each question must point at a specific frame and a specific unobserved behaviour: *"Frame 12 shows 'Let's make this a bit harder' — what actually changed in the exercises after that message?"*
- If the user answers, treat the answer as **user-confirmed evidence** — it may be used anywhere observed evidence may be used.
- If the user doesn't know or doesn't answer, treat the behaviour as **unobserved**: score conservatively on that point and describe it in the report only in hedged form (*"the screenshots do not show what the harder format is"*). Never substitute a plausible-sounding specific.

This check happens once, here — do not scatter clarifying questions through later steps.

---

## Step 4: Evaluate against the seven IUI dimensions

Read `/references/iui-rubric.md` for full scoring anchors before scoring.

The seven dimensions:

1. **Adaptivity & Personalisation** — does the interface adapt to individual users over time?
2. **User Modelling & Representation** — does the system maintain and act on a model of the user?
3. **Context-Awareness** — does the interface respond to situational, temporal, or task context?
4. **Transparency & Explainability** — can the user understand why the interface behaves as it does?
5. **User Control & Controllability** — can the user override or steer system behaviour?
6. **Naturalness & Interaction Quality** — does the interface support low-friction, natural interaction?
7. **Proactivity & Task Support** — does the interface anticipate user needs and reduce effort?

Score each dimension 0–5. For static prototypes, infer intent from design signals (labels, IA, copy, affordances) and flag assumptions explicitly.

**Evidence tiers — apply to every factual claim.** While scoring, internally classify each claim about the interface as one of:

1. **Observed** — directly visible in a frame (UI copy, an affordance, a layout element). May be stated as fact anywhere.
2. **User-confirmed** — supplied or verified by the user, including answers from the 3c gap check. May be stated as fact anywhere.
3. **Inferred** — plausible but not visible or confirmed. May inform a score (conservatively), but must be written in hedged language wherever it appears (*"the escalation message implies a format change, though the frames don't show what it is"*), and must **never** appear as a specific fact inside a recommendation, verdict, or sketch. If an inferred detail feels important enough to state specifically, that is the signal it belonged in the 3c gap check — ask, don't invent.

The failure mode this prevents: describing an unobserved behaviour with invented specificity (e.g. naming an exact format change no frame shows). A report that penalises interfaces for unexplained decisions must not make unexplained inferences of its own.

---

## Step 5: Score checkpoint — present table and wait for confirmation

**Do not generate the full report yet.** Present only the scores table inline and invite a brief check-in before proceeding.

Present it like this:

---

**Here's how I've scored [interface name] across the seven IUI dimensions. Does this feel right before I write up the full report?**

| # | Dimension | Score | Key Evidence |
|---|-----------|-------|--------------|
| 1 | Adaptivity & Personalisation | x/5 | [1-sentence evidence summary] |
| 2 | User Modelling & Representation | x/5 | [1-sentence evidence summary] |
| 3 | Context-Awareness | x/5 | [1-sentence evidence summary] |
| 4 | Transparency & Explainability | x/5 | [1-sentence evidence summary] |
| 5 | User Control & Controllability | x/5 | [1-sentence evidence summary] |
| 6 | Naturalness & Interaction Quality | x/5 | [1-sentence evidence summary] |
| 7 | Proactivity & Task Support | x/5 | [1-sentence evidence summary] |
| | **Total** | **x/35 (x%)** | |

**IUI Maturity Band: [Band Name]**

If any score feels off, or you think I've missed something visible in the interface, let me know now and I'll adjust before generating the report.

---

Wait for the user to respond. Where the interactive option tool is available, offer the checkpoint as tappable options — "Scores look right — write the report" / "I want to push back on a score" — with pushback details arriving as a typed message. If they approve, proceed to Step 6. If they push back on a score, revise it, briefly explain the adjustment, and present the updated table once more before generating.

---

## Step 6: Generate full report

Once scores are confirmed, generate both outputs in this order:

### 6a: Inline markdown report

Output the full report in the conversation using this structure:

---

### IUI Evaluation Report

**Interface:** [name]
**Input type:** [type]
**Domain:** [domain]
**Audience:** [audience]
**Persona:** [persona]
**Evaluated by:** IUI Evaluator v1.0 — literature-grounded rubric
**Date:** [today's date]
**Score:** [raw]/35 ([pct]%) — [Band]

---

#### IUI Maturity Band

| Band | Raw | % |
|------|-----|---|
| Pre-intelligent | 0–10 | 0–28% |
| Emerging IUI | 11–18 | 31–51% |
| Developing IUI | 19–25 | 54–71% |
| Mature IUI | 26–31 | 74–89% |
| Exemplary IUI | 32–35 | 91–100% |

[One sentence locating this interface in the band and what that means.]

---

#### What This Interface Does Well

2–3 paragraphs. Specific — reference actual visible elements.

---

#### Priority Improvements (Top 3)

Rank by impact for this domain and persona.

**Improvement [n]: [title]**
- **Dimension:** [name]
- **Why it matters for [persona]:** [1–2 sentences]
- **Suggested approach:** [concrete, actionable — no generic UX advice]
- **IUI literature basis:** [citation + principle]

---

#### Domain-Specific Commentary

1 paragraph. Reference domain weighting from the rubric.

---

#### Overall Verdict

1 paragraph. Direct. No hedging. State clearly whether this qualifies as an IUI.

---

### 6b: PDF report with before/after wireframe sketches

Generate a Python + ReportLab script and run it via `bash_tool`. The PDF must include:

- Cover block with score, percentage, and band
- Dimension scores table
- Full written report sections
- For each of the top 3 improvements: before/after wireframe sketch tailored to the specific interface

**PDF layout rules — follow exactly:**

```python
from reportlab.lib.pagesizes import A4
from reportlab.lib.units import mm
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont

# FONTS — mandatory. Base-14 Helvetica cannot render diacritics
# (Heričko, Höök, Šumak print as ■ boxes). Register DejaVu and use it everywhere:
pdfmetrics.registerFont(TTFont('DVS', '/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf'))
pdfmetrics.registerFont(TTFont('DVS-Bold', '/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf'))
pdfmetrics.registerFont(TTFont('DVS-Oblique', '/usr/share/fonts/truetype/dejavu/DejaVuSans-Oblique.ttf'))
# Every ParagraphStyle, TableStyle FONTNAME, and every String() in a Drawing
# must use DVS / DVS-Bold / DVS-Oblique. Never Helvetica, anywhere.

PAGE_W, PAGE_H = A4
LM = RM = 20 * mm
TM = 20 * mm
BM = 28 * mm                          # extra bottom margin for footer
USABLE_W = PAGE_W - LM - RM          # ~481 pt — never exceed in any Table

GUTTER  = 10
PANEL_W = (USABLE_W - GUTTER) / 2    # ~236 pt — sketch panel width
PANEL_H = 160                          # pt — fixed sketch height
PANEL_PAD = 8                          # pt — inner padding on every panel side
LINE_H  = 12                           # pt — fixed line grid inside panels
```

**Table rules:**
- All `colWidths` must sum exactly to `USABLE_W`
- All cell content must be `Paragraph()` objects — never raw strings
- Always set `VALIGN TOP` and `WORDWRAP True` in `TableStyle`
- Use `repeatRows=1` on tables that may span pages

**Sketch rules:**
- Use `Drawing(USABLE_W, PANEL_H)` for each sketch
- Left panel = Before (`x=0`), right panel = After (`x=PANEL_W+GUTTER`)
- Before panel title bar: `#CC4A3C` (red). After panel title bar: `#2A9D8F` (teal)
- No `RoundRect` in ReportLab 4.x — use this Path substitute:

```python
from reportlab.graphics.shapes import Path

def RoundRect(x, y, w, h, radius=3, fillColor=None, strokeColor=None, strokeWidth=0.75):
    r = min(radius, w/2, h/2)
    p = Path(fillColor=fillColor, strokeColor=strokeColor, strokeWidth=strokeWidth)
    p.moveTo(x+r, y); p.lineTo(x+w-r, y)
    p.curveTo(x+w-r, y, x+w, y, x+w, y+r); p.lineTo(x+w, y+h-r)
    p.curveTo(x+w, y+h-r, x+w, y+h, x+w-r, y+h); p.lineTo(x+r, y+h)
    p.curveTo(x+r, y+h, x, y+h, x, y+h-r); p.lineTo(x, y+r)
    p.curveTo(x, y+r, x, y, x+r, y); p.closePath()
    return p
```

- Wrap each `Drawing` in a `Flowable` subclass using `renderPDF.draw()`
- **All panel text must go through a wrap helper — never place raw unwrapped `String`s.** Text laid out at arbitrary coordinates is the cause of the ragged, overflowing sketches this rule exists to prevent:

```python
def panel_text(drawing, x0, y_top, text, font='DVS', size=8, color=None, max_w=None):
    """Wrap text to the panel width and lay lines on the LINE_H grid.
    x0 = panel x + PANEL_PAD; max_w defaults to PANEL_W - 2*PANEL_PAD.
    Returns the y of the next free line."""
    from reportlab.pdfbase.pdfmetrics import stringWidth
    from reportlab.graphics.shapes import String
    max_w = max_w or (PANEL_W - 2 * PANEL_PAD)
    words, line, lines = text.split(), '', []
    for w in words:
        trial = (line + ' ' + w).strip()
        if stringWidth(trial, font, size) <= max_w: line = trial
        else: lines.append(line); line = w
    lines.append(line)
    y = y_top
    for ln in lines:
        drawing.add(String(x0, y, ln, fontName=font, fontSize=size,
                           fillColor=color or colors.HexColor('#333B47')))
        y -= LINE_H
    return y
```

- Budget panel content before drawing: at `LINE_H = 12` a 160 pt panel fits ~11 lines including the title bar — plan each panel's elements to that budget and shorten copy rather than letting it collide or overflow
- Keep every element inside `[x_panel + PANEL_PAD, x_panel + PANEL_W - PANEL_PAD]`; nothing may cross the gutter between panels
- Wrap each improvement block — heading, rationale paragraphs, BEFORE/AFTER label table, and sketch — in `KeepTogether([...])` so a block never splits across a page break; if it cannot fit on the current page it moves whole to the next
- Sketch content must be tailored to the specific interface being evaluated — not generic wireframes
- **BEFORE panels may only depict what is observed:** UI copy and elements reproduced from the actual frames, or user-confirmed details. Never draw a "before" state containing invented specifics.
- **AFTER panels are proposals** — new copy and elements are expected, but any *claim about current system behaviour* embedded in them (what triggers what, what changes to what) must be observed or user-confirmed, or phrased as a proposal (*"e.g. …"*) rather than a description.
- Add a two-cell 'BEFORE / AFTER' label table above each sketch

**Colour palette:**
```
SLATE      = #1E2A38   (headings)
TEAL       = #2A9D8F   (accents, after bars, rules)
TEAL_LIGHT = #E6F4F2   (total row, chip backgrounds)
LIGHT_ROW  = #F5F7FA   (alternating table rows)
RED        = #CC4A3C   (before bars)
```

**Footer:**
- Page number centred
- "IUI Evaluator v1.1 | literature-grounded rubric" right-aligned
- Implemented via `onFirstPage` / `onLaterPages` callbacks

Save to `/mnt/user-data/outputs/iui-evaluation-report.pdf` and run via `bash_tool`.

### 6c: Visual QA — mandatory before delivering

A PDF that has not been looked at must never be delivered. After generating, render every page to images and inspect each one:

```bash
pdftoppm -jpeg -r 80 /mnt/user-data/outputs/iui-evaluation-report.pdf /tmp/qa_page
```

View each `/tmp/qa_page-*.jpg` and check:

- **No ■ / missing-glyph boxes anywhere** — if present, a style is still using Helvetica; fix the font registration
- **No text overflowing a panel, cell, or page margin**, and no text colliding with another element
- **BEFORE/AFTER label bars flush with their panels** — same width, no offset
- **No orphaned fragments** — a page carrying a single stray paragraph means spacing or KeepTogether needs adjusting
- **Tables inside margins** with no column spilling past `USABLE_W`
- **Consistent spacing** between sections across pages

Fix any failure in the script, regenerate, and re-render. Repeat until every page passes. Only then call `present_files`. This loop is not optional — it is the difference between a report that looks designed and one that looks generated.

---

## Evaluation conduct notes

- **Do not conflate general UX quality with IUI quality.** A polished conventional UI can score low. That is a legitimate finding.
- **Score conservatively for static prototypes.** Absence of evidence → score 0–1, with an explicit note.
- **Calibrate to domain.** Missing transparency in a health interface is more serious than in a consumer app.
- **Persona is a lens, not a constraint.** Score universally; contextualise through the persona in the improvements section.
- **Cite evidence, not impressions.** Every score must reference something visible or inferable — a label, affordance, copy, or missing element.
- **Ask, don't invent.** When a behaviour is announced but not shown, the correct move is a 3c gap-check question, not a plausible guess. Unresolved gaps stay hedged all the way through the PDF.
