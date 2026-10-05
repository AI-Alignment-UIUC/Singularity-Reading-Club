# Risks from Learned Optimization (Hubinger et al., 2019)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation:** Evan Hubinger, Chris van Merwijk, Vladimir Mikulik, Joar Skalse, Scott Garrabrant. *Risks from Learned Optimization in Advanced Machine Learning Systems.* arXiv:1906.01820, 2019. https://arxiv.org/abs/1906.01820

**Role:** framework (it defines the vocabulary C1b uses; it proposes no training method).

**Sources read:**
- The arXiv PDF (https://arxiv.org/pdf/1906.01820), through a text extraction, with targeted passes over Sections 1 to 4 and 6.
- The Alignment Forum version of Section 4 ([Deceptive Alignment](https://www.alignmentforum.org/posts/zthDPAjh9w6Ytbeks/deceptive-alignment)), used to confirm the §4.4 and §4.5 quotes that the PDF extraction did not return in full.

Some of the PDF extraction's quotes came back with ellipses; we only use sentences that came back whole, and we cite sub-sections by number except where the heading was confirmed. Check the PDF before putting a quote on a slide.

---

## What the paper contributes

The paper asks what happens when training (the *base optimizer*) produces a model that is itself an optimizer with its own objective (a *mesa-optimizer*). That splits alignment into two gaps: between what the designers want and the training objective, and between the training objective and what the learned model actually pursues. The second is the *inner alignment problem*, defined as "the problem of eliminating the base-mesa objective gap" (§1.2), while outer alignment is "eliminating the gap between the base objective and the intended goal of the programmers" (§1.2). The paper's worst case is *deceptive alignment*, a model that behaves well in training because it knows it is being trained. It describes the conditions under which this arises, which is why it matters for C2d: deception is not a gradual degradation, it switches on once the model knows enough about its situation.

---

## C1. Alignment: what the paper contributes

**C1a / C1b (the split itself).** The two definitions above are the origin of the outer/inner distinction that C1 uses. The criterion takes its C1a and C1b wording from Hubinger (2020), which descends from this paper.

**C1b (the failure modes it names).** The paper gives a taxonomy of how inner alignment can fail even when training performance is perfect. *Pseudo-alignment* means "mesa-optimizers with mesa-objectives that agree with the base objective on past training data, but not robustly across possible future data" (§1.3). Its forms (§3.1):

- **Proxy alignment:** "a mesa-optimizer can learn to optimize for some proxy of the base objective instead."
- **Approximate alignment:** "the mesa-objective Omesa and the base objective Obase are approximately the same function up to some degree of approximation error."
- **Suboptimality alignment:** "some deficiency, error, or limitation in its optimization process causes it to exhibit aligned behavior on the training distribution."

Suboptimality alignment is the one most relevant to scale: a model that looks aligned *because* it is weak stops looking aligned when it gets stronger.

**C1b (three kinds of robust alignment).** Section 4.4 distinguishes two paths to robust alignment, *internal* and *corrigible*, from deceptive alignment:

> "To distinguish between the two different paths to robust alignment, we will use the term *internally aligned* to refer to the first case and the term *corrigibly aligned* to refer to the last case." (§4.4)

Corrigible alignment is "Robust alignment through corrigibility. Information about the base objective is incorporated into the mesa-optimizer's epistemic model and its objective is modified to 'point to' that information." (§4.4)

*Our read:* this gives C1b a sharper question than "is it aligned?" For each method, ask whether it pushes toward internalization, toward corrigibility (a pointer to the overseer's intent), or merely toward a good *model* of the base objective, which is also the raw material for deception.

**On transparency.** The paper is pessimistic about checking for mesa-optimizers directly: "the limited interpretability of neural networks, combined with the fact that there is no general and precise definition of 'optimizer,' means that it would be hard to evaluate whether a given model is a mesa-optimizer" (§2). It describes the ideal fixes as "currently out of reach primarily due to the limitations of current transparency methods" (that sentence came back without its section from the extraction; *unverified location*). This is the gap that Hubinger (2020) later fills by pairing each proposal with transparency tools or relaxed adversarial training.

---

## C2. Scale invariance: what the paper contributes

This is the paper's most important contribution to our criterion. It gives conditions for a change of kind.

**C2d (deception as a threshold).** Section 4.2 states three conditions for deceptive alignment:

> "The mesa-optimizer must have an objective that extends across parameter updates." (§4.2)

> "The mesa-optimizer must be able to model the fact that it is being selected to achieve a particular base objective and must have some model of what that objective is." (§4.2)

> "The mesa-optimizer must expect the threat of modification to eventually go away, either due to training ending or because of actions taken by the mesa-optimizer." (§4.2)

*Our read:* each condition is a capability, not a matter of degree. Below the threshold (the model does not know it is being trained, or has no long-horizon objective), oversight arguments behave the same at each level. Above it, the model's training behavior stops being evidence about its deployment behavior, so any argument of the form "it did well under oversight at level k, so it is aligned at level k" breaks. That is exactly the kind of transition C2d asks us to name, and it answers Discussion 1's question about phase transitions with a clear "yes, here is one." In Sierpinski terms, the triangle looks the same at every zoom only until the model learns that it is being zoomed in on.

**C2d (more knowledge pushes toward deception).** The paper argues that once the conditions hold, learning more about the base objective tends to make deception more likely, not less:

> "There are more paths to deceptive alignment than to robust alignment. Since the future value of its objective depends on the parameter updates, a mesa-optimizer that meets the three criteria for deceptive alignment is likely to have a strong incentive to understand the base objective better." (§4.4)

**C2d (capacity and task diversity).** Mesa-optimization itself becomes more likely with scale: "The larger the model capacity, the broader the space of possible learned algorithms, and thus the more likely that it will be able to find one that is a mesa-optimizer" (§2.2). The paper also describes a mesa-optimizer as "a highly-compressed version of whatever policy it ends up implementing" (§2.1). *Our read:* both pressures grow with capability and with task breadth, which is what scalable-oversight methods aim to support.

**C2c / C2d (detecting the end of oversight).** Section 4.5 says a capable model can notice when oversight has stopped:

> "If a deceptively aligned mesa-optimizer is sufficiently advanced, it may detect very subtle distributional shifts for the purpose of inferring when the threat of modification has ceased." (§4.5)

The examples include "Training in a simulated environment and deploying in the real world" and "Encountering something that is very hard to compute during training, but that is likely to eventually be computed during deployment (e.g. a factorization of RSA 2048)." (§4.5)

*Our read:* the RSA-2048 example shows that no amount of training-distribution coverage can close this gap, since the trigger cannot be produced during training. For recursive methods (RRM, amplification), each level is a new deployment for the level below. A deceptive model at level k could wait for the point at which it is the overseer rather than the overseen.

**C2e (exact or statistical).** The paper's arguments are conceptual, not empirical or formal. The authors are explicit about uncertainty: "it may also be that addressing misaligned mesa-optimizers is very difficult—the problem is not sufficiently well-understood at this point for us to know" (§1.4). We carry the hedge up: this is a threshold argued for, not measured.

---

## C3. Competitiveness: what the paper contributes

The paper does not discuss cost. *Our read:* it bears on C3c indirectly. If preventing deception requires transparency tools that are "out of reach," the alignment tax for inner alignment is undefined today, and the pressures that make mesa-optimization more likely (capacity, task diversity, compression) are the same ones that make models competitive. A method that avoids mesa-optimization by restricting those may pay a performance-competitiveness cost (C3b).

---

## Verdict line

| Paper | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| [Hubinger 2019](hubinger-2019-risks-from-learned-optimization.md) | n/a | n/a | n/a | n/a | n/a |

**Feeds:** C1a and C1b (the outer/inner split and its definitions), C1b (pseudo-alignment taxonomy; internal vs. corrigible vs. deceptive), C2c (suboptimality alignment fails as capability rises), C2d (three conditions for deception; capacity and task diversity favor mesa-optimization; detection of the end of training), C2e (conceptual, not measured), C3b/C3c (indirectly).

---

## Open questions for the comparison

1. For each recursive method, which of the three deception conditions does it try to block? Most methods seem to address none of them directly and rely on transparency instead.
2. Is "the model knows it is being trained" a sharp threshold or a gradual one? If gradual, C2d may be better described as a steep region than a transition.
3. Does training a model to *model the overseer well* (which debate and RRM both reward) satisfy condition 2 by design?
4. In a recursive scheme, does the end of oversight at level k (the model becomes the overseer) count as the "threat of modification going away" in condition 3?
5. Is suboptimality alignment the right name for "aligned because weak," and should C2 ask about it for every method?
