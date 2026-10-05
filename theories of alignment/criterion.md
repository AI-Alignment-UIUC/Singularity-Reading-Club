# Analysis Criterion and Recursive Step Rules

**Version 1.1 (2026-10-05).** This file defines how every alignment method in this folder is analyzed and how the analyses are summarized upward into the [comparison README](README.md).

> **Change policy.** Edit this file only with Evan's explicit permission. When a newly added paper suggests a change (a new failure mode, a sub-question that should be asked of every method, a better verdict scale), record it under [Proposed amendments](#proposed-amendments) and ask Evan. Once he approves, apply it, bump the version, log it in the [changelog](#changelog), and re-run the affected steps for the papers already analyzed. Paper summaries and the README can be updated freely as long as they follow the current version.

Where it comes from:
- **The three criteria** come from Evan's brief for this folder: inner/outer alignment, scale invariance, and competitiveness. Inner/outer alignment and competitiveness are Hubinger's grid from *An overview of 11 proposals* (2020), already used in [Discussion 2](../scalable%20oversight/discussion-2-topics.md).
- **Scale invariance** comes from [Discussion 1](../scalable%20oversight/history.md) (Block 3: the Sierpinski triangle, power laws, exact vs. statistical self-similarity), the question that closes the club's [recursive summary](../discussions/recursive-summary.md) ("does the difficulty of supervision stay self-similar as capability grows... or are there phase transitions?"), and Evan's point after Discussion 2 that amplification and RRM try to learn an invariant human way of choosing sub-solutions.
- **The recursive step rules** come from Wu et al. (2021), as applied in the [recursive summary](../discussions/recursive-summary.md): each layer is summarized from the one below and must be checkable against it.

---

## Part 1. The three criteria

Each method gets a verdict on each sub-criterion, with the assumption the verdict rests on.

### Verdict scale

| Mark | Meaning |
|---|---|
| **✓** | The source gives a plausible argument or evidence that the criterion holds, given a named assumption |
| **~** | Holds only partly, or only under a strong or untested assumption |
| **✗** | A known failure mode applies and the method does not address it |
| **?** | The source does not address it, and we can't infer an answer |
| **n/a** | Not a method (a problem statement, framework or evaluation paper) |

Every mark carries one line naming the **load-bearing assumption**: the single claim that would flip the mark if false.

### C1. Alignment

- **C1a. Outer alignment.** If a model perfectly optimized this training objective, would we be happy with it? (Hubinger: "if we actually got a model that was trying to optimize for the given loss/reward/etc., would we like that model?") Ask what the objective actually rewards: truth, or what an overseer *believes* is true? Approval, or the outcome?
- **C1b. Inner alignment.** Does training actually produce a model pursuing that objective, rather than a proxy or a deceptive mesa-optimizer? Ask what the method does about deceptive alignment, and whether it relies on transparency, adversarial training, or nothing.
- **C1c. Where the human sits.** Record the human's role (demonstrator, evaluator, judge, constitution author, absent). This is not graded. It is carried up because it explains most of the other verdicts.

### C2. Scale invariance

A method is **scale-invariant** if the argument for its safety has the same form at every capability level: the step that lets an overseer supervise a slightly stronger system also works when it is applied again, to a system far beyond the original overseer. Recursion is a fractal; the question is whether alignment survives the zoom.

- **C2a. The repeated step.** Is there a single local step that the method applies at every level (decompose and recombine, argue and refute, assist and evaluate, summarize and check)? Name it. A method with no repeated step is not scale-invariant by construction; it has to be re-argued at each capability level.
- **C2b. What must stay invariant.** What has to be the same at every level for the step to keep working? Usually some piece of human judgment: the function that picks good sub-answers, spots a flawed argument, or says "this step is acceptable and not deceptive." Is it plausible that this is a fixed function (or a stable basin) rather than something that drifts with framing, context and the strangeness of higher-level questions?
- **C2c. Error behavior across levels.** Does the method apply a correcting step at every level (a check, vote or refutation)? State, or estimate, how error at level *n+1* depends on error at level *n*, and say which regime applies: **contracting** (error shrinks at every level), **thresholded** (shrinks only while per-step error stays below a known bound), **compounding** (fidelity falls like αⁿ), or **cascading** (errors flow up faster than they are corrected, so better leaf-level judgment doesn't raise the ceiling). A method that reports error at only one level gets **?**. The regimes come from the [scale invariance library](scale%20invariance%20library/2-errors-across-levels.md) (von Neumann 1956, Knill et al. 1998, Williamson 1967, Lorenz 1969).
- **C2d. Phase transitions.** Name any capability threshold where the argument changes kind rather than degree: the model can model the overseer, deceive it, self-modify, copy itself, or produce outputs no human-checkable piece can capture (obfuscated arguments, Gödel-style limits on self-certification).
- **C2e. Exact or statistical.** Is the self-similarity exact (a proof that holds at every level, like the Sierpinski triangle at scale 2ⁿ) or statistical (an empirical trend over the range tested, like a coastline)? An empirical result at one gap size is one point on the log-log plot, not a slope.
- **C2f. Measurability.** Can the claim be tested across scales, for example with sandwiching (Bowman et al.) or weak-to-strong experiments at several capability gaps? A box-counting plot needs several scales.

The C2 verdict is the weakest of C2b to C2d, adjusted by C2e: a statistical argument can earn at most **~**.

### C3. Competitiveness

- **C3a. Training competitiveness.** Could a lab with a reasonable lead afford it without throwing that lead away? (Hubinger.) Count extra human labor, extra model calls, and whether it fits existing training pipelines.
- **C3b. Performance competitiveness.** If it works, would the result satisfy the use cases for advanced AI? (Hubinger.) Note restrictions on domain, agency or speed.
- **C3c. Alignment tax trend.** Does the cost grow, shrink or stay flat with capability? This connects C3 to C2: a method whose tax grows with scale stops being used just when it matters.

---

## Part 2. Recursive step rules

The analysis is a tree. Layer 0 is the papers. Layer 1 is one summary per paper, in [`papers/`](papers/). Layer 2 is the comparison grid by criterion in the [README](README.md). Layer 3 is the README's synthesis. The same step rules apply at every layer, so the method we use to summarize is itself a single repeated step (C2a), and it should be held to the same standard.

### Rule 0. Sources (Layer 0)
- Read the paper itself where possible. Quote it directly, with section numbers.
- If a source could not be opened, say so at the top of its summary, and mark anything taken from secondary sources.
- Don't quote from memory. A quote that couldn't be checked against the text is marked *(unverified)*.

### Rule 1. Paper → analysis summary (Layer 0 → 1)
Each summary in `papers/` uses this template, in this order:
1. **Header:** citation, link, role (*method*, *method family*, *framework*, *evaluation*, or *motivation*), which sources were actually read.
2. **The method in one paragraph**, with the paper's own one-sentence statement quoted.
3. **C1, C2, C3**, each sub-criterion with its verdict mark, the load-bearing assumption, and at least one quote with its section that supports the verdict. If the paper says nothing, the mark is **?** and the reasoning is labelled as ours.
4. **Verdict line:** one row in the format of the README grid (C1a, C1b, C2, C3a, C3b), so the step upward is a copy, not a rewrite.
5. **Open questions** the paper leaves for the comparison.

Step constraints:
- **Faithfulness.** Every claim attributed to the authors needs a quote or section. Our own judgments are labelled as ours (e.g. "*Our read:*").
- **Relevance filter.** Keep only what bears on a criterion. Experimental details stay only if they are evidence for a verdict.
- **Carry uncertainty up.** A hedge in the source stays a hedge in the summary. Don't upgrade "we hope" to "we show."
- **Steelman before critique.** State the authors' strongest case for each criterion before the failure modes.

### Rule 2. Summaries → comparison (Layer 1 → 2)
- The grid cell for a method is copied from its summary's verdict line. If the comparison suggests a different mark, change the summary first, then copy.
- Group methods by their **repeated step** (C2a), since methods that share a step share failure modes.
- Where two summaries disagree about a shared assumption (for example, whether human judgment is a stable function), keep both and name the disagreement. Don't average it away.
- Each comparison claim links to the summaries it comes from.

### Rule 3. Comparison → synthesis (Layer 2 → 3)
- The synthesis answers three questions only: which load-bearing assumptions recur across methods, which phase transitions threaten most methods at once, and what result would change the most verdicts.
- Every synthesis claim must be traceable to at least two grid rows. A claim that rests on one paper belongs in that paper's summary.
- Keep it short enough to read in two minutes. That is the whole point of the top layer.

### Rule 4. Audit (every layer)
- **Checkable downward.** A reader should be able to verify any line at layer *k* by reading only layer *k − 1*. That is the property Wu et al. rely on and the one that makes the tree oversight rather than just compression.
- **Spot-check.** When a summary is added or changed, check at least one of its quotes against the source, and one grid cell against its summary.
- **Corrections flow down, then up.** A correction is made at the lowest layer where it applies, then re-propagated.

### Rule 5. Adding a method
1. Write its Layer 1 summary under the current criterion version.
2. Add its row to the grid and re-check the synthesis.
3. If it doesn't fit the criterion (a failure mode no sub-criterion catches, a role the template doesn't cover), don't stretch the criterion. Write it under [Proposed amendments](#proposed-amendments) and ask Evan.

---

## Proposed amendments

Suggestions waiting for Evan's decision. Nothing here is in force.

| # | Proposed change | Prompted by | Status |
|---|---|---|---|
| 1 | **Add C1d, monitorability.** Does training preserve our ability to detect misalignment, or does it optimize against the detector? Require the check to use a detector *held out* from training (a different probe family or interpretability method), since a refit of the same detector shows only that one door is still open. | [Libon 2026](papers/libon-2026-training-against-probes.md) (frozen probes are evaded, §4.1; the post-training audit is a fresh linear probe on the same labels, §4.4), [Hubinger 2020](papers/hubinger-2020-eleven-proposals.md) | Awaiting Evan |
| 2 | **Say how marks combine in C2.** Rank the marks (is ? weaker than ~?) so "weakest of C2b to C2d" is defined, and decide whether a failure measured on a weak policy caps a method at ✗ or at ~. | [Christiano 2018](papers/christiano-2018-amplification.md) and [Leike 2018](papers/leike-2018-reward-modeling.md) (C2c is ?, reported C2 ~), [Wu 2021](papers/wu-2021-recursive-summarization.md) (C2c measured ✗) | Awaiting Evan. 1.1 settles when C2c is ? (error reported at one level only); how marks combine is still open |
| 3 | **Split C2b by kind of invariant.** Record which one the method needs: a whole selection or decomposition function, an outcome-evaluation judgment, only a step-acceptability (corrigibility) basin, or a fixed written spec plus a model's reading of it. Add a second question: who chooses which branch gets checked, and can they find the flaw within budget? | [Barnes 2020](papers/barnes-2020-obfuscated-arguments.md), [Hubinger 2020](papers/hubinger-2020-eleven-proposals.md), [Leike 2018](papers/leike-2018-reward-modeling.md) §7.5, [Bai 2022](papers/bai-2022-constitutional-ai.md) | Awaiting Evan |
| 4 | **Add C2g, the overseer-model gap.** Must the overseer stay at least as strong as the model at every step, and does the chain re-anchor to humans between generations? | [Hubinger 2020](papers/hubinger-2020-eleven-proposals.md) (P2), [Bai 2022](papers/bai-2022-constitutional-ai.md) | Awaiting Evan |
| 5 | **Split C2c.** Ask separately whether errors grow and whether our ability to *detect* errors degrades ("more frequent but less understood"). | [Anthropic 2026](papers/anthropic-2026-when-ai-builds-itself.md) | Awaiting Evan. Not covered by 1.1, which addresses error growth only |
| 6 | **Name standard thresholds in C2d** to check for every method: the model can model its overseer, evaluator or detector; a same-family evaluator; the move from imitation to optimizing learned approval; the move from recommending to acting; reward or feedback tampering. | [Amodei 2016](papers/amodei-2016-concrete-problems.md), [Hubinger 2019](papers/hubinger-2019-risks-from-learned-optimization.md), [Christiano 2018](papers/christiano-2018-amplification.md) §4.3, [Bai 2022](papers/bai-2022-constitutional-ai.md) | Awaiting Evan |
| 7 | **Tighten C1b and C2f.** Grade inner-alignment safeguards at the capability level the method targets, not today's. In C2f, ask whether the measurement includes a model with a reason to evade, whether scale trends hold per domain (weak-to-strong's slope changes sign across tasks), and whether the overseer's calibration is reported. | [Hubinger 2020](papers/hubinger-2020-eleven-proposals.md), [Burns 2023](papers/burns-2023-weak-to-strong.md), [Bowman 2022](papers/bowman-2022-measuring-progress.md), [Lindsey 2025](papers/lindsey-2025-biology-of-llm.md) | Awaiting Evan |
| 8 | **Add a criterion for risks outside the model** ("safe model, unsafe world"). | [Hubinger 2020](papers/hubinger-2020-eleven-proposals.md) (STEM AI's vulnerable-world worry) | Awaiting Evan |
| 9 | **Template variants in Rule 1.** Add a "framework and method family" role, and let evaluation papers grade the baseline method they test with a full row. | [Hubinger 2020](papers/hubinger-2020-eleven-proposals.md), [Bowman 2022](papers/bowman-2022-measuring-progress.md) | Awaiting Evan |

## Changelog

- **1.0 (2026-10-05):** first version.
- **1.1 (2026-10-05):** C2c now asks for a correcting step and the per-level error map, and names four regimes (contracting, thresholded, compounding, cascading). A method that reports error at only one level gets ?. Approved by Evan; from suggestion B in the [scale invariance library](scale%20invariance%20library/README.md#suggestions-for-c2). Every summary's C2c was re-checked; no mark changed.
