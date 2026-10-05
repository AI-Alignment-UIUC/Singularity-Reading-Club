# Theories of Alignment

A comparison of alignment methods under three criteria: **inner and outer alignment**, **scale invariance**, and **competitiveness**. Each paper is summarized on its own against a fixed [criterion](criterion.md), and this page compares those summaries.

**Read top-down for the gist or bottom-up to check it.** Layer 3 (the synthesis) is at the top, Layer 2 (the grid) is next, Layer 1 is the [paper summaries](#paper-summaries-layer-1), and Layer 0 is the papers themselves. Any claim here should be checkable against the layer below it.

---

## The criterion in brief

The full version, with its change policy, is in [criterion.md](criterion.md) (version 1.0). It changes only with Evan's permission; suggestions from new papers are logged there as [proposed amendments](criterion.md#proposed-amendments).

**C1. Alignment.** *Outer:* if a model perfectly optimized this objective, would we be happy with it? *Inner:* does training actually produce a model pursuing that objective, rather than a proxy or a deceptive mesa-optimizer? We also record where the human sits.

**C2. Scale invariance.** Does the safety argument have the same form at every capability level? We ask six things: what single step the method repeats; what must stay invariant for that step to keep working (usually some piece of human judgment); whether errors shrink, stay bounded or compound across levels; which capability thresholds change the argument in kind (phase transitions); whether the self-similarity is exact, like the Sierpinski triangle, or only statistical, like a coastline; and whether it can be measured at several scales. This criterion comes from [Discussion 1](../scalable%20oversight/history.md), the closing question of the [recursive summary](../discussions/recursive-summary.md), and Evan's point that amplification tries to learn an invariant human way of selecting sub-solutions.

**C3. Competitiveness.** *Training:* can a lab with a lead afford it? *Performance:* would the result meet the use cases for advanced AI? We also ask whether the alignment tax grows with capability.

**Marks.** ✓ plausible argument or evidence · ~ partial: the source gives an argument, but it rests on an untested assumption or only holds in some settings · ✗ a known failure applies and is not addressed · ? the source is silent · n/a not a method. Every mark in a summary names the assumption that would flip it. C2 is the weakest of its parts, and a merely statistical argument can earn at most ~.

**Recursive step rules.** The analysis is a tree, and the same rules apply at every layer, in the style of [Wu et al. (2021)](papers/wu-2021-recursive-summarization.md):
1. *Sources:* quote the paper with section numbers, and say what couldn't be read.
2. *Paper → summary:* one fixed template per paper; every attributed claim has a quote; our judgments are labelled; hedges stay hedges; steelman before critique.
3. *Summaries → grid:* grid cells are copied from the summaries, never rewritten here; methods are grouped by their repeated step; disagreements are named, not averaged.
4. *Grid → synthesis:* every synthesis claim rests on at least two grid rows.
5. *Audit:* each layer is checkable against the one below; corrections are made at the lowest layer and propagated up.

---

## Layer 3. Synthesis

**1. Almost every scalable method bets on one invariant judgment, and the papers show three different versions of it failing.** Amplification, RRM, recursive summarization and debate all repeat one local step and need some judgment to be the same at every level. [Christiano et al.](papers/christiano-2018-amplification.md) need H's way of picking and combining subquestions; [Leike et al.](papers/leike-2018-reward-modeling.md) need only the user's judgment of outcomes; [Irving et al.](papers/irving-2018-debate.md) need only the judge's verdict on one short leaf claim. The required invariant gets smaller in that order, which supports the weaker reading of Evan's question: what has to be stable may be a basin ("this step is acceptable and not deceptive") rather than a whole selection function ([Hubinger](papers/hubinger-2020-eleven-proposals.md), cross-cutting point 4). But the evidence pushes against each version. Wu et al. found that higher-level inputs are out of distribution for the same step and that labelers disagree on about one comparison in five. Christiano et al. have to keep refitting their model of H because its inputs drift. And [Barnes](papers/barnes-2020-obfuscated-arguments.md) shows a second invariant that nobody had listed: even if the human's judgment of every step were perfect, the honest side has to find *which* step to check, and that can be as hard as factoring.

**2. Inner alignment is the weakest column, and every fix depends on a transparency tool that hasn't been shown to scale.** No method gets ✓ on C1b. The ones that do best lean on inspecting internals: Hubinger's transparency tools and relaxed adversarial training, [attribution graphs](papers/lindsey-2025-biology-of-llm.md), and [training against probes](papers/libon-2026-training-against-probes.md). Lindsey et al. find satisfying graphs for about a quarter of the prompts they tried, on a model with no reason to hide. Libon et al. show in their own experiments that training against a frozen detector teaches the model to evade it. Refitting the probe fixes that, and a fresh linear probe still finds the property afterwards, but the authors note their attacks weren't adapted to the probe and none of the models had a reason to hide. [Risks from Learned Optimization](papers/hubinger-2019-risks-from-learned-optimization.md) predicts exactly where this breaks: a model that knows it is being trained and inspected.

**3. The same phase transition threatens nearly every method: the model becomes able to model its overseer.** It appears as a model gaming a same-family evaluator reading a public constitution ([CAI](papers/bai-2022-constitutional-ai.md)), a strong student imitating its weak supervisor's errors ([weak-to-strong](papers/burns-2023-weak-to-strong.md)), an agent tampering with the user's feedback (Leike §4.3), a model re-encoding a property its probe looks for (Libon), and deceptive alignment (Hubinger 2019). A second transition recurs in the recursive methods: switching from imitation to optimizing a learned approval signal (Christiano §4.3, Hubinger proposal 4).

**4. Errors compound, and only one paper has measured it.** [Wu et al.](papers/wu-2021-recursive-summarization.md) is the one place in the set where error across levels was measured, and it compounds. Leike et al. pose the same question for RRM as an induction "if error accumulation can be bounded" and leave it open. [When AI builds itself](papers/anthropic-2026-when-ai-builds-itself.md) describes the real-world loop where this matters, and adds that errors may become less understood as well as more frequent.

**5. Competitiveness and scale invariance pull in opposite directions.** The cheapest methods (CAI, weak-to-strong, probe training) have weak or no scale arguments. The methods with the cleanest repeated step (debate, amplification, RRM) are only ~ on competitiveness, since they need more human or model calls per judgment. A method that stops being used as capability grows fails the same way a method that stops working does.

**What would change the most verdicts:** a measurement at several capability gaps rather than one, with a model that has a reason to evade (a deceptive model organism), checked by a detector held out from training. Every C2 mark in the grid is statistical, and every experiment in the set gives one or a few points on the plot. [Bowman et al.](papers/bowman-2022-measuring-progress.md) supply the protocol (sandwiching), and weak-to-strong already has several gaps; neither yet has the adversary. In Discussion 1's terms, nobody has drawn the box-counting plot yet.

---

## Layer 2. Comparison grid

Cells are copied from each summary's verdict line. **No method gets ✓ on scale invariance.** That is a result, not a gap: a ✓ needs an argument that holds at every capability level, and the only such arguments in the set (debate's PSPACE theorem, amplification's HCH limit) assume idealized players. Everything else is evidence from one or a few model sizes, which the criterion caps at ~. Each summary's C2 section has the full check (repeated step, invariant, error behavior, phase transitions, exact vs. statistical, measurability).

Cells are copied from each summary's verdict line. Rows are grouped by the step the method repeats, since methods that share a step share failure modes. Hubinger's versions of a method (P*n*) can grade differently from the original paper, because he pairs each with an inner-alignment safeguard and grades his own variant.

| Method | Repeated step | Human's role | Outer alignment (C1a) | Inner alignment (C1b) | Scale invariance (C2) | Training cost (C3a) | Performance (C3b) |
|---|---|---|---|---|---|---|---|
| **Decompose and recombine** | | | | | | | |
| [Iterated amplification (Christiano 2018)](papers/christiano-2018-amplification.md) | decompose, recombine, distill | decomposer | ~ | ✗ | ~ | ~ | ~ |
| [Recursive reward modeling (Leike 2018)](papers/leike-2018-reward-modeling.md) | assist, evaluate, train next agent | evaluator of outcomes | ~ | ~ | ~ | ~ | ✓ |
| [Recursive summarization (Wu 2021)](papers/wu-2021-recursive-summarization.md) | summarize and check | evaluator at each node | ~ | ? | ✗ | ✓ | ~ |
| [P2 Imitative amp. + intermittent oversight](papers/hubinger-2020-eleven-proposals.md#p2-imitative-amplification--intermittent-oversight-3) | as amplification | decomposer | ~ | ~ | ~ | ~ | ~ |
| [P3 Imitative amp. + relaxed adversarial training](papers/hubinger-2020-eleven-proposals.md#p3-imitative-amplification--relaxed-adversarial-training-4) | as amplification | decomposer | ~ | ~ | ~ | ~ | ~ |
| [P4 Approval-based amp. + RAT](papers/hubinger-2020-eleven-proposals.md#p4-approval-based-amplification--relaxed-adversarial-training-5) | as amplification, approval reward | approver | ~ | ~ | ✗ | ~ | ~ |
| [P8 RRM + relaxed adversarial training](papers/hubinger-2020-eleven-proposals.md#p8-recursive-reward-modeling--relaxed-adversarial-training-9) | as RRM | evaluator | ~ | ~ | ~ | ~ | ~ |
| [P10 Amp. + auxiliary RL + RAT](papers/hubinger-2020-eleven-proposals.md#p10-amplification-with-auxiliary-rl-objective--relaxed-adversarial-training-11) | as amplification, plus RL | decomposer | ~ | ~ | ~ | ~ | ~ |
| [P11 Amp. alongside RL + RAT](papers/hubinger-2020-eleven-proposals.md#p11-amplification-alongside-rl--relaxed-adversarial-training-12) | as amplification, plus RL | decomposer | ~ | ~ | ~ | ~ | ~ |
| **Argue and refute** | | | | | | | |
| [Debate (Irving 2018)](papers/irving-2018-debate.md) | argue, refute, recurse into the dispute | judge | ~ | ✗ | ~ | ~ | ~ |
| [Debate after obfuscated arguments (Barnes 2020)](papers/barnes-2020-obfuscated-arguments.md) | same, with cross-examination | judge | ~ | ? | ✗ | ~ | ~ |
| [P9 Debate + transparency tools](papers/hubinger-2020-eleven-proposals.md#p9-ai-safety-via-debate-with-transparency-tools-10) | same | judge | ~ | ~ | ~ | ✓ | ✓ |
| **Evaluate against a fixed specification** | | | | | | | |
| [Constitutional AI (Bai 2022)](papers/bai-2022-constitutional-ai.md) | AI critiques and ranks by written principles | constitution author | ~ | ✗ | ✗ | ✓ | ✓ |
| [P7 Narrow reward modeling + transparency](papers/hubinger-2020-eleven-proposals.md#p7-narrow-reward-modeling--transparency-tools-8) | none | evaluator | ~ | ~ | ✗ | ~ | ~ |
| [P1 RL + transparency tools](papers/hubinger-2020-eleven-proposals.md#p1-reinforcement-learning--transparency-tools-2) | none | environment designer | ~ | ~ | ✗ | ~ | ~ |
| **Weak supervises strong** | | | | | | | |
| [Weak-to-strong generalization (Burns 2023)](papers/burns-2023-weak-to-strong.md) | weak labels, strong student | weak supervisor (stand-in) | ~ | ✗ | ~ | ✓ | ~ |
| [Plain dialog baseline (Bowman 2022)](papers/bowman-2022-measuring-progress.md) | none | non-expert with a model assistant | ~ | ? | ✗ | ✓ | ~ |
| **Inspect internals** | | | | | | | |
| [Training against probes (Libon 2026)](papers/libon-2026-training-against-probes.md) | refit probe, train model against it | labels the probe data | ~ | ~ | ✗ | ✓ | ✓ |
| [Attribution graphs (Lindsey 2025)](papers/lindsey-2025-biology-of-llm.md) | none (per prompt) | analyst | n/a | ~ | ✗ | ~ | ✓ |
| [P5 Microscope AI](papers/hubinger-2020-eleven-proposals.md#p5-microscope-ai-6) | none | reads what the model learned | ~ | ~ | ✗ | ~ | ~ |
| **Keep the human out** | | | | | | | |
| [P6 STEM AI](papers/hubinger-2020-eleven-proposals.md#p6-stem-ai-7) | none | mostly absent | ~ | ? | ✗ | ~ | ~ |

**Framing papers** (no row of marks; they supply definitions and failure modes):
- [Concrete Problems in AI Safety (Amodei 2016)](papers/amodei-2016-concrete-problems.md): the original statement of scalable oversight and reward hacking. Feeds C1a, C2c, C2d.
- [Risks from Learned Optimization (Hubinger 2019)](papers/hubinger-2019-risks-from-learned-optimization.md): the outer/inner split, mesa-optimizers, and the conditions for deceptive alignment. Feeds C1b, C2d.
- [When AI builds itself (Anthropic 2026)](papers/anthropic-2026-when-ai-builds-itself.md): recursive self-improvement as the real-world recursion, with human review as the bottleneck. Feeds C2c, C2d, C3c.

**Where the summaries disagree:**
- *Debate's inner alignment.* Irving et al. get ✗ (a known failure, not addressed); the Barnes summary gets ? because the post is silent on it; Hubinger's P9 gets ~ because he adds transparency tools. All three are correct under the criterion; they grade different versions.
- *How a ? combines into C2.* The amplification and RRM summaries both have C2c at ? and report C2 as ~. Recursive summarization has C2c at a measured ✗, so C2 is ✗ under the strict rule, although its summary argues the failure may come from a weak policy. Criterion 1.0 doesn't say how ? combines; this is [proposed amendment 2](criterion.md#proposed-amendments).
- *What the invariant is.* The amplification summary needs a whole selection function, the RRM and debate summaries a smaller judgment, and the Hubinger summary only a basin. That ordering is synthesis point 1; it hasn't been settled.

---

## Paper summaries (Layer 1)

| Paper | Role | Summary |
|---|---|---|
| Amodei et al., *Concrete Problems in AI Safety* (2016) | framework | [amodei-2016-concrete-problems.md](papers/amodei-2016-concrete-problems.md) |
| Irving, Christiano & Amodei, *AI Safety via Debate* (2018) | method | [irving-2018-debate.md](papers/irving-2018-debate.md) |
| Christiano, Shlegeris & Amodei, *Supervising strong learners by amplifying weak experts* (2018) | method | [christiano-2018-amplification.md](papers/christiano-2018-amplification.md) |
| Leike et al., *Scalable agent alignment via reward modeling* (2018) | method | [leike-2018-reward-modeling.md](papers/leike-2018-reward-modeling.md) |
| Hubinger et al., *Risks from Learned Optimization* (2019) | framework | [hubinger-2019-risks-from-learned-optimization.md](papers/hubinger-2019-risks-from-learned-optimization.md) |
| Hubinger, *An overview of 11 proposals for building safe advanced AI* (2020) | framework and method family | [hubinger-2020-eleven-proposals.md](papers/hubinger-2020-eleven-proposals.md) |
| Barnes, *Debate update: Obfuscated arguments problem* (2020) | evaluation | [barnes-2020-obfuscated-arguments.md](papers/barnes-2020-obfuscated-arguments.md) |
| Wu et al., *Recursively Summarizing Books with Human Feedback* (2021) | method | [wu-2021-recursive-summarization.md](papers/wu-2021-recursive-summarization.md) |
| Bowman et al., *Measuring Progress on Scalable Oversight for LLMs* (2022) | evaluation | [bowman-2022-measuring-progress.md](papers/bowman-2022-measuring-progress.md) |
| Bai et al., *Constitutional AI: Harmlessness from AI Feedback* (2022) | method | [bai-2022-constitutional-ai.md](papers/bai-2022-constitutional-ai.md) |
| Burns et al., *Weak-to-Strong Generalization* (2023) | method and evaluation | [burns-2023-weak-to-strong.md](papers/burns-2023-weak-to-strong.md) |
| Lindsey et al., *On the Biology of a Large Language Model* (2025) | framework (transparency) | [lindsey-2025-biology-of-llm.md](papers/lindsey-2025-biology-of-llm.md) |
| Anthropic Institute, *When AI builds itself* (2026) | motivation | [anthropic-2026-when-ai-builds-itself.md](papers/anthropic-2026-when-ai-builds-itself.md) |
| Libon et al., *Alignment via Training Against Probes Without Losing Monitorability* (2026) | method | [libon-2026-training-against-probes.md](papers/libon-2026-training-against-probes.md) |

**Scope.** These are the papers on the README's Papers list, the papers quoted in the Discussion 2 primer, Hubinger's post, the club's earlier reading on attribution graphs, and the new probe-training paper. Non-method links from the chat (Bengio's *How Rogue AIs may Arise*, the forecasts) are left out.

**Sources not fully read.** Each summary says what it could open. The Libon et al. summary was rebuilt from the full PDF Evan supplied. The main gaps: only part of Lindsey et al. was readable (up to the jailbreak case study, without the hidden-goals, limitations and discussion sections); and Christiano's corrigibility and HCH posts weren't read, so the basin argument in synthesis point 1 is our inference. All quotes came through a text-extraction tool, so check one against the PDF before putting it on a slide.

**Adding a method:** follow [Rule 5](criterion.md#rule-5-adding-a-method). Write its summary, copy its verdict line here, re-check the synthesis, and log anything the criterion doesn't catch as a proposed amendment.
