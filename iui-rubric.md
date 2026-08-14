# IUI Evaluation Rubric — Literature Reference

## Theoretical Grounding

This rubric draws on the following bodies of work:

- **Maybury & Wahlster (1998)** — foundational definition: IUIs improve efficiency, effectiveness, and naturalness of HCI by reasoning on models of the user, domain, task, discourse, and media.
- **Jameson (2003)** — adaptive interfaces and agents; benefits/costs of adaptivity depend on system ecology (context, user population, usage variability).
- **Höök (1999)** — designing and evaluating IUIs; utility and usability of adaptive hypermedia; early articulation of transparency and user control as IUI quality criteria.
- **Brdnik, Heričko & Šumak (2022)** — systematic mapping study of 211 IUI papers (2012–2022); identifies adaptation, representation, and intelligence as the three core IUI characteristics.
- **Lavie & Meyer (2010)** — benefits and costs of adaptive UIs cannot be evaluated in isolation; must account for system ecology.
- **Findlater & Gajos (2009)** — adaptive mechanisms may improve one dimension while increasing cognitive/perceptual load along another.
- **Mezhoudi, Khaddam & Vanderdonckt (2015) — "Toward Usable IUI"** — identifies Controllability, Predictability, and Transparency as paramount IUI usability criteria.
- **Affective Computing / Multimodality literature** — naturalness, context-fitness, multimodal input/output, emotional responsiveness.

---

## The Seven Evaluation Dimensions

### Dimension 1: Adaptivity & Personalisation
*Does the interface adapt to individual user behaviour, preferences, or context over time?*

**What to look for:**
- Evidence of adaptive layouts, content, or workflows (not just theming)
- Personalisation mechanisms (user profiles, history, preference capture)
- Distinction between *adaptive* (system-driven) and *adaptable* (user-driven) features — both are valid, each with trade-offs
- Onboarding or progressive disclosure that accounts for novice vs. expert users

**Scoring anchors:**
- 0 — Fully static; no adaptation of any kind
- 1 — Superficial theming/customisation only (e.g. dark mode)
- 2 — Basic adaptable features (user-configurable preferences)
- 3 — Some adaptive behaviour (e.g. recently used items, smart defaults)
- 4 — Clear adaptive personalisation with evidence of user modelling
- 5 — Rich, multi-signal personalisation that evolves with user behaviour

---

### Dimension 2: User Modelling & Representation
*Does the system appear to maintain and act on a model of the user's goals, knowledge, or context?*

**What to look for:**
- Role or skill-level awareness in content/feature presentation
- Goal inference (does the interface anticipate next steps or needs?)
- Contextual memory (does the system remember prior interactions or state?)
- Transparency about what the system "knows" about the user

**Scoring anchors:**
- 0 — No evidence of user modelling
- 1 — Minimal role-based access control only
- 2 — Implicit modelling (e.g. recency/frequency heuristics)
- 3 — Explicit user state tracking with visible effects on the interface
- 4 — Multi-dimensional user model (role + skill + history + goal)
- 5 — Transparent, inspectable user model with user agency over it

---

### Dimension 3: Context-Awareness
*Does the interface respond to situational context — temporal, spatial, task, or environmental?*

**What to look for:**
- Temporal context (time of day, session stage, urgency)
- Task context (current workflow state, prior steps, downstream implications)
- Device/environmental context (responsiveness isn't sufficient; context-sensitive *content* matters)
- Domain-specific context (e.g. patient acuity in health, learner progress in education)

**Scoring anchors:**
- 0 — No contextual awareness
- 1 — Device-responsive only
- 2 — Workflow-aware (current task state affects interface)
- 3 — Multi-dimensional context sensitivity (task + temporal)
- 4 — Proactive context use (interface acts on inferred context, not just responds)
- 5 — Rich, domain-appropriate context model with graceful degradation when context is unavailable

---

### Dimension 4: Transparency & Explainability
*Can the user understand why the interface is behaving as it is?*

**What to look for (per Höök 1999, Mezhoudi et al. 2015):**
- Explanations for automated decisions or recommendations
- Visible reasoning or rationale (not black-box outputs)
- Feedback when the system makes inferences or predictions
- Ability to inspect or override system decisions

**Scoring anchors:**
- 0 — No transparency; system acts as a black box
- 1 — Minimal labels or descriptions
- 2 — Some explanations for system outputs
- 3 — Consistent rationale provided for adaptive or AI-driven behaviour
- 4 — Inspectable logic; user can see what triggered a behaviour
- 5 — Full transparency with user-accessible explanation layer and audit trail

---

### Dimension 5: User Control & Controllability
*Can the user override, correct, or steer system behaviour?*

**What to look for (per Jameson 2003, Mezhoudi et al. 2015):**
- Undo/redo and error recovery affordances
- Ability to correct system inferences or predictions
- User-driven vs. system-driven adaptation balance
- Graceful degradation when automation fails or surprises the user

**Scoring anchors:**
- 0 — No user control over system behaviour
- 1 — Basic undo/redo only
- 2 — Some customisation controls
- 3 — User can override adaptive behaviour
- 4 — Fine-grained control with clear feedback loops
- 5 — Full human-in-the-loop: user can inspect, correct, and teach the system

---

### Dimension 6: Naturalness & Interaction Quality
*Does the interface support natural, low-friction interaction aligned with user mental models?*

**What to look for:**
- Natural language input or conversational interaction where appropriate
- Multimodal options (voice, gesture, touch, visual) relevant to domain
- Alignment with user mental models (not just WIMP conventions)
- Cognitive load minimisation — progressive disclosure, chunking, meaningful defaults
- Error prevention and graceful error handling

**Scoring anchors:**
- 0 — Highly mechanical, form-based only interaction
- 1 — Standard GUI with no natural interaction affordances
- 2 — Some natural interaction (e.g. search, autocomplete)
- 3 — Multimodal or conversational features present
- 4 — Well-integrated natural interaction that reduces cognitive load
- 5 — Seamless, multimodal, low-friction interaction that matches domain communication norms

---

### Dimension 7: Proactivity & Task Support
*Does the interface anticipate needs, offer assistance, or reduce user effort through intelligent task support?*

**What to look for:**
- Proactive suggestions, alerts, or recommendations
- Intelligent defaults based on past behaviour or context
- Guided workflows or step assistance
- Automation of repetitive or predictable sub-tasks
- Appropriate proactivity (not interruptive or intrusive)

**Scoring anchors:**
- 0 — Purely reactive; no proactive features
- 1 — Basic notifications only
- 2 — Smart defaults or autocomplete
- 3 — Contextual suggestions or guided workflows
- 4 — Proactive task assistance based on inferred goals
- 5 — Anticipatory assistance that reduces cognitive load without overriding user agency

---

## Domain-Specific Weighting Guidance

Different domains emphasise different dimensions. Adjust interpretation accordingly:

| Domain | Priority Dimensions |
|--------|-------------------|
| Health / Clinical | Context-Awareness, Transparency, Controllability (patient safety stakes) |
| Education / EdTech | User Modelling, Adaptivity, Proactivity (learner scaffolding) |
| Finance / FinTech | Transparency, Controllability, Naturalness (trust and compliance) |
| Productivity / Enterprise | Proactivity, Adaptivity, Naturalness (efficiency gains) |
| Consumer / eCommerce | Naturalness, Proactivity, Adaptivity (engagement and personalisation) |
| Accessibility-first | Naturalness (multimodality), Adaptivity, Controllability |

---

## Scoring & Output Format

**Total IUI Score: /35** (sum of seven dimensions, each 0–5)

**Percentage conversion:** `(total score ÷ 35) × 100`, rounded to nearest whole number.
Example: 24/35 = 68.6% → **69%**

**Band interpretation:**

| Band | Raw Score | Percentage | Description |
|------|-----------|------------|-------------|
| Pre-intelligent | 0–10 | 0–28% | Conventional UI with no meaningful IUI characteristics |
| Emerging IUI | 11–18 | 31–51% | Early adaptive features present; significant gaps across most dimensions |
| Developing IUI | 19–25 | 54–71% | Meaningful intelligence present but uneven; some dimensions strong, others absent |
| Mature IUI | 26–31 | 74–89% | Strong across most dimensions; isolated gaps in specific areas |
| Exemplary IUI | 32–35 | 91–100% | Comprehensive, coherent, well-balanced intelligent interface design |

**Display format in reports:**  
Always show both raw score and percentage: e.g. **24/35 (69%) — Developing IUI**

**Report structure per evaluation:**
1. Context summary (domain, audience, persona, input source)
2. Dimension-by-dimension scores with brief evidence/rationale
3. Strengths summary
4. Priority improvement recommendations (top 3, ranked by impact)
5. Domain-specific commentary
6. Total score + band + one-paragraph overall verdict

---

## Notes on Evaluating Static Prototypes vs. Live UIs

When evaluating from a screenshot or static mockup:

- **Infer intent** where interaction is implied but not demonstrable (e.g. a "personalised feed" label implies user modelling intent even if behaviour can't be tested)
- **Flag assumptions** explicitly — note when a score reflects inferred behaviour rather than observed behaviour
- **Penalise absence** — if a dimension shows no evidence in a prototype, score conservatively; absence of evidence is evidence of absence in early-stage design
- **Credit design signals** — UI copy, information architecture, affordances, and labelling all carry evidence of IUI intent even in static form
