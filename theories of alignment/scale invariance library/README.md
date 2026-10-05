# Scale Invariance Library

Papers from outside AI alignment that describe scale invariance precisely: fractals, the renormalization group, dynamical systems, error correction, turbulence, firms and statistics. Alignment papers rarely say "scale invariance". They say "iterate", "recursively", "at each level", "the same procedure", "generalizes to stronger models". This library is for learning to recognize the idea under those names, and to ask the questions these older fields have already worked out.

It serves criterion **C2** in [criterion.md](../criterion.md) (C2a to C2f). Nothing here changes the criterion; suggestions are listed under [Suggestions for C2](#suggestions-for-c2) for Evan to decide on.

---

## The six lessons

Each lesson is one thing these fields learned the hard way, the C2 question it sharpens, and where to read it.

**1. A guarantee at every level needs the repeated step to be a contraction.** If one step shrinks every error by a fixed factor, the result is the same from any start and holds at every depth. That is the whole content of the fractal theorem. A step that only *preserves* error, or shrinks it sometimes, gets none of this. *C2a, C2c, C2e.* [Hutchinson](1-self-similarity-and-fixed-points.md#hutchinson-1981-fractals-and-self-similarity), [Lohmiller & Slotine](2-errors-across-levels.md#lohmiller-and-slotine-1998-on-contraction-analysis-for-nonlinear-systems), [Brandt](2-errors-across-levels.md#brandt-1977-multi-level-adaptive-solutions-to-boundary-value-problems).

**2. Deviations from the invariant come in two kinds.** Under iteration, most deviations die out (physics calls them irrelevant, and their dying-out is why details stop mattering). A few grow (relevant), and more iteration cannot remove them. One growing direction is enough to break scale invariance. So C2b's question, "is the human judgment a fixed function or a stable basin?", should be asked direction by direction: random noise in judgment probably washes out, but a consistent bias that every level passes on is a relevant direction. *C2b, C2d.* [Wilson](1-self-similarity-and-fixed-points.md#wilson-1982-the-renormalization-group-and-critical-phenomena-nobel-lecture), [Feigenbaum](1-self-similarity-and-fixed-points.md#feigenbaum-1980-universal-behavior-in-nonlinear-systems), [Kadanoff](1-self-similarity-and-fixed-points.md#kadanoff-1966-scaling-laws-for-ising-models-near-tc).

**3. Stacked levels have a threshold, and it is sharp.** With a correcting step at each level, error per level goes from $p$ to about $cp^2$: below $p = 1/c$ it vanishes with depth, above it it explodes. Without a correcting step, fidelity decays as $\alpha^n$. And when errors flow *up* from finer levels faster than they are corrected, shrinking the bottom-level error buys almost nothing. The question for any recursive method is its per-level error map, not its error at one level. *C2c, C2d.* [von Neumann](2-errors-across-levels.md#von-neumann-1956-probabilistic-logics-and-the-synthesis-of-reliable-organisms-from-unreliable-components), [Knill, Laflamme & Zurek](2-errors-across-levels.md#knill-laflamme-and-zurek-1998-resilient-quantum-computation), [Williamson](2-errors-across-levels.md#williamson-1967-hierarchical-control-and-optimum-firm-size), [Lorenz](2-errors-across-levels.md#lorenz-1969-the-predictability-of-a-flow-which-possesses-many-scales-of-motion), [Kolmogorov (Landau's objection)](3-where-scaling-breaks-and-how-to-test-it.md#kolmogorov-1941-1962-the-local-structure-of-turbulence-and-its-refinement).

**4. Correction only works on errors it can see, and independence is assumed.** Multigrid works because every error is visible at some level; error correction works because errors at different levels are roughly independent; decomposition works when subproblems are nearly independent. An error visible at no level, or the same blind spot at every level, defeats all three. *C2b, C2d.* [Brandt](2-errors-across-levels.md#brandt-1977-multi-level-adaptive-solutions-to-boundary-value-problems), [Knill, Laflamme & Zurek](2-errors-across-levels.md#knill-laflamme-and-zurek-1998-resilient-quantum-computation), [Simon](2-errors-across-levels.md#simon-1962-the-architecture-of-complexity).

**5. Every self-similar regime has edges, and new kinds of behavior live past them.** Scaling holds between an inner and an outer cutoff. Sharp transitions only appear in large systems, so small experiments sit on the near side of them. The worst events often come from a different mechanism and sit off the fitted curve. A scaling exponent can itself drift with a hidden parameter. *C2d, C2e.* [Anderson](3-where-scaling-breaks-and-how-to-test-it.md#anderson-1972-more-is-different), [Barenblatt](3-where-scaling-breaks-and-how-to-test-it.md#barenblatt-intermediate-asymptotics-and-incomplete-similarity), [Sornette](3-where-scaling-breaks-and-how-to-test-it.md#sornette-2009-dragon-kings-black-swans-and-the-prediction-of-crises), [Bak, Tang & Wiesenfeld](1-self-similarity-and-fixed-points.md#bak-tang-and-wiesenfeld-1987-self-organized-criticality).

**6. A straight line on a log-log plot is not evidence of scale invariance.** You need measurements at several scales, a principled range, a goodness-of-fit test, a comparison with forms that fit equally well but extrapolate differently, and ideally a data collapse. Even then the conclusion is an induction. *C2e, C2f.* [Clauset, Shalizi & Newman](3-where-scaling-breaks-and-how-to-test-it.md#clauset-shalizi-and-newman-2009-power-law-distributions-in-empirical-data), [Mitzenmacher](3-where-scaling-breaks-and-how-to-test-it.md#mitzenmacher-2004-a-brief-history-of-generative-models-for-power-law-and-lognormal-distributions), [Mandelbrot](1-self-similarity-and-fixed-points.md#mandelbrot-1967-how-long-is-the-coast-of-britain), [Stanley](1-self-similarity-and-fixed-points.md#stanley-1999-scaling-universality-and-renormalization-three-pillars).

---

## Field guide: what scale invariance looks like in an alignment paper

When an alignment paper uses a phrase on the left, it is making a scale-invariance claim. The middle column is the outside idea it corresponds to; the right column is the question to ask.

| The alignment paper says | It is really claiming | Ask (and read) |
|---|---|---|
| "iterate", "repeat the procedure", "at each step the overseer is the previous model" | a repeated step with a fixed point | Is the step a contraction? Does it converge from any start? ([Hutchinson](1-self-similarity-and-fixed-points.md#hutchinson-1981-fractals-and-self-similarity)) |
| "a team of humans with assistants behaves like a stronger human" | block-spin coarse-graining: same form, new parameters | What was assumed about the form staying the same, and where is the step valid? ([Kadanoff](1-self-similarity-and-fixed-points.md#kadanoff-1966-scaling-laws-for-ising-models-near-tc)) |
| "small mistakes get washed out at higher levels" | errors are irrelevant directions at a stable fixed point | Which errors grow instead? Is there a consistent bias? ([Wilson](1-self-similarity-and-fixed-points.md#wilson-1982-the-renormalization-group-and-critical-phenomena-nobel-lecture)) |
| "only the human's judgment of X needs to be reliable" | universality: only a few features of the input matter | Which feature, and is it the one that survives? ([Feigenbaum](1-self-similarity-and-fixed-points.md#feigenbaum-1980-universal-behavior-in-nonlinear-systems)) |
| "if error accumulation can be bounded" | a threshold theorem | What is the per-level error map, and where is its threshold? ([Knill et al.](2-errors-across-levels.md#knill-laflamme-and-zurek-1998-resilient-quantum-computation), [von Neumann](2-errors-across-levels.md#von-neumann-1956-probabilistic-logics-and-the-synthesis-of-reliable-organisms-from-unreliable-components)) |
| "decompose into subquestions", "factored cognition" | near-decomposable hierarchy | Are the subproblems really nearly independent? Are verified sub-answers stable? ([Simon](2-errors-across-levels.md#simon-1962-the-architecture-of-complexity)) |
| "each level is trained to do what the level above wanted" | a delegation chain with per-level fidelity α | What restores α toward 1 at each level? ([Williamson](2-errors-across-levels.md#williamson-1967-hierarchical-control-and-optimum-firm-size)) |
| "the checker at each level catches errors at its level" | multigrid | Is every error class visible at some level? ([Brandt](2-errors-across-levels.md#brandt-1977-multi-level-adaptive-solutions-to-boundary-value-problems)) |
| "better human labels at the leaves will fix it" | errors flowing up a scale cascade | Does leaf accuracy even set the ceiling? ([Lorenz](2-errors-across-levels.md#lorenz-1969-the-predictability-of-a-flow-which-possesses-many-scales-of-motion)) |
| "if each pair is safe, the whole stack is safe" | composition of contracting systems | Same metric at every level? Bounded coupling? ([Lohmiller & Slotine](2-errors-across-levels.md#lohmiller-and-slotine-1998-on-contraction-analysis-for-nonlinear-systems)) |
| "the trend holds across model sizes", "scaling laws" | a power law over a measured range | Range, fit test, alternatives, extrapolation distance? ([Clauset et al.](3-where-scaling-breaks-and-how-to-test-it.md#clauset-shalizi-and-newman-2009-power-law-distributions-in-empirical-data)) |
| "it worked at this capability gap" | one point on a log-log plot | Where are the other points? ([Mandelbrot](1-self-similarity-and-fixed-points.md#mandelbrot-1967-how-long-is-the-coast-of-britain)) |
| "the slope depends on the task" | incomplete similarity | What hidden parameter is the exponent tracking? ([Barenblatt](3-where-scaling-breaks-and-how-to-test-it.md#barenblatt-intermediate-asymptotics-and-incomplete-similarity)) |
| "emergent capabilities", "qualitatively new behavior" | phase transitions; more is different | What becomes possible past the transition that the argument didn't cover? ([Anderson](3-where-scaling-breaks-and-how-to-test-it.md#anderson-1972-more-is-different)) |
| "we've only seen small failures" | a power-law tail, possibly with dragon-kings | Do the largest failures belong to the same population? ([Sornette](3-where-scaling-breaks-and-how-to-test-it.md#sornette-2009-dragon-kings-black-swans-and-the-prediction-of-crises)) |
| "the system settles into a stable equilibrium with the overseer" | self-organized criticality | Is the equilibrium minimally stable, with failures of every size? ([Bak, Tang & Wiesenfeld](1-self-similarity-and-fixed-points.md#bak-tang-and-wiesenfeld-1987-self-organized-criticality)) |

---

## Applying it to the methods in the comparison

*Our read*, offered as starting points for discussion. These are not verdicts; the verdicts stay in the paper summaries under the current criterion.

- **Iterated amplification** ([summary](../papers/christiano-2018-amplification.md)) is Kadanoff's step: the amplified human is claimed to be "the same kind of overseer with better parameters". Wilson's question is the one to press: is the human's way of decomposing a fixed point that is stable in *every* direction, or does a consistent bias grow with each distillation?
- **Recursive reward modeling** ([summary](../papers/leike-2018-reward-modeling.md)) states its open question in threshold-theorem form ("if error accumulation can be bounded"). Von Neumann and Knill et al. say what an answer would look like: a per-level error map, a correcting step, and a threshold. Williamson says what happens without one.
- **Recursive summarization** ([summary](../papers/wu-2021-recursive-summarization.md)) is the only method in the set where error across levels was measured, and it compounds. In this library's terms that is Williamson's α < 1 with no restoring organ, or Landau's objection measured.
- **Debate** ([summary](../papers/irving-2018-debate.md)) is closest to multigrid: each round focuses on the part of the argument where the disagreement lives. [Obfuscated arguments](../papers/barnes-2020-obfuscated-arguments.md) is the error class that is visible at no level, which is exactly what breaks multigrid and Simon's near-decomposability.
- **Weak-to-strong** ([summary](../papers/burns-2023-weak-to-strong.md)) has measurements at several gaps, which makes it the only method where Clauset's and Barenblatt's tests can be applied today. A slope that changes sign across tasks is incomplete similarity, and the cleanest next step is a data-collapse plot across gaps.
- **Constitutional AI** ([summary](../papers/bai-2022-constitutional-ai.md)) and **training against probes** ([summary](../papers/libon-2026-training-against-probes.md)) have no repeated step in the C2a sense, but Bak, Tang & Wiesenfeld suggest a worry for both: optimization against a fixed evaluator can settle at the edge of what the evaluator catches, where most failures are small and a few are not.

---

## Suggestions for C2

These are proposals for Evan. B was approved and is in [criterion.md](../criterion.md) as version 1.1; the others change nothing until he approves them. Several overlap with amendments already pending there; the overlaps are noted.

| # | Suggestion | From | Overlaps |
|---|---|---|---|
| A | **C2b, ask direction by direction.** Name the deviations from the invariant that the method expects to wash out across levels (noise) and any that would grow (a consistent bias, a correlated blind spot). One growing direction caps C2b. | Wilson, Feigenbaum, Knill et al. | Pending #3 (split C2b by kind of invariant) |
| B | **Applied in criterion 1.1.** **C2c, ask for the per-level error map.** Record whether the method has a correcting step at each level, what the error at level *n+1* is as a function of level *n*, and whether a threshold is known. Separate four behaviors: contracting, bounded by a threshold, compounding (αⁿ), and cascading upward (leaf accuracy doesn't set the ceiling). | von Neumann, Knill et al., Williamson, Lorenz | Pending #2 and #5 |
| C | **C2e, add incomplete similarity.** Besides exact versus statistical, ask whether the slope itself is stable or drifts with the gap, the domain or the overseer's level. A drifting slope is weaker than a noisy one. | Barenblatt, Kolmogorov 1962 | Pending #7 (slope changes sign across tasks) |
| D | **C2f, a minimum standard for a scaling claim.** At least three gaps; the range stated; a fit test or a comparison against one alternative form; and, where possible, a data collapse. A claim that fails this caps at ? rather than ~. | Clauset et al., Stanley, Mandelbrot | Pending #7 |
| E | **C2d, add off-curve failures.** Ask whether the largest failures could come from a different mechanism than the observed ones (dragon-kings), not only whether the trend continues. | Sornette, Anderson | Pending #6 (standard thresholds) |

---

## Reading order for the club

For one session, read three: **Wilson** (the vocabulary), **Knill, Laflamme & Zurek** (the threshold, in two equations), and **Clauset, Shalizi & Newman** §1, §4 and §5.3 (how to doubt a log-log plot). **Simon** is the most readable of the set and needs no math. **Anderson** is four pages and argues the other side.

## All papers

| Paper | Field | Teaches | File |
|---|---|---|---|
| Hutchinson 1981 | fractal geometry | exact self-similarity from a contraction | [1](1-self-similarity-and-fixed-points.md#hutchinson-1981-fractals-and-self-similarity) |
| Mandelbrot 1967 | fractal geometry | statistical self-similarity; slopes need many rulers | [1](1-self-similarity-and-fixed-points.md#mandelbrot-1967-how-long-is-the-coast-of-britain) |
| Kadanoff 1966 | statistical physics | coarse-graining to the same form | [1](1-self-similarity-and-fixed-points.md#kadanoff-1966-scaling-laws-for-ising-models-near-tc) |
| Wilson 1982 | statistical physics | fixed points; shrinking vs growing deviations | [1](1-self-similarity-and-fixed-points.md#wilson-1982-the-renormalization-group-and-critical-phenomena-nobel-lecture) |
| Feigenbaum 1980 | dynamical systems | universality from a repeated step | [1](1-self-similarity-and-fixed-points.md#feigenbaum-1980-universal-behavior-in-nonlinear-systems) |
| Stanley 1999 | statistical physics | glossary; data collapse | [1](1-self-similarity-and-fixed-points.md#stanley-1999-scaling-universality-and-renormalization-three-pillars) |
| Bak, Tang & Wiesenfeld 1987 | complex systems | scale-free states as attractors | [1](1-self-similarity-and-fixed-points.md#bak-tang-and-wiesenfeld-1987-self-organized-criticality) |
| von Neumann 1956 | reliable computing | restoring organs and thresholds | [2](2-errors-across-levels.md#von-neumann-1956-probabilistic-logics-and-the-synthesis-of-reliable-organisms-from-unreliable-components) |
| Knill, Laflamme & Zurek 1998 | quantum error correction | concatenation: p → cp² | [2](2-errors-across-levels.md#knill-laflamme-and-zurek-1998-resilient-quantum-computation) |
| Lorenz 1969 | meteorology | upward error cascades | [2](2-errors-across-levels.md#lorenz-1969-the-predictability-of-a-flow-which-possesses-many-scales-of-motion) |
| Williamson 1967 | economics of firms | control loss αⁿ | [2](2-errors-across-levels.md#williamson-1967-hierarchical-control-and-optimum-firm-size) |
| Brandt 1977 | numerical analysis | scale-independent convergence | [2](2-errors-across-levels.md#brandt-1977-multi-level-adaptive-solutions-to-boundary-value-problems) |
| Lohmiller & Slotine 1998 | control theory | guarantees that compose | [2](2-errors-across-levels.md#lohmiller-and-slotine-1998-on-contraction-analysis-for-nonlinear-systems) |
| Simon 1962 | systems theory | near-decomposability | [2](2-errors-across-levels.md#simon-1962-the-architecture-of-complexity) |
| Anderson 1972 | condensed matter | new laws at each scale | [3](3-where-scaling-breaks-and-how-to-test-it.md#anderson-1972-more-is-different) |
| Barenblatt (substitutes for 1972) | fluid mechanics | intermediate asymptotics; drifting exponents | [3](3-where-scaling-breaks-and-how-to-test-it.md#barenblatt-intermediate-asymptotics-and-incomplete-similarity) |
| Kolmogorov 1941, 1962 | turbulence | a clean cascade, and Landau's objection | [3](3-where-scaling-breaks-and-how-to-test-it.md#kolmogorov-1941-1962-the-local-structure-of-turbulence-and-its-refinement) |
| Sornette 2009 | complex systems | events off the curve | [3](3-where-scaling-breaks-and-how-to-test-it.md#sornette-2009-dragon-kings-black-swans-and-the-prediction-of-crises) |
| Clauset, Shalizi & Newman 2009 | statistics | testing power laws | [3](3-where-scaling-breaks-and-how-to-test-it.md#clauset-shalizi-and-newman-2009-power-law-distributions-in-empirical-data) |
| Mitzenmacher 2004 | applied probability | power law vs lognormal | [3](3-where-scaling-breaks-and-how-to-test-it.md#mitzenmacher-2004-a-brief-history-of-generative-models-for-power-law-and-lognormal-distributions) |

---

## How the quotes were gathered

This follows the criterion's Rule 0 as far as the environment allowed. Direct downloads were blocked, so every quote was read through a web-fetch tool that passes the page through a small model. Each quote is marked:

- **✔** two separate fetches (of the same copy or two copies) returned the same wording.
- ***(single fetch)*** seen once. Check against the PDF before quoting it elsewhere.
- ***(secondary)*** the author's words as quoted in another paper, because the original could not be reached (Lorenz 1969, Williamson 1967).

Math notation, Greek letters and page numbers are the least reliable parts: the tool drops tildes and exponents and was sometimes a page or two off. Three sources were replaced or only partly read, and each entry says so: Barenblatt & Zel'dovich 1972 (replaced by Barenblatt's later open papers), Feigenbaum (the 1983 Physica D reprint, not the 1980 original), and von Neumann (sections 1 to 10.5.3.2; one fetch returned a "conclusions" sentence that later fetches couldn't find, so it is left out). "Our read" paragraphs are our interpretation, not the authors'.
