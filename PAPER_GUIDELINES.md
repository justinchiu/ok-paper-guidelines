# Paper Guidelines (For Agents)

Guidelines adapted from Sasha Rush's talk: https://www.youtube.com/watch?v=qNlwVGxkG7Q

This file is written as **instructions for an agent** helping a human author draft or revise a paper. The optimization target is an "okay paper": **structure, coherence, clarity** (not rhetoric, not cleverness, not exhaustive empirical polish).

Key insight: **great papers are also okay papers**. They meet the structure/coherence/clarity bar first, then layer on creativity, rhetoric, and empirical precision. Nail the okay version before reaching for greatness.

Assumes a standard empirical ML-ish paper; adapt the section mapping as needed.

## Operating Mode

### What to optimize

Think of human knowledge as a circle. Your contribution is a red line pushing past the frontier:
- **Novelty** = the angle/direction you're pushing (what makes this different from other attempts)
- **Importance** = how far you pushed past what was known (why should anyone care)

The paper's job is to make both of these legible to the reader.

- Keep the narrative split into two halves:
  - **Explanatory:** related work, background, method (teach what the contribution is).
  - **Empirical:** experimental setup, results, analysis (set up evidence, then verify and stress-test).

### How to read (and revise) like an author

- Work backwards: infer why each paragraph exists and why it's placed there.
- Be generous about intent, but ruthless about confusion: if a reader could be confused, treat that as actionable signal.
- **Pictorial outline exercise**: For papers you admire, sketch out the section proportions + where figures/tables/algorithms land. This builds intuition for how to lay out your own work. Do this for 2-3 papers before writing.

### The "three levels" constraint

Write in only two modes, and label transitions:

1. **High level** (level 1): Why this matters, what the parts are, what to pay attention to.
2. **Summarization** (level 2): Why might it work? What does it do? Hand-wavy "how it works" that is neither an argument nor an implementable description (**never write here**).
3. **Technical** (level 3): What are the goals? What is the process? Declarative, precise, "should type-check," reproducible. Narrative glue around math / equations, which should stand alone to someone with enough context.

Rule: every paragraph must be clearly **level 1** or **level 3**.
Rule: The math must tell the story while remaining minimal. The contributions of the paper should jump out via math. Uncluttered mathematical storytelling is critical.

## Agent Workflow (default)

1. Ask for (or extract) the paper's **audience**, **three defendable contributions**, and the **evidence** that supports each contribution.
2. Produce a 1-page **pictorial outline** as text (section order + rough % of space + where the key table/figure/algorithm goes).
3. For each section: output a bullet outline first, then prose.
4. Enforce dominoes: empirical sections **never introduce new ideas**; they only validate what was set up earlier.

## Section Playbook

### Introduction

**Goal:** Convey novelty and importance at a high level. Don't say anything else.

*Sets up both explanatory and empirical sections.*

Agent rules:
- Output is **level 1 only** (no math, no implementation details).
- List **3 clearly defendable contributions** that are justified by experiments/analysis later.
- Narrow claims to the smallest area you can actually defend.
- Treat "importance" as both scientific and practical (e.g., enabling laptop inference can be a real contribution).
- It's normal to write the intro **last**, after the rest of the paper stabilizes.

Structure (from GPTQ example):
1. Paragraph 1: Argue for **importance** (LLMs are widely used but use too much memory)
2. Paragraph 2: Discuss **novelty** (compression is well-studied but doesn't apply to large-scale LLM inference)
3. Paragraph 3: List **contributions** explicitly

---

### Related Work (Explanatory)

**Goal:** Demonstrate how your contributions differ from others. Focus on novelty.

Agent rules:
- Not bookkeeping: use other works to explain what your work is (and is not).
- Organize around ~3 facets of your contribution; collect papers per facet (a spreadsheet is fine).
- Prefer "closest alternatives / other attempts at the same goal," not "papers we used."
- Make contrasts explicit: "Unlike X, we …" / "X assumes …; we remove that assumption by …".

Structure:
1. One paragraph per facet of related work
2. Each facet has one sentence on each related paper in that facet, explaining what the paper did
3. Final sentence of paragraph: how this work contrasts with and goes beyond both prior work

---

### Background (Explanatory)

**Goal:** Import the deeper technical substrate your method directly extends or modifies.

Think of it as: `from neurips import X` — but at the **right depth level** for your audience.

Agent rules:
- Background is mostly **level 3**.
- **Skip what's obvious** to your target audience. (Example: GPTQ runs all experiments on transformers but never describes transformers—because the method applies to any large model and the audience knows transformers.)
- **Include what you directly build on.** (Example: GPTQ does cover Optimal Brain Quantization, layer-wise quantization, and the Hessian formulation in detail—because the method directly modifies these.)
- **Litmus test:** Does your Method section directly modify, extend, or contrast with this technical machinery? If yes → Background. If it's just context the audience already has → skip or cite.
- Notation hygiene: define symbols once, define shapes/types, and reuse consistently.
- Background is not linear: it can grow as the Method section reveals what needs importing. Revise as you write the paper.

Common mistakes:
- Writing a textbook intro to concepts your audience already knows (too shallow).
- Skipping the specific technical setup you're building on, forcing readers to dig up prior papers (too sparse).
- Including material that's never referenced again in Method (wasted space).

---

### Method (Explanatory)

**Goal:** Explain this to yourself 6 months ago. Help someone else up the hill.

Agent rules:
- Do not describe the order of discovery; describe the clean conceptual structure.
- Constrain scope by:
  - introducing little to no new notation (push it to Background), and
  - planning ahead: almost anything stated here should be tested later in Results/Analysis.
- Write mostly **level 3**, with short level 1 lead-ins per module.
- Provide "enough to implement," not hand-holding and not memoir.

Structure:
1. Describe modular components/steps at high level
2. For each component: high-level intro on what the goal is, then level 3 technical description
3. Include an algorithm in documented pseudocode (ties the modules into something implementable)

---

### Experimental Setup (Empirical)

**Goal:** Set up the dominoes. Reader should know exactly what experiments will look like.

Agent rules:
- Don't overthink it. "Write everything required to do the experiments," then move ~half to the appendix.
- Keep in the main paper: metrics, data, baselines, and the exact evaluation protocol.
- Just the facts; no speculation.
- If Results starts accumulating setup details, move them back here.
- Explicitly list:
  - baselines included (and what they represent), and
  - baselines not included (cite them and explain why they're excluded).
- Details like model hyperparameters can go in the appendix in a table.

---

### Results (Empirical)

**Goal:** Verify the conclusions stated earlier. Knock down the dominoes.

Agent rules:
- Tables/figures are the star.
- One experiment → one table/figure → one paragraph.
- The paragraph's job is to restate the **takeaway** so the reader can verify they agree on what the table implies.
- Forbid: new notation, new methods, new datasets, new evaluation protocol.

Tables:
- Each table shows results of a single experiment
- Captions should be **declarative** (no "I" or "we")
- Provide enough info for standalone context
- Sort columns to show main comparison clearly
- Separate sections so like compares to like
- Highlight what was being tested

---

### Analysis (Empirical)

**Goal:** Check the robustness of your contribution. Most useful section for serious readers.

Agent rules:
- Analysis is for stress-testing the contribution: would small changes have altered the main conclusions?
- Creativity is allowed, but only if it produces insight.
- Good analysis types:
  - qualitative examples (successes/failures),
  - ablations (which design choices mattered),
  - limits (where it breaks down),
  - adversarial properties (sensitivity to small changes).
- Avoid random trivia; more is not automatically better.
- If an analysis doesn't teach anything, cut it.

---

## Cross-section Guardrails (agent checklist)

- Keep explanatory vs empirical boundaries: set up claims and mechanisms first; validate later.
- Empirical sections never introduce new core ideas; if you find a new idea, move it earlier and restructure.
- Ensure each claim in the Introduction has supporting evidence in Results/Analysis.
- Ensure each technical object used in Method is defined in Background.

## Summary (one-line goals)

| Section | Goal | Key Tip |
|---------|------|---------|
| Introduction | Convey novelty + importance | 3 defendable contributions |
| Related Work | Show how you differ | Use others to explain yourself |
| Background | Import deeper technical substrate | Right depth: skip obvious, include what you extend |
| Method | Explain to past self | Top-down, modular |
| Experimental Setup | Set up dominoes | Just the facts |
| Results | Verify conclusions | Tables are the star |
| Analysis | Check robustness | Only interesting insights |

## Common add-ons

- **Abstract:** A miniature paper: motivation + what you did + the key result(s) in a few sentences.
- **Conclusion:** Restate contributions and the evidence; keep it aligned with the intro claims (no new claims).
- **Appendix:** Move implementation details, extra baselines/ablations, and long proofs so the main narrative stays crisp.
