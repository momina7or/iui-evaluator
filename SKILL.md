---
name: iui-evaluator
description: >
  Evaluate UI screenshots against Intelligent User Interface (IUI) criteria from academic literature. Trigger when a user uploads screenshots and asks for an IUI evaluation, UX intelligence audit, or AI interface review. Also trigger for: "how intelligent is this UI?", "does this qualify as an IUI?", "evaluate this design for intelligence", "does this use AI well?", "is this a good AI product?", "review the AI UX of this", "what IUI improvements would you suggest?". Conducts structured elicitation (domain, audience, persona), scores across seven research-grounded dimensions, produces a scored report with improvement recommendations and before/after wireframe sketches as a PDF. Requires screenshots as input — for automatic URL capture use the companion iui-evaluator-code Claude Code skill.
---

# IUI Evaluator Skill

Evaluate UI prototypes, screenshots, and Figma designs against Intelligent User Interface (IUI) criteria drawn from the academic literature (Maybury & Wahlster 1998; Jameson 2003; Höök 1999; Brdnik et al. 2022; Springer 2015).

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

**Input requirement:** This skill works from screenshots only. For automatic capture from URLs (Lovable, Bolt, Figma Make, Figma design files, any live web app), use the companion `iui-evaluator-code` skill for Claude Code.

---

## Step 1: Elicitation

Ask all three questions in a single conversational message. Do not use a numbered list or form — keep the tone direct and professional:

> "Before I evaluate this interface, I need a little context. Which domain does it belong to — health, education, finance, productivity, consumer, or something else? Who are the primary users, and do you have a quick persona sketch (even just a name, role, and one key frustration)?"

If the user has already provided any of this (earlier in the conversation, or in their initial message), extract it silently and pre-fill Step 2 without asking again.

---

## Step 2: Pre-filled context card — confirm before proceeding

Once you have enough information (from Step 1 or inferred from the conversation), present a context card for confirmation **before** running the evaluation. This surfaces your interpretation and lets the user correct any misread before you invest in scoring.

Format the card exactly like this:

---

**Before I run the evaluation, here's what I'll be working from:**

| Field | Value |
|-------|-------|
| Interface | [inferred name / description] |
| Input type | [screenshot / Figma URL / live URL / description] |
| Domain | [inferred domain] |
| Target audience | [inferred audience] |
| Persona | [inferred or user-provided persona] |

Does this look right? Let me know if you'd like to adjust anything — otherwise I'll proceed with the evaluation.

---

Wait for the user to confirm or correct before moving to Step 3. If they confirm, proceed. If they edit, update silently and confirm once more. Do not begin scoring until the context card is approved.

---

## Step 3: Receive and resolve the UI input

Accept input in any of these forms, then follow the resolution path below before proceeding to Step 4.

### 3a: Uploaded screenshots

The ideal input. Proceed directly to Step 4 with no further action needed. Note how many frames were provided and what flows they cover.

If the UI was uploaded before Step 1, proceed directly from Step 2 to Step 4 once the context card is confirmed.

---

### 3b: URL input — resolution path

When the user provides a URL, determine which path applies:

#### Path 1 — Standard Figma design file (`figma.com/design/` or `figma.com/file/`)

Use the Figma REST API to retrieve the node tree. This gives richer IUI signal than screenshots: component names, interaction definitions, layout structure, and full IA — all without needing the user to export anything.

Tell the user:

> "This is a Figma design file — I can analyse it directly via the Figma API, which is richer than screenshots. Two quick things before we start:
>
> **1. Token:** Go to figma.com → your profile (top left) → Settings → Security → Personal access tokens → Generate new token. Any Figma account works — free tier is fine, no paid seat needed.
>
> **2. File sharing:** Make sure the file is set to 'anyone with the link can view' (check via Share in Figma). If it's restricted to specific people the API won't be able to read it.
>
> Paste your token here when ready. I won't store it — keep it in your password manager for next time."

**Token safety — important:** Do NOT store the Figma token in Claude's memory or suggest doing so. A personal access token has read access to all files in the user's Figma account. If the user asks whether you can remember it, say:

> "I won't store your token — it has broad access to your Figma account and shouldn't sit in an external database. Keep it in your password manager and paste it each time. It takes a few seconds."

Once you have the token, call:

```
GET https://api.figma.com/v1/files/{FILE_KEY}
Headers: X-Figma-Token: {TOKEN}
```

Extract `FILE_KEY` from the URL: `figma.com/design/{FILE_KEY}/...`

From the response JSON, analyse:
- `document.children` — page and frame structure (IA)
- Component names — naming conventions signal intent (e.g. `UserProfile`, `AdaptiveFeed`, `ConfidenceSlider`)
- `interactions` on nodes — what triggers what (reveals proactivity and control affordances)
- Text content — UI copy, labels, placeholder text
- Constraints and layout — responsiveness signals

Score against IUI dimensions using this structural evidence, flagging that behaviour is inferred from design intent rather than observed.

---

#### Path 2 — Figma Make (`figma.com/make/`)

Figma Make generates a live React app hosted behind Figma's auth wall. `web_fetch` is blocked by robots.txt and the Figma REST API does not apply.

Tell the user immediately — do not ask for screenshots or a description first:

> "Figma Make generates a live React app behind Figma's auth layer — I can't fetch it directly. I have an auto-capture script that handles this: it opens a headless browser, fully renders the app, navigates through your interaction flow automatically, and saves screenshots for me to evaluate. About 2 minutes to run.
>
> Do you have Node.js installed? If yes, I'll give you three commands and you'll be done. If not, I can walk you through installing it first — it's a one-time setup."

Immediately give them the setup instructions — do not wait for them to confirm Node.js knowledge. Use the same step-by-step format as Path 3 (open terminal → install tools → save and run script → upload screenshots). Follow immediately with the full contents of `/capture/capture.js` in a code block.

**Note:** Claude cannot run this script automatically from the chat interface — it has no access to the user's terminal or local machine. If the user asks why it can't run automatically, explain this honestly: that capability requires Claude Code (the terminal tool), not claude.ai chat. A Claude Code version of this skill — where Playwright installs and runs automatically on consent — is on the roadmap.

---

#### Path 3 — Bolt (`bolt.new`) or Lovable (`lovable.app`) or any deployed React/JS app

These are public URLs but render client-side — `web_fetch` returns a JS shell with no meaningful UI content.

Attempt `web_fetch` on the URL. Then:

**If the fetch returns meaningful UI text** (nav labels, headings, button copy, form fields — more than ~200 words of visible content): proceed to score from that, noting it is HTML-structural evidence only.

**If the fetch returns a near-empty shell** (which is the common case for Lovable and Bolt): do NOT fall back to asking for screenshots or a description. Instead, give the user the capture script immediately with full setup instructions. Present it like this:

---

> "This app is built in React — the page source is empty until a real browser runs it, which is why I can't read it directly. I have a capture script that opens a headless (invisible) browser, renders your app fully, clicks through the main flow automatically, and saves screenshots for me to evaluate. Here's how to run it:
>
> **Step 1 — Open your terminal**
> - **Mac:** Press `Cmd + Space`, type *Terminal*, press Enter
> - **Windows:** Press `Windows key`, type *cmd* or *PowerShell*, press Enter
>
> **Step 2 — Install the required tools** (one-time only)
> Paste this into your terminal and press Enter. It installs Node.js if you don't have it, then installs Playwright (the browser automation tool):
>
> ```
> npx --yes playwright install chromium
> ```
>
> If you see an error saying `npx` is not found, Node.js isn't installed yet. Download it from nodejs.org (the LTS version) — install it, then come back and run the line above again. The whole thing takes about 3 minutes.
>
> **Step 3 — Save and run the capture script**
> I'll give you the script in the next message. Save it as `capture.js` on your Desktop, then run:
>
> ```
> node ~/Desktop/capture.js YOUR_APP_URL ./screenshots
> ```
>
> Replace `YOUR_APP_URL` with your actual app URL.
>
> **Step 4 — Upload the screenshots**
> Drag the contents of the `screenshots` folder into this chat. I'll evaluate from there.
>
> Ready to start? I'll send you the script now."

---

Immediately follow with the full contents of `/capture/capture.js` in a code block so the user can copy-paste it directly into a text editor and save it.

If they hit any error at any step, help them troubleshoot inline — common issues are: `npx not found` (Node.js not installed), `permission denied` (Mac: prefix command with `sudo `), `cannot find module` (wrong directory — make sure they're running from where `capture.js` was saved).

If they explicitly cannot run anything (no computer access, on a phone, etc.), only then fall back to:
1. Manual screenshots — ask them to capture 6–8 key screens covering the main flow and drop them into the chat
2. Description-based evaluation as a last resort — flag conservative scoring throughout

**Bonus for Lovable apps:** Lovable projects sync to GitHub. Offer this as an alternative to the capture script:

> "If your Lovable project is connected to GitHub, I can analyse the source code directly — component names, state management, routing, and context usage all carry IUI signal. It's often richer than screenshots for certain dimensions. Share the repo URL if you'd like to try that route instead."

---

#### Path 4 — Described interface (no visual)

If no URL or image is provided, evaluate from description. Flag clearly that all scores are inferred from description alone and conservative scoring applies throughout.

---

### 3c: State your evidence base before scoring

Before moving to Step 4, always state in one sentence what evidence you are working from:

- *"Evaluating from 8 screenshots covering onboarding through feedback screens."*
- *"Evaluating from Figma API node tree — behaviour is inferred from design intent."*
- *"Evaluating from HTML structure only — client-side content not accessible."*
- *"Evaluating from user description — all scores are inferred and conservatively applied."*

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

Wait for the user to respond. If they approve, proceed to Step 6. If they push back on a score, revise it, briefly explain the adjustment, and present the updated table once more before generating.

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

PAGE_W, PAGE_H = A4
LM = RM = 20 * mm
TM = 20 * mm
BM = 28 * mm                          # extra bottom margin for footer
USABLE_W = PAGE_W - LM - RM          # ~481 pt — never exceed in any Table

GUTTER  = 10
PANEL_W = (USABLE_W - GUTTER) / 2    # ~236 pt — sketch panel width
PANEL_H = 160                          # pt — fixed sketch height
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
- Sketch content must be tailored to the specific interface being evaluated — not generic wireframes
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
- "IUI Evaluator v1.0 | literature-grounded rubric" right-aligned
- Implemented via `onFirstPage` / `onLaterPages` callbacks

Save to `/mnt/user-data/outputs/iui-evaluation-report.pdf`. Run via `bash_tool`, confirm success, then call `present_files`.

---

## Evaluation conduct notes

- **Do not conflate general UX quality with IUI quality.** A polished conventional UI can score low. That is a legitimate finding.
- **Score conservatively for static prototypes.** Absence of evidence → score 0–1, with an explicit note.
- **Calibrate to domain.** Missing transparency in a health interface is more serious than in a consumer app.
- **Persona is a lens, not a constraint.** Score universally; contextualise through the persona in the improvements section.
- **Cite evidence, not impressions.** Every score must reference something visible or inferable — a label, affordance, copy, or missing element.
