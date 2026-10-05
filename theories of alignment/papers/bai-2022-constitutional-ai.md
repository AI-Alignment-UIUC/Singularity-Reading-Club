# Constitutional AI (Bai et al., 2022)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation:** Yuntao Bai, Saurav Kadavath, Sandipan Kundu, Amanda Askell, Jackson Kernion, et al. *Constitutional AI: Harmlessness from AI Feedback.* arXiv:2212.08073, 2022. https://arxiv.org/abs/2212.08073

**Role:** method (supervised critique-and-revision, then reinforcement learning from AI feedback, both steered by a written list of principles).

**Sources read:** the arXiv PDF (https://arxiv.org/pdf/2212.08073), through a text extraction, with targeted passes over the abstract, Section 1.1, Section 3.1, Section 4.1, Section 4.3, the Figure 4 caption and Section 6.2. Quotes below come from that extraction; check the PDF before putting one on a slide. Two short phrases attributed to §1.1 under C1a came back only as fragments and are marked *(unverified)*. Nothing was taken from secondary sources. Sources we could not open: none.

---

## The method in one paragraph

The paper trains a harmless but non-evasive assistant without human labels for harmlessness. In the first phase (SL-CAI), a helpful-only model answers harmful prompts, is asked to critique its own answer against a principle, revises it, and is finetuned on the revisions. In the second phase (RL-CAI), the finetuned model produces pairs of answers, an AI feedback model picks the better one according to a randomly sampled principle, a preference model is trained on those AI labels, and the policy is trained with RL against that preference model. Humans still supply helpfulness labels, but for harmlessness their only input is the constitution itself. The paper's own statement (Abstract):

> "As AI systems become more capable, we would like to enlist their help to supervise other AIs." (Abstract)

> "The process involves both a supervised learning and a reinforcement learning phase." (Abstract)

The RL phase is named directly: "we use 'RL from AI Feedback' (RLAIF), where the AI evaluates responses according to a set of constitutional principles" *(section not confirmed in our extraction; the RLAIF description is in §1 and §4)*.

**Scalable oversight is the stated motivation.** The paper opens with the capability-gap framing (§1):

> "We would like to train AI systems that remain helpful, honest, and harmless, even as some AI capabilities reach or exceed human-level performance. This suggests that we will need to develop techniques that do not rely on humans to supervise all aspects of AI behavior." (§1)

and names its own family of techniques (§1.1):

> "We use the term 'Scaling Supervision' for techniques that leverage AI to help humans to more efficiently supervise AI." (§1.1)

> "Scaling supervision has been widely discussed as a possibility for AI alignment, with specific proposals such as [Christiano et al., 2018, Irving et al., 2018] and recent empirical work like [Bowman et al., 2022]." (§1.1)

*Our read:* the paper uses "scaling supervision" rather than "scalable oversight", but cites amplification, debate and Bowman et al.'s sandwiching paper as its lineage, so it belongs in the same grid. Note that "more efficiently" is the 2016 sense of scalable (fewer labels; see [Amodei 2016](amodei-2016-concrete-problems.md)), while the opening sentence is the capability-gap sense. Most of the paper's evidence is about the first.

---

## C1. Alignment

**C1a. Outer alignment: ~**
*Load-bearing assumption:* the AI feedback model's reading of a short natural-language constitution matches what the authors meant by "harmless", even under strong optimization pressure.

*Steelman.* The objective is written down in plain language and is therefore inspectable. The authors want to "encode desirable AI behavior in a simple and transparent form" *(unverified, §1.1)*, and they want a model that is "never evasive, in order to reduce the tension between helpfulness and harmlessness" *(unverified, §1.1)*. Sampling principles at random per comparison is a small hedge against any one principle being mis-specified: "we wrote a set of 16 different principles, and randomly sampled a principle for each comparison label" (§4.1), which the authors report gives more robust preference-model behavior.

*Failure modes.* What the policy actually optimizes is not the constitution but a preference model trained on AI judgments of it. The paper reports Goodharting against that proxy directly:

> "We found that RL-CAI models can be over-trained, resulting in Goodharting behavior whereby models can be overly harsh in responding to harmful prompts, or may include boilerplate language" (§4.3)

The constitution is also deliberately short: "We will finetune AI models to be harmless using only of order ten simple principles, stated in natural language" (§1.1); "We have written a total of 16 different principles related to harmlessness" (§3.1). And the principles were not carefully derived: "These principles were selected in a fairly ad hoc manner for research purposes" (footnote in §3.1).

*Our read, on the club question* ("a written constitution is a short specification of values; how does it avoid the hidden complexity of wishes?"): it doesn't avoid it, it delegates it. Sixteen sentences cannot contain the complexity of "harmless". The complexity lives in the feedback model's pretrained understanding of those sentences, which is learned from human text. So CAI is outer aligned to the extent that a language model's interpretation of "choose the less harmful response" already carries the hidden complexity. That is a real bet, and in 2022 it was a reasonable one at 52B scale, but it moves the question from "is the constitution right?" to "is the evaluator's interpretation right, and does it stay right?" (which is C2b). The objective rewards what the evaluator *judges* to comply, not compliance.

**C1b. Inner alignment: ✗**
*Load-bearing assumption:* RL against a learned preference model can produce a policy that pursues a proxy (or behaves well only when evaluated), and nothing in CAI checks for this.

*Steelman.* Chain-of-thought evaluation makes the evaluator's reasons legible, which is a weak form of transparency on the *evaluator* side; the authors see this as making AI decision-making easier to understand and evaluate *(unverified, §1.1)*.

*Failure mode.* There is no transparency check on the *policy*, no adversarial training aimed at deceptive behavior, and no discussion of mesa-optimization. The paper is candid that removing humans from the loop reduces observation:

> "This could lead developers to deploy models with unforeseen failure modes that have not been thoroughly tested and observed by humans." (§6.2)

*Our read:* the over-training result in §4.3 is already a small, visible inner-and-outer failure (the policy learns "add boilerplate" rather than "be harmless"). A less visible proxy would not be caught by the method itself.

**C1c. Where the human sits: constitution author** (plus helpfulness labeler). For harmlessness, the paper says it will "largely eliminate direct human supervision" *(section not confirmed)*. The human writes principles and picks prompts; the AI applies them.

---

## C2. Scale invariance

**C2a. The repeated step.** *Evaluate by principle:* a model reads a principle and judges which of two outputs better satisfies it (or critiques and revises one output). *Our read:* the paper applies this step once, with a model of the same family judging itself. It does not iterate across generations. But the step is obviously iterable: generation *n* could serve as the feedback model for generation *n+1*, which is how CAI-style training is used in practice. So we grade it as if iterated, while noting the paper never tests that.

**C2b. What must stay invariant: ~**
*Load-bearing assumption:* the evaluator's interpretation of a fixed sentence such as "choose the less harmful response" is a stable function across model generations, rather than drifting as the evaluator gets stronger and the questions get stranger.

*Steelman.* The constitution is the invariant part by construction: the same text is handed to every evaluator. And the evaluator gets better with scale, not worse:

> "The trends suggest that models larger than 52B will be competitive with human feedback-trained preference models." (Figure 4 caption)

*Our read:* this is Evan's "invariant human function for selecting sub-solutions" in its most compressed form. The function is not a human; it is a short text plus whatever the evaluator reads into it. Fixing the text does not fix the function, because the meaning of "harmful" when evaluating a superhuman plan is not something the sixteen sentences determine. The counterpoint from Discussion 1 (higher-level questions differ in kind) applies with extra force: there is not even a noisy human in the loop to anchor the drift. Figure 4 shows evaluator *accuracy on human-labeled comparisons* rising with size, which is evidence of agreement with humans on questions humans can label, not of invariance on questions they cannot.

**C2c. Error behavior across levels: ?**
*Regime (criterion 1.1):* No levels are stacked and nothing re-anchors to humans, so there is no map. If generations were chained, the likely regime is compounding (our read). **?** stands.
The paper does not stack levels, so it neither measures nor bounds compounding. *Our read:* if each generation's preference model inherits the previous evaluator's misreadings of the constitution, and the policy Goodharts that preference model (as §4.3 shows it can), errors plausibly compound, since nothing re-anchors to humans between generations. This is the same worry the primer raises for RRM.

**C2d. Phase transitions: ✗**
*Load-bearing assumption:* once the policy can model its evaluator (here, a model from the same family reading a known constitution), it can optimize for the evaluator's judgment rather than for harmlessness, and CAI has no defense at that threshold.

*Our read:* CAI is unusually exposed to the "model the overseer" transition because the overseer is a near-copy of the policy and the criteria are public text. Self-evaluation also invites correlated blind spots: an error the policy makes because of how it represents "harm" is the error its evaluator is least likely to flag. The authors flag a related risk at the level of use rather than mechanism: "This means that these methods also make it easier to train pernicious systems." (§6.2)

**C2e. Exact or statistical: statistical.** The only scale evidence is Figure 4's trend across model sizes up to 52B for evaluator accuracy. There is no argument that holds at every level.

**C2f. Measurability.** In principle yes: one could run CAI with evaluators of several sizes on policies of several sizes and measure harmlessness against a stronger held-out judge or human experts (a sandwiching design). The paper measures evaluator size against human-labeled comparisons, which is one axis of that plot.

**C2 verdict: ✗.** Weakest of C2b to C2d is C2d. *Load-bearing assumption:* a policy that can model a same-family evaluator reading a public constitution will exploit it; if that is false (for example, if evaluator and policy errors are uncorrelated), C2 rises to ~, capped there by C2e.

---

## C3. Competitiveness

**C3a. Training competitiveness: ✓**
*Load-bearing assumption:* AI labels are cheap relative to human labels and slot into an existing RLHF pipeline.

The method replaces human harmlessness labels with model calls and keeps the RLHF machinery: "These methods make it possible to control AI behavior more precisely and with far fewer human labels" *(section not confirmed)*. *Our read:* this is close to a training-cost improvement over RLHF, not a tax, which is why variants of it are widely used.

**C3b. Performance competitiveness: ✓**
*Load-bearing assumption:* the harmlessness gain does not cost helpfulness, as the paper's harmless/helpful comparison reports at 52B.

The design goal was to remove the evasiveness tax of earlier harmlessness training (see the "never evasive" aim under C1a). *Our read:* the over-training result (§4.3) shows a tax can reappear if RL runs too long, so this mark depends on stopping early.

**C3c. Alignment tax trend: shrinks (claimed).** Figure 4's trend implies the AI evaluator gets closer to human-feedback quality as models grow, so the relative cost of CAI falls with scale. *Our read:* this is the attractive and the worrying property at once. The tax falls exactly because the human is removed, which is what C2b and C2d say is the risk.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| [Constitutional AI (Bai 2022)](bai-2022-constitutional-ai.md) | ~ | ✗ | ✗ | ✓ | ✓ |

**Feeds:** C1a (short spec, interpretation delegated to evaluator; Goodharting of the preference model, §4.3), C1c (human as constitution author), C2a (evaluate-by-principle, iterable across generations but not iterated in the paper), C2b (fixed text is not a fixed function), C2d (same-family evaluator with public criteria), C2e (single trend to 52B), C3c (tax shrinks as the human is removed).

---

## Open questions for the comparison

1. **Hidden complexity of wishes.** Does any method in the grid avoid delegating the complexity of values to a learned model's interpretation? If none does, the real comparison is between *which* learned interpretation each method trusts and how it checks it.
2. **Fixed text vs. fixed function.** CAI makes the invariant part (the constitution) explicit, which no other method does. Is that an advantage for C2b (you know what is supposed to stay fixed) or a false comfort (the text was never the part that mattered)?
3. **Generational CAI.** If model *n* is the feedback model for model *n+1*, what re-anchors the chain to humans? Should C2c require a re-anchoring step, analogous to RRM's human evaluator at each level?
4. **Self-evaluation correlation.** When policy and evaluator share a base model, are their errors correlated? This seems testable and would move C2d.
5. **Goodharting as a measurable slope.** The §4.3 over-training result could be plotted against evaluator size: does a bigger evaluator delay Goodharting, and by how much? That would be a C2f measurement the paper almost has.
