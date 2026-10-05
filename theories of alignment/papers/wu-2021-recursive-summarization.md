# Recursive Summarization (Wu et al., 2021)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Jeff Wu, Long Ouyang, Daniel M. Ziegler, Nisan Stiennon, Ryan Lowe, Jan Leike, Paul Christiano. *Recursively Summarizing Books with Human Feedback.* 2021. [arXiv:2109.10862](https://arxiv.org/abs/2109.10862).

**Role:** *method* (recursive task decomposition plus RLHF; a concrete instance of decomposition-based scalable oversight).

**Sources read.** The arXiv PDF, read through a text extraction tool (WebFetch) with targeted prompts. Direct download of the PDF was blocked, so we could not read the raw text end to end. Quotes below come from that extraction with the section it reported. Quotes containing "..." were shortened by the extraction, not by us; check them against the PDF before putting one on a slide. Nothing here is taken from secondary sources.

**Criterion version.** 1.0.

**Self-reference.** The step rules in our [criterion](../criterion.md) (Part 2) and the club's [recursive summary](../../discussions/recursive-summary.md) are both modeled on this paper, so whatever we conclude about its C2 also applies to how this folder is built.

---

## The method in one paragraph

The authors' own statement (Abstract): "Our method combines learning from human feedback with recursive task decomposition: we use models trained on smaller parts of the task to assist humans in giving feedback on the broader task." A book is split into chunks. The model summarizes each chunk, then summarizes concatenations of those summaries, and so on up the tree until one summary covers the whole book. The decomposition itself is done by a fixed algorithm, so only the "summarize this input" step is learned (§2.1: "the decomposition operation can be performed algorithmically, and the ML model only needs to be trained on the Respond operation"). Training is the standard RLHF pipeline applied to every node: behavioral cloning on labeler demonstrations, then "many iterations of reward learning and reinforcement learning" from labeler comparisons (§2.3). Labelers at a higher node judge a summary against the lower-level summaries it was built from, not against the book.

---

## C1. Alignment

### C1a. Outer alignment: **~**
*Load-bearing assumption:* a summary that labelers rate as faithful to its direct input, at every node, composes into a summary faithful to the book.

*Steelman.* The objective is ordinary human preference over summaries, and decomposition makes each preference judgment one a human can actually make. The motivation is stated as alignment (§6): "we want to empower humans to give feedback to models on tasks that are very difficult to evaluate. We expect this to be a critical part of the alignment problem because we need to make sure humans can communicate their values to AI systems."

*The gap.* What is rewarded at a higher node is faithfulness to the lower summaries, not to the book. Labelers rate "only the quality of the summary with respect to the direct input to the model, rather than the subset of the book representing the true summarization target" (Appendix A.3). So the objective at depth *k* is "agree with depth *k − 1*." Information that the lower layers dropped or invented is invisible to the top-level reward. *Our read:* this is a well-defined and mostly benign objective for summarizing fiction, but it rewards what the overseer can see from the layer below, which is exactly the "believes true vs. is true" gap C1a asks about.

### C1b. Inner alignment: **?**
*Load-bearing assumption:* models at this scale are not modeling their labelers well enough to optimize against them.

The paper does not discuss deceptive alignment, mesa-optimization or reward hacking (our targeted search found no such passage). Its one inner-alignment-adjacent point is that decomposition aids inspection (§2.4): "It makes it easier to trace what the model is thinking, and debug errors." *Our read:* traceability is a transparency property of the output tree, not of the model's cognition, so it gives at most weak help against a deceptive policy. We mark **?** because the source is silent.

### C1c. Where the human sits
Demonstrator (behavioral cloning data) and evaluator (comparisons for the reward model) at every node, judging each summary against its direct input only. The human never needs to read the whole book; that is the point.

---

## C2. Scale invariance

### C2a. The repeated step
**Summarize and check.** The same learned "Respond" operation summarizes a leaf chunk or a concatenation of summaries, and the same kind of human comparison checks it against its input. This is the most literal repeated step in our set: one function, one evaluation protocol, applied at every height of the tree.

### C2b. What must stay invariant: **~**
*Load-bearing assumption:* the human judgment "is this a good summary of this input?" is the same function whether the input is raw prose or a summary of summaries.

*Steelman.* The paper shows the learned step transfers up the tree. With a curriculum that starts on leaves (§2.3.2: "For early rounds, we initially train only on the first leaves...We then move to the entire first subtree...at this point, our model is already capable of generalizing to the full tree"), and (§4.1.2) "training on the first subtree does comparably to training on the full tree." That is evidence that one step trained at low levels keeps working higher up.

*The counterpoint.* Higher-level inputs are a different kind of text. The authors note that "Inputs produced by itself are outside of the training distribution, thus causing auto-induced distributional shift" (§2.3.1). The judgment also differs in kind: deciding what matters across a whole book is not the same task as compressing a page. Human judgment is also noisy at the leaf level already: "Labeler agreement for relative quality of model-written summaries was nearly 80%" (§4.1.1). *Our read:* this is Evan's "invariant human function for selecting sub-solutions" in its cleanest form, and the paper gives partial evidence for it (transfer works) and partial evidence against it (distribution shift, about one in five comparisons disputed).

### C2c. Error behavior across levels: **✗**
*Regime (criterion 1.1):* **Compounding.** Measured across tree depths. There is no correcting step, since a higher node cannot recover what a lower node dropped. In Williamson's terms, fidelity falls like αⁿ with depth. **✗** stands.
*Load-bearing assumption:* errors at each level are not caught or corrected by the level above.

This is the one sub-criterion the paper measures directly, and the answer is compounding. §4.1.2: "Likert scores for the full book summaries were significantly lower than Likert scores of any of the individual decomposed tasks. This is unsurprising, since the errors accumulated at each depth are all reflected in the full book summary score." §6.1: "Policy errors at lower levels compound at each composition task, ultimately leading to large errors." The protocol has no mechanism for a higher node to recover information a lower node dropped, since its labeler sees only the lower summaries (Appendix A.3). So a top-level evaluator does in practice depend on the lower layers being right: checking the top against the layer below is cheap, but checking it against the book requires reading the book.

The end result: "over 5% of summaries from the best 175B model were given a score of 6 out of 7, and over 15% were given a 5 out of 7" (§4.1.2). *Our read:* the paper is honest that the full-book result is far from human quality, and attributes part of the gap to compounding.

### C2d. Phase transitions: **~**
*Load-bearing assumption:* the important content of the task is local enough to survive chunking.

The paper names a structural limit rather than a capability threshold. §6.1: "Task decomposition assumes that separate parts of the task can be completed independently. However, this may not be true for summarizing books." And: "Consider a case where important information is sprinkled lightly across many parts of the book...Determining the kinds of tasks that are amenable to decomposition remains an open problem." It also reports a qualitative failure at the top of the tree: book summaries "often read more as a list of events from the book, rather than a coherent summary that a human would write" (§6.1). *Our read:* this is a phase transition in task type, not model capability. Global properties (a theme, a slow plot twist, a subtle lie spread across chapters) are the summarization analogue of obfuscated arguments: no single checkable node contains them. The capability-driven transitions (a policy that models its labelers) are not discussed.

### C2e. Exact or statistical
Statistical. The evidence is an empirical trend over one task, one fixed decomposition and two model sizes (6B and 175B). There is no argument that the step preserves quality at every level, and the measured result is the opposite (errors compound). So the C2 verdict could be at most **~** even before C2c.

### C2f. Measurability
Partly testable, and the paper supplies a natural axis: tree depth. Scores are reported per depth and for the full book (§4.1.2), so one could plot quality against height. The capability axis is thinner: 175B RL beats BC while "the improvement is smaller for the 6B models" (§4.1.2), which is two points, not a slope. *Our read:* recursive summarization is the easiest method in this folder to put on a box-counting plot, because depth is an explicit, countable scale. Nobody has done it across several model generations.

### C2 verdict: **✗**
The repeated step exists and transfers up the tree (C2a, C2b partial), but errors compound across levels (C2c) and global information falls through the decomposition (C2d). Criterion v1.0 sets C2 to the weakest of C2b to C2d, which is C2c (**✗**). *Our read:* this is harsh, because the compounding was measured on a weak policy with a fixed decomposition, and the paper does not show that compounding is intrinsic to the step. Whether a measured failure on a weak policy should cap a method at **~** instead is listed as a proposed amendment in the [criterion](../criterion.md#proposed-amendments), not applied here.

---

## C3. Competitiveness

### C3a. Training competitiveness: **✓**
*Load-bearing assumption:* the fixed decomposition fits the task, so no extra machinery is needed.

It is standard RLHF on short inputs, which is cheap per label. Appendix E.2: "It took over 12 hours on average for a labeler to read a full book, and additionally over 1 hour to write the summary. This is over 50 times longer than it takes labelers to do a single decomposed summarization task." The extra cost is more model calls (one per node), which scales with book length, not with anything exotic.

### C3b. Performance competitiveness: **~**
*Load-bearing assumption:* the target task decomposes into nearly independent pieces.

On its benchmark it is strong: "Our 175B models beat all non-oracle baselines on ROUGE by 3-4 points" on BookSum (§4.2). Its advantages include length: "Our procedure generalizes gracefully to longer books...regardless of the length of books in the training dataset" (§2.4). But the authors say the decomposition is fixed and hand-designed (§5: "we assume a fixed decomposition"), and the output lacks global coherence (§6.1). *Our read:* that covers summarization-like tasks but not open-ended agentic work, where nobody knows the decomposition in advance. Whether a learned decomposition is feasible is an open question in the paper itself (§6.2): "Is learning a task decomposition model, rather than using a fixed decomposition, feasible?"

### C3c. Alignment tax trend
*Our read:* roughly flat per node and linear in input length, which is favorable. The hidden tax is quality: compounding errors mean the gap to a non-decomposed (or human-written) summary may grow with depth, and depth grows with the size of the task. The paper does not measure this trend.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| Recursive summarization (Wu 2021) | ~ | ? | ✗ | ✓ | ~ |

---

## Open questions for the comparison

1. **Strict vs. adjusted C2.** C2c is a measured **✗**, so the strict rule gives C2 **✗**. Should a method keep **~** when the failure was measured on a weak policy? The same choice will come up for RRM, where Leike et al. raise error accumulation as an open question.
2. **Does the top need the bottom?** Labelers at depth *k* check only depth *k − 1*. That gives Rule 4's "checkable downward" property, but it also means a top-level check cannot catch an omission made three levels down. Is one-layer checkability enough, or does an oversight tree need occasional spot-checks all the way to Layer 0? (Our criterion's Rule 4 already asks for one quote check per summary; this paper suggests why.)
3. **Global properties.** What is the summarization version of the obfuscated arguments problem, and does debate's adversary help find information "sprinkled lightly across many parts" (§6.1) where a fixed tree cannot?
4. **Learned decomposition.** If the decomposition becomes learned (§6.2), the decomposer joins the set of things that must be aligned, and the method moves closer to iterated amplification. Does it inherit amplification's verdicts?
5. **Self-reference.** This folder is a recursive summary checked by humans one layer at a time. Our own C2c risk is the same one the paper measured: errors in a paper summary propagate into the grid and the synthesis.
