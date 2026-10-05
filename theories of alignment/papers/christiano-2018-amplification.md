# Iterated Amplification (Christiano et al., 2018)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Paul Christiano, Buck Shlegeris and Dario Amodei, *Supervising strong learners by amplifying weak experts* (2018). [arXiv:1810.08575](https://arxiv.org/abs/1810.08575).
**Role:** method.
**Criterion version:** 1.0.
**Sources read:** the full paper via the arXiv PDF (sections 1 to 6; appendices only skimmed through their headings). Quotes below were checked against that text.
**Not read:** Christiano's blog posts on corrigibility and HCH. Anything we say about them is marked as secondary and carries no quote.
**Hubinger mapping:** the amplification family is proposals 2, 3, 4, 10 and 11 in *An overview of 11 proposals* (2020). This paper is the base method those proposals add oversight or RL to; it is closest to proposal 2 without the intermittent oversight.

---

## The method in one paragraph

The paper's own statement (abstract): Iterated Amplification is "an alternative training strategy which progressively builds up a training signal for difficult problems by combining solutions to easier subproblems." A human H answers a question by splitting it into subquestions and asking copies of the current model X to answer them: "Amplify<sup>H</sup>(X) answers a question Q by having H identify a sequence of useful subquestions, using X to compute a subanswer to each subquestion" (§1.1). X is then trained by supervised learning to predict what this composite system would say: "X is an autoregressive model trained to predict Amplify<sup>H</sup>(X)'s output" (§1.1). To save human time, a learned "human predictor" H′ stands in for H (§2.1). Repeating the loop makes the overseer grow with the model: "At each point in training, Amplify<sup>H</sup>(X) is modestly smarter than X working on its own, and X chases this moving target" (§2.3). The experiments use five toy algorithmic tasks with a hand-coded H (§4.1).

---

## C1. Alignment

### C1a. Outer alignment: **~**
*Load-bearing assumption:* the answers a human would give when coordinating many copies of X (in the limit, an "exponentially large team of copies of H") are answers we would endorse.

*Steelman.* The paper is explicitly motivated by outer alignment. It argues that the usual fallback, a short-term proxy, is what goes wrong:

> "aggressively optimizing that proxy can lead to pathological behavior ... an example of Goodhart's Law. For example, we might find that user-reported satisfaction ... is a good proxy for long-term benefit to society ... but if we maximize it with RL our agent may maintain fraudulent appearances or effectively manipulate users into providing high ratings." (§1)

In the version tested, X is trained by imitation, not by maximizing a score, so there is no reward for it to game. The target is what the amplified human *would answer*, and the intended fixed point is "an agent that 'approximates' the behavior of an exponentially large team of copies of H" (§2.3).

*Critique.* The objective rewards matching what H (or H′) would say given X's subanswers. That is the overseer's judgment, not the truth, so the method is outer aligned only if that team of copies of H is. The authors also expect the practical version to drop the imitation property: "In many important applications we suspect that we would learn a reward function from Amplify<sup>H</sup>(X) and then train X to maximize that reward function" (§4.3). That reintroduces a learned proxy under optimization pressure, which is the problem §1 set out to avoid.

*Our read:* ✓ for the pure imitation version under the assumption above; ~ overall because the paper's own expected deployment form is a learned reward.

### C1b. Inner alignment: **✗**
*Load-bearing assumption:* that a distilled X trained to predict Amplify<sup>H</sup>(X) on the training distribution actually computes something like that tree, rather than a different procedure that agrees with it only on the training distribution.

*Steelman.* Supervised imitation gives a dense, per-question training signal, and the paper notes X is "trained exclusively to solve the problem they are given" (§5). There is no outer reward that a learned optimizer would be tempted to seize.

*Critique.* The paper does not discuss mesa-optimization or deception, and it states plainly that the learned model need not resemble the process that trained it:

> "The hierarchical decomposition itself is discarded as an artifact of training, and the actual procedure learned by the agent will generally not mirror the structure used in training." (§2.3)

So whatever safety argument rests on the decomposition being human-checkable does not transfer automatically to X. Nothing in the paper checks X's internals. Hubinger's variants exist precisely to fill this gap (intermittent oversight in proposal 2, relaxed adversarial training in 3, 4, 10, 11).

*Our read:* a known failure mode (a distilled model that agrees with the tree on-distribution while doing something else) applies and the paper does not address it.

### C1c. Where the human sits
Decomposer and recombiner inside the training loop. H chooses subquestions and combines subanswers; in practice a learned imitation H′ of H does this (§2.1). In the experiments the human is replaced entirely: "Rather than having a human perform the decomposition, we provide a hard-coded algorithm H" (§4.1).

---

## C2. Scale invariance

### C2a. The repeated step
**Decompose, recombine, distill.** The same local operation runs at every level: H splits a question, copies of the current X answer the pieces, H combines them, and X is trained on the result (§1.1, §2.3). The paper describes capability growing by repeating this step: "Once X is able to provide simple answers, the human is able to provide slightly better answers by breaking them into simple pieces. Then X learns to provide slightly better answers" (§2.3).

### C2b. What must stay invariant: **~**
*Load-bearing assumption:* H's way of choosing useful subquestions and combining subanswers is one fixed function that remains good on questions far from the ones H was calibrated on.

*Steelman (Evan's reading).* This paper makes the invariant unusually concrete. The thing that stays fixed across levels is H, and the authors literally train a model of it: "we train H′ to imitate the role of H when computing Amplify<sup>H</sup>(X)" (§2.1). They argue that this function is small and learnable: "Because H′ is only learning how to identify subquestions and combine subanswers, rather than solving an entire task, we expect to train it with much less data" (§2.1). The key assumption is stated at the level of that function, not of any particular task:

> "The key assumption underlying Iterated Amplification is that a human can coordinate multiple copies of X to perform better than a single copy of X." (§5)

The decomposition need not be clever, only helpful: it "need not be an efficient decomposition in order to be suitable for Iterated Amplification", since "answers to the subquestion just need to help at all on the original task" (§4.2; wording checked, section *(unverified)*). The bar is low, which is what makes an invariant human function plausible. This is the paper's version of what later writing calls the factored cognition hypothesis; the paper does not use that phrase or the term HCH, but §2.3's "exponentially large team of copies of H" is the same object.

*Critique (the club's counterpoint).* Human judgment is noisy and shifts with framing, and the questions at level k are not the questions H′ was fitted on. The paper concedes both halves of this:
- The inputs to H drift: "H′ needs to predict how H will respond to subanswers provided by X. Because X is changing, this distribution is non-stationary, and so we need to continuously update H′ throughout the training process" (§2.1). If H were already an invariant function on all inputs, continual refitting would be unnecessary.
- Coverage is an assumption, not a result: "the question distribution D needs to be broad enough to cover not only the questions we care about, but also all of the subquestions asked during the computation of Amplify<sup>H</sup>(X)" (§4.3).
- The evidence that real humans can do this is an analogy: "human organizations are often able to significantly outperform individual humans in many domains, supporting our key assumption" (§5).

*The weaker bet.* Christiano's later writing (secondary, not read for this summary) suggests the real requirement may be narrower than "H is a fixed function that picks good sub-solutions." It may only need H to reliably say "this step is acceptable and not deceptive", with corrigibility as a basin of attraction that small errors fall back into. *Our read:* this paper does not make that argument. Its stated assumption (§5) is about capability ("perform better than a single copy"), not about acceptability. So the paper commits to the stronger invariant, and the corrigibility-basin version would need its own source.

### C2c. Error behavior across levels: **?**
*Load-bearing assumption:* small imitation errors in X at level k do not get amplified by H's recombination at level k+1.

The paper does not bound or measure error growth across rounds. Its description is conditional: "If all goes well, at the end of this process we're left with an agent that 'approximates' the behavior of an exponentially large team of copies of H" (§2.3). The toy results ("Iterated Amplification is able to solve these tasks effectively with at worst a modest slowdown", §4.2) show the process converging in one setting where H is exact, which says little about how noisy human errors propagate. *Our read:* unaddressed.

### C2d. Phase transitions: **~**
*Load-bearing assumption:* that the imitation (supervised) form is kept, so X is never directly rewarded for persuading H.

- **Toward an RL objective.** The authors expect to switch to learning a reward from Amplify<sup>H</sup>(X) and maximizing it (§4.3). That is a change of kind: once X optimizes a learned judgment of H, it is rewarded for outputs H approves of, and the §1 manipulation worry applies inside the loop.
- **Subanswers H cannot read.** Once X's subanswers are themselves beyond H, H can only combine them, not check them. The paper's own target is tasks "beyond human scale" (§5), which is exactly where this applies.
- **Learned procedure diverges from the tree** (§2.3, quoted under C1b). At some capability X could model H′ well enough that matching it is easier than answering.

*Our read:* the imitation version avoids the most obvious transition (optimizing against the overseer), but the paper names a deployment path that crosses it, and does not discuss the others.

### C2e. Exact or statistical
**Statistical, and the experiment makes the invariance exact by fiat.** There is no proof. The only evidence is five tasks at one size (64 elements; §4.1, Appendix C). Because H is a hard-coded algorithm, H is perfectly invariant across levels in the experiments, so they cannot test C2b at all. The authors say so: "These experiments don't shed any light on whether humans can decompose interesting real world tasks, nor on whether it would be feasible to learn messy real world decompositions" (§4.3).

### C2f. Measurability
In principle yes: each task has a size parameter and a known algorithmic answer, so one could vary depth or size and plot accuracy against the number of amplification rounds. The paper does not report such a sweep. A sandwiching version would replace the hard-coded H with real humans on tasks where ground truth is known.

### C2 verdict: **~**
The weakest of C2b to C2d is C2c (**?**), with C2b and C2d at **~**; the evidence is statistical and from one scale, which caps the mark at **~** in any case. *Our read:* we report **~** rather than **?** because the paper does give an argument (§5) for the invariant step, just not for error behavior. See the candidate amendment on how **?** combines.

---

## C3. Competitiveness

### C3a. Training competitiveness: **~**
*Load-bearing assumption:* that learning H′ on realistic decompositions stays as cheap relative to direct supervision as it was on toy tasks.

*Steelman.* "Supervised learning required tens of millions of examples in order to learn these algorithms. ... In contrast, Iterated Amplification required tens of thousands of examples in order to learn the much simpler decompositions" (§4.2), and the method reached its results "with at worst a modest slowdown" (§4.2). The authors argue the advantage should persist "since performing decomposition is strictly easier than solving the task without the aid of X" (§4.2).

*Critique.* The same authors hedge: "the sample complexity will likely be much larger for more realistic tasks, where modeling H might require (for example) a high-quality language model" (Table 2 caption). H′ must also be refit continuously (§2.1), and every training example for X requires several calls to X.

### C3b. Performance competitiveness: **~**
*Load-bearing assumption:* that imitating an amplified human is enough capability for the use cases that matter, or that the RL variant can be added without losing alignment.

A pure imitator is bounded by what the amplified team would answer, and the method is framed around question answering. The authors themselves expect to move to reward maximization (§4.3), and Hubinger's proposals 10 and 11 add an RL objective for exactly this reason. *Our read:* competitive on reasoning-heavy questions if the key assumption holds; unclear for agentic, fast or open-ended tasks.

### C3c. Alignment tax trend
*Our read:* the per-round human cost could stay flat if H′ generalizes, since H′ only ever learns the small decomposition step. But the paper also expects sample complexity to rise on realistic tasks (Table 2 caption) and needs a question distribution broad enough to cover every subquestion (§4.3), both of which plausibly grow with the difficulty of the target task. Not measured.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| Amplification (Christiano 2018) | ~ | ✗ | ~ | ~ | ~ |

---

## Open questions for the comparison

1. **Is H a fixed function?** §2.1 trains H′ as an explicit model of human judgment but must keep refitting it because its inputs drift. Is that drift a sign that the invariant fails, or just ordinary non-stationary training? This is the same question RRM raises about the user's evaluation (see [Leike et al.](leike-2018-reward-modeling.md)); Rule 2 says keep both readings.
2. **Capability invariant or acceptability invariant?** The paper's key assumption (§5) is about doing better than one copy. The corrigibility-basin argument is about every step being acceptable. Which one do Hubinger's proposals 2, 3, 4, 10, 11 actually need?
3. **Imitation vs reward.** The safety story of §1 leans on not optimizing a proxy, but §4.3 expects to learn a reward from the amplified human. Does the amplification family survive that switch, or does it become RRM?
4. **Exact H, noisy H.** What happens to the toy results when H is replaced by a noisy or framing-sensitive judge? This is the cheapest experiment that would turn C2c from **?** into a measurement.
5. **Process vs product.** §2.3 says the distilled X does not mirror the decomposition. Does any later amplification work (or interpretability tool) check whether it does?
