# IUI Evaluator

**A Claude skill for evaluating how intelligent your interface actually is.**

Most UI feedback focuses on usability. This skill asks a harder question: does your interface *reason* about its users, *adapt* to their context, and *anticipate* what they need — or does it just look good and get out of the way?

The IUI Evaluator scores any interface against seven dimensions drawn from two decades of Intelligent User Interface research. It produces a structured report, a percentage score, a maturity band, and — critically — before/after wireframe sketches showing exactly what each recommended improvement would look like.

---

## What is an Intelligent User Interface?

An Intelligent User Interface (IUI) is a human-computer interface that improves the efficiency, effectiveness, and naturalness of interaction by **reasoning on models of the user, domain, task, and context** — not just responding to direct input.

The term was formalised by Maybury & Wahlster (1998) and has been refined across hundreds of studies since. The short version: a conventional UI waits to be told what to do. An intelligent interface anticipates, adapts, and explains itself.

The gap between these two things is large, underappreciated, and measurable — which is what this skill does.

---

## What it evaluates

Seven dimensions, each scored 0–5:

| # | Dimension | The question it asks |
|---|-----------|----------------------|
| 1 | **Adaptivity & Personalisation** | Does the interface change behaviour based on individual users over time? |
| 2 | **User Modelling & Representation** | Does the system maintain and act on a model of who the user is? |
| 3 | **Context-Awareness** | Does it respond to temporal, task, or situational context — not just explicit input? |
| 4 | **Transparency & Explainability** | Can users understand *why* the interface is behaving as it does? |
| 5 | **User Control & Controllability** | Can users override, correct, or steer system behaviour? |
| 6 | **Naturalness & Interaction Quality** | Does it support low-friction, natural interaction — language, voice, multimodal? |
| 7 | **Proactivity & Task Support** | Does it anticipate needs and reduce cognitive load without being intrusive? |

**Total: /35. Converted to a percentage and mapped to a maturity band:**

| Band | Score | % |
|------|-------|---|
| Pre-intelligent | 0–10 | 0–28% |
| Emerging IUI | 11–18 | 31–51% |
| Developing IUI | 19–25 | 54–71% |
| Mature IUI | 26–31 | 74–89% |
| Exemplary IUI | 32–35 | 91–100% |

---

## What you get

A full evaluation produces:

- **A scored dimension table** with evidence for each score, shown inline for your review before the report is finalised
- **A written report** with strengths, three prioritised improvements, domain-specific commentary, and an overall verdict
- **A downloadable PDF** — professionally formatted, with the same report content plus before/after wireframe sketches for each improvement
- **Before/after sketches** — not generic wireframes, but targeted illustrations of the specific UI change recommended, generated for your interface

The evaluation workflow includes two confirmation checkpoints: one before scoring begins (to validate how the interface's domain and persona have been interpreted), and one after scoring (to let you push back on any dimension before the full report is written).

---

## What it accepts

**This skill works from screenshots.** Upload frames covering the main flows of your interface and the skill evaluates from there.

| Input | Notes |
|-------|-------|
| **Screenshots** | The only input — upload 6–8 frames covering your main flows |
| **Description only** | Accepted as a fallback — all scores flagged as inferred and conservatively applied |

**What makes good screenshot coverage:**
- Landing / home screen
- Primary onboarding or setup flow  
- The core task screen (where the main work happens)
- Any feedback, results, or output screens
- Settings or personalisation screens if they exist

6–8 screenshots is the sweet spot. Most tools let you capture these with `Cmd + Shift + 4` (Mac) or `Windows + Shift + S` (Windows).

---

### Working with a live app or prototype?

Open it in your browser, take 6–8 screenshots covering the main flow with `Cmd + Shift + 4` (Mac) or `Windows + Shift + S` (Windows), and upload them here. Screenshots and supporting documentation are the only inputs — the skill never fetches URLs.

---

## Example output

**Interface evaluated:** SlideGen — a fictional AI-powered PDF-to-presentation tool  
**Domain:** Productivity / AI Generation  
**Score:** 24/35 (69%) — Developing IUI

| # | Dimension | Score |
|---|-----------|-------|
| 1 | Adaptivity & Personalisation | 3/5 |
| 2 | User Modelling & Representation | 3/5 |
| 3 | Context-Awareness | 4/5 |
| 4 | Transparency & Explainability | 3/5 |
| 5 | User Control & Controllability | 3/5 |
| 6 | Naturalness & Interaction Quality | 4/5 |
| 7 | Proactivity & Task Support | 4/5 |

> *"SlideGen is meaningfully intelligent in the right places. The proactive document analysis and clarification-before-generation pattern demonstrate genuine IUI intent, well-executed. Where it falls short is in the dimensions that differentiate a smart tool from a genuine intelligent collaborator: it does not explain its reasoning, does not persist knowledge of the user across sessions, and offers no meaningful steering once generation is underway."*

---

## Installation

**For Claude.ai (this is what most people want):**

1. Download `iui-evaluator.skill` from the [Releases](../../releases) page
2. Go to **claude.ai → your profile → Customize → Skills**
3. Click the **+** button → **Create skill** → **Upload a skill**
4. Select the `.skill` file you downloaded

The skill is now active. Use it in any conversation — it triggers automatically when you describe wanting to evaluate a UI.

It triggers on phrases like:
- *"Evaluate this UI for IUI criteria"*
- *"How intelligent is this interface?"*
- *"Does this use AI well?"*
- *"Evaluate this design for effective use of AI"*
- *"What IUI improvements would you suggest?"*
- *"Is this a good AI product?"*
- *"Review the AI UX of this"*

---

## The evaluation workflow

The skill runs in six steps, with two checkpoints that require your input before proceeding:

```
1. Elicitation     → skill asks about domain, audience, persona
2. Context card    → skill shows what it understood — you confirm or edit  ◀ checkpoint
3. UI input        → you upload screenshots (or describe the interface)
4. Scoring         → skill evaluates all seven dimensions
5. Score table     → skill shows scores and evidence — you confirm or push back  ◀ checkpoint
6. Full report     → markdown inline + PDF download with wireframe sketches
```

The two checkpoints exist because a wrong persona assumption or a miscalibrated score will propagate into every recommendation. Getting that right before generating the full report is worth the extra turn.

---

## The literature behind the rubric

The seven dimensions and their scoring anchors are grounded in:

- **Maybury & Wahlster (1998)** — *Readings in Intelligent User Interfaces* — the foundational definition of IUI and the user/domain/task/discourse model framework
- **Jameson (2003)** — *Adaptive Interfaces and Agents* — benefits and costs of adaptivity; the system ecology principle (adaptation must match usage variability)
- **Höök (1999)** — *Designing and Evaluating Intelligent User Interfaces* — early articulation of transparency, predictability, and user control as IUI quality criteria
- **Brdnik, Heričko & Šumak (2022)** — *IUI and Their Evaluation: A Systematic Mapping Study* — synthesis of 211 IUI papers (2012–2022); adaptation, representation, and intelligence as the three defining IUI characteristics
- **Lavie & Meyer (2010)** — *Benefits and Costs of Adaptive User Interfaces* — why adaptive features cannot be evaluated in isolation from system ecology
- **Findlater & Gajos (2009)** — *Design Space and Evaluation Challenges of Adaptive Graphical User Interfaces* — how adaptive mechanisms can increase cognitive load even when improving task performance
- **Mezhoudi, Khaddam & Vanderdonckt (2015)** — *Toward Usable Intelligent User Interface* (HCI International 2015, LNCS 9170) — controllability, predictability, and transparency as the three paramount IUI usability criteria

The full rubric with scoring anchors, domain-specific weighting guidance, and notes on evaluating static prototypes is in [`references/iui-rubric.md`](references/iui-rubric.md).

---

## Design decisions

**Why seven dimensions and not more?**
The seven dimensions map to the most consistently cited IUI characteristics across the literature. More dimensions would add noise without adding discrimination — most real interfaces cluster meaningfully across these seven.

**Why 0–5 per dimension?**
Coarse enough to be defensible from visual evidence alone (which is all you have with screenshots), fine enough to distinguish meaningfully different implementations of each quality.

**Why before/after sketches rather than general recommendations?**
Because "add more transparency" is useless advice. A sketch of the specific UI element — the form field that needs a rationale chip, the generation log that needs a pause checkpoint — makes the recommendation actionable without requiring a full design handoff.

**Why two confirmation checkpoints?**
Because IUI evaluation is interpretive. The rubric is grounded in research but applying it requires judgement about what counts as evidence. Surfacing the interpretation before the report is written — and surfacing the scores before the prose is generated — keeps the human in the loop at the moments that matter most.

---

## Roadmap

- **Multi-frame journey evaluation:** Per-screen scores across a sequential flow, with a journey-level aggregate.
- **Comparative mode:** Score two interfaces side by side, with a dimension diff.
- **Phase 2 rubric:** A second evaluation layer grounded in a specific hybrid IUI architecture — repositioning LLMs within a human-centred system rather than treating them as the interface itself. Published as a companion skill with full theoretical basis.

---

## About

Built by Momina Wahab as part of ongoing research into AI-native interface design and the boundaries of what "intelligent" actually means in human-computer interaction.

*This skill will be published as part of a wider body of work on hybrid IUI architecture. The Phase 2 rubric, grounded in that theoretical framework, will follow.*

---

## Licence

MIT. Use it, adapt it, cite the literature.
