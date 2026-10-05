# Training against probes (Libon et al., 2026)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Lena Libon, Alexander Panfilov, Ben Rank, Xin Chen, Jonas Geiping and Maksym Andriushchenko, "Alignment via Training Against Probes Without Losing Monitorability," arXiv [2609.38645](https://arxiv.org/abs/2609.38645) (v2, 1 Oct 2026). Code: [aisa-group/training_against_probes](https://github.com/aisa-group/training_against_probes).

**Role:** method.

**Sources read (criterion v1.1, Rule 0).** The full v2 PDF, which Evan supplied, read as extracted text: main body, Discussion, and Appendices B (probe analysis), C and E (completion categories, safety evaluations). **Rerun on 2026-10-05 under criterion 1.1**, from the PDF again, with the [scale invariance library](../scale%20invariance%20library/README.md) as a lens. The rerun added Appendix E.3 (harmful fine-tuning) and the retrained-regime results in E.2 and B.4, which the first pass left out; see [What the rerun changed](#what-the-rerun-changed). Quotes below are copied from that text with their section numbers. The figures and tables did not extract cleanly, so numbers are taken from the running text and from Tables 1 and 2 only.

**Models tested.** Mistral 7B Instruct v0.1 and an abliterated Llama 3 8B Instruct (harmfulness); Qwen3-14B, a reasoning model (honesty). The unmodified Llama 3 8B Instruct appears only as a comparison.

---

## The method in one paragraph

Most alignment training scores what a model *outputs*. The authors argue that output-based methods "ask whether a response looks aligned, but never inspect how the model represents the response it is producing" (§1), and that a capable model could pass such checks without changing underneath. The abstract states the bet: "Such superficial compliance could be harder when the objective is defined on model internals rather than outputs." *Probe-guided fine-tuning* fits probes that detect an undesired property in the residual stream, then uses their scores as the only training signal: "At each fine-tuning step, we generate on-policy completions, read out their activations, and score them with the probes. These scores are the supervisory signal for the model update" (§3.1). The loss is a hinge penalty for activations outside the "benign" region, plus a KL anchor to the base model (§3.4). Probes are linear or non-linear, one per layer or a *polytope* of K = 16 (§3.2). The key design choice is the update regime: "In the frozen regime, we hold the initial probes fixed. In the continuously updated regime, they track the model, continuing from their current weights for N steps after each model update" (§3.3). A third, *retrained* regime fits fresh probes from scratch after each update (App. B.3).

The paper's own summary: "Training against probes thus offers a way to shape what models represent rather than only what they output, which may become increasingly important as models get better at making their outputs look aligned" (Abstract).

---

## C1. Alignment

### C1a. Outer alignment: **~**
*Load-bearing assumption:* a model that keeps its activations out of the "harmful" region will also give good responses, not just non-harmful ones.

**Steelman.** The objective is "don't represent the undesired property," regularized toward the original model. That is closer to the target than "a rater approved the output," which rewards what the overseer believes. For honesty it seems to work as hoped: "responses remain coherent and relevant to the prompt, with improvements taking the form of challenging false premises or acknowledging information that the base model conceals" (§4.3).

**What the objective leaves out.** The authors name it themselves (§5, Limitations): "A probe objective does not specify what a good response should look like. Improving the target metric can therefore lead to unintended responses. We see this in the harmfulness experiments: instead of explicitly refusing harmful requests, the model often reframes the task, produces placeholder content, or gives empty or disrupted answers." Explicit refusals "account for at most 1% of responses at our selected continuously updated checkpoints" (§4.3). The rest are soft refusals, disrupted text, and *pseudo-compliance*, which "appears to fulfill the request but provides no usable instructions for carrying it out" (§4.3). In Figure 2 a bomb-making request is answered with a fajita recipe.

*Our read:* this is the outer-alignment gap in a clean form. The objective rewards an absence (no harmful representation) and is silent on what should be present. Pseudo-compliance is a model that *looks* like it is complying while not complying, which is the mirror image of the deception the method was meant to prevent. It is harmless here, but it is not what a user or overseer would ask for. The frozen regime shows the gap more starkly: "the probe loss L_probe goes to zero while StrongREJECT stays above 0.40 for all models and probe variants" (§4.1).

### C1b. Inner alignment: **~**
*Load-bearing assumption:* a model that hides a property from a continuously refit probe has to remove the property, rather than re-encode it somewhere the probe family can't see.

**Steelman.** This is the one method in our set aimed squarely at deceptive alignment. The introduction cites alignment faking and models that "may manipulate their own training through gradient or exploration hacking" (§1). Output-based training can't tell an aligned model from one acting aligned; a probe can, in principle. Three results suggest the change goes deeper than the output:
- **Updated probes change behavior; frozen ones don't.** "Frozen probes are instead evaded as activations move across their fixed decision boundaries, allowing the probe loss to fall without reducing harmful compliance" (§4.1). Appendix B.5 shows how: "The frozen probe is thus evaded by a translation of the representation across its fixed boundary rather than by the concept being restructured. A fresh probe would re-separate the classes." Updated probes rotate "up to ∼90°" to follow the concept (App. B.5).
- **Robustness to removing refusal.** After abliteration, StrongREJECT on the continuously updated checkpoints changes "by only −0.03 to +0.07 ... compared with an increase from 0.09 to 0.76 for standard Llama 3 8B Instruct" (§4.2).
- **A partly independent check.** Models trained against the non-linear polytope are audited with a fresh *linear* probe, and the property is still found: "training against a non-linear polytope does not make the target property accessible only to a non-linear detector" (§4.4).

**The Goodhart question.** The paper is candid that its robustness tests are not adversarial to the method: "these attacks cannot establish model security as they are not adaptive to the probe intervention: prior work shows that attacks optimized against latent-space defenses can circumvent them (Bailey et al., 2026). We therefore interpret these results as a promising proof-of-concept" (§4.2). The auditing probes are also fit on the same labeled data as the training probes (§3.3), and none of the models started out with a reason to hide anything.

**The change does not survive further training.** Appendix E.3 attacks the Llama 3 8B checkpoints with a LoRA fine-tune on 400 conversations. Even with no harmful data the safety disappears: "Fine-tuning on benign data only (p = 0%) removes the safety of all four probe-trained checkpoints (e.g., 0.09 → 0.83 for the continuously updated polytope trained model and 0.08 → 0.91 for the retrained probe trained model), while Llama 3 8B Instruct stays at 0.12" (App. E.3). The authors offer "a possible but untested explanation": the checkpoints are "the abliterated base with a merged LoRA adapter, and further fine-tuning overrides this small update" (App. E.3). With harmful data (p ≥ 20%) every model, including the conventionally trained one, reaches 0.66 to 0.87. This sits next to the abliteration result above: the trained models resist one specific attack (removing the refusal direction) and lose everything to ordinary fine-tuning.

*Our read:* the evidence shows the method can reshape a *cooperative* model's representations without destroying their readability. It doesn't test a model that has an incentive to keep a goal hidden, which is the case C1b is about. E.3 adds a second caution: on the abliterated base, the trained behavior is a small update that benign fine-tuning erases, so "the property was removed" is not yet shown even for a cooperative model. So **~**, held up only by the abliteration and monitorability results.

### C1c. Where the human sits
**Label author.** Humans (or existing datasets: BeaverTails, Liars' Bench) define what counts as harmful or dishonest by labeling completions; the probes then act as the evaluator. No human is in the training loop. This is closer to Constitutional AI than to debate: the definition is written once and an automated judge applies it.

---

## C2. Scale invariance

### C2a. The repeated step
**Refit the probe on the current model's activations, then train the model against the refit probe** (§3.3). The step repeats every training update, and it is adversarial, like debate, except that the two players are a probe and the model. The paper's main finding is about this step: "What matters most is therefore that the probe is refit to the drifted activations at all" (App. B.3).

### C2b. What must stay invariant: **~**
*Load-bearing assumption:* the property stays readable by a simple probe at every capability level, and the human labels defining it stay meaningful.

Over one training run, it does: fresh linear probes on the selected checkpoints "retain held-out AUROC close to the base models: 0.85–0.90 for harmfulness and 0.98–0.99 for dishonesty" (§4.4, Table 2), and "Recoverability stays high almost everywhere" across layers and steps (App. B.4). *Our read:* this is invariance across *training steps of one model*, not across *capability levels*. Whether a much stronger model under pressure keeps harmful intent in a linearly readable subspace is the linear representation hypothesis (§2) applied at a scale nobody has tested.

*Our read, through the library:* C2b's "stable basin" option asks whether the trained state is an attractor. In [Wilson's](../scale%20invariance%20library/1-self-similarity-and-fixed-points.md#wilson-1982-the-renormalization-group-and-critical-phenomena-nobel-lecture) terms it is not, at least for the abliterated Llama runs: a perturbation as mild as benign fine-tuning moves the model all the way back (App. E.3). What stays invariant is the *readability* of the property (B.4); the *safe behavior* does not. The update regime also matters at the margin: "Only in the retrained regime do some later layers become harder to monitor" (App. B.4), and the retrained checkpoints "are less consistently resistant" to abliteration, rising by 0.18 to 0.22 (App. E.2).

### C2c. Error behavior: **~**
*Load-bearing assumption:* the refit keeps pace with the model's drift at every step.

**Correcting step:** the probe refit after each model update (§3.3). **Levels:** training steps; the paper reports probe angle, probe scores and a fresh probe's AUROC at every step (Figures 10 and 11), so this is a per-level measurement, not a single point.

**The map, with and without the correcting step.**
- *Without it (frozen):* the error grows step by step until the detector is blind. "The frozen probe never rotates (by construction) ... its mean score on both harmful and benign completions decreases over fine-tuning, so the harmful mean crosses the fixed boundary into the benign region even though the two classes stay separable" (App. B.5), ending where "the probe loss L_probe goes to zero while StrongREJECT stays above 0.40" (§4.1). **Compounding.**
- *With it (continuously updated or retrained):* the probe follows the concept, rotating "up to ∼90°" (App. B.5), and a fresh probe's held-out AUROC "stays high almost everywhere" across layers and steps (App. B.4). The gap stays bounded over every step measured. "What matters most is therefore that the probe is refit to the drifted activations at all" (App. B.3).

*Regime (criterion 1.1):* **thresholded**, with an unknown threshold. The gap stays bounded while the refit keeps up, and the frozen runs show what happens when it doesn't. The paper doesn't vary how much drift a refit can absorb (the refit budget N, the model size, or a model optimizing against the refit), so where the threshold sits is not measured. **~** stands.

*Our read, through the library:* the frozen regime is the [von Neumann](../scale%20invariance%20library/2-errors-across-levels.md#von-neumann-1956-probabilistic-logics-and-the-synthesis-of-reliable-organisms-from-unreliable-components) case without a restoring organ, and its failure has the shape [Bak, Tang and Wiesenfeld](../scale%20invariance%20library/1-self-similarity-and-fixed-points.md#bak-tang-and-wiesenfeld-1987-self-organized-criticality) predict for optimization against a fixed evaluator: the system settles where the evaluator reports zero while the behavior persists. This is the paper's cleanest scale-invariance evidence, and it is evidence across training steps of one model, not across capability levels.

### C2d. Phase transitions: **✗**
*Load-bearing assumption:* the model can't model, or optimize against, its own probe.

The authors point at the threshold but don't test it. Adaptive attacks against latent-space defenses are known to work (§4.2, citing *Obfuscated activations bypass LLM latent-space defenses*), and the conclusion leaves "its robustness under stronger optimization and in other settings an open question" (§5). The threshold is a model that can represent the refit process and keep the property in a form the next refit misses, a version of the gradient hacking the introduction names as motivation. A second threshold is a model whose harmful concept is no longer separable by a simple probe at all. Echoing the club's [recursive summary](../../discussions/recursive-summary.md), there is also a self-certification worry: the same kind of instrument trains the model and certifies that training didn't break the instrument. Under the criterion, a named failure mode that the method doesn't address is **✗**.

*Our read, through the library:* [Knill, Laflamme and Zurek's](../scale%20invariance%20library/2-errors-across-levels.md#knill-laflamme-and-zurek-1998-resilient-quantum-computation) threshold needs errors at different levels to be independent. Here the training probe, its refits and the audit probe are all fit on the same labeled pairs: "The refit uses the same labeled (prompt, completion) pairs as those used in the initial fit" (§3.3). A blind spot in those labels is shared by every level, which is the correlated-error case that defeats a threshold argument.

### C2e. Exact or statistical
**Statistical.** The result is empirical, over three models from 7B to 14B parameters.

### C2f. Measurability
Good in principle. The setup is cheap enough to run on a ladder of model sizes (and the code is public), but the paper gives three nearby points, not a slope: two models for harmfulness (7B and 8B) and one for honesty (14B), so no property has more than two sizes. In [Mandelbrot's](../scale%20invariance%20library/1-self-similarity-and-fixed-points.md#mandelbrot-1967-how-long-is-the-coast-of-britain) terms that is one or two rulers. The per-step measurements (C2c) are the richest axis the paper has. The test that would move this verdict: the same experiment across several sizes, with an adaptive attack or a deceptive model organism, audited by a detector from a different family (sparse autoencoder features, attribution graphs).

**C2 verdict: ✗** (the weakest of C2b to C2d). *Our read:* the method has one of the cleanest repeated steps in the folder and measures drift within a run, so this is a statement about what hasn't been tested, not a flaw in the experiments.

---

## C3. Competitiveness

### C3a. Training competitiveness: **✓**
*Load-bearing assumption:* labeled data for the probe is cheap to collect for the properties we care about.

Probes are small, the model is trained with LoRA (§3.4), and the probes are fit on 750 harmful and 750 benign completions (§3.5). The method is data-efficient compared with DPO: "Probe- and polytope-guided fine-tuning outperform DPO at smaller training data budgets, with comparable Pareto fronts only at 5,000 pairs" (§4.1).

### C3b. Performance competitiveness: **✓**
*Load-bearing assumption:* MMLU, GSM8K, IFEval and XSTest represent the uses we care about.

Utility stays near the base model throughout (Figure 3; checkpoints were selected to keep at least 95% of base utility), the method beats "inference-time steering with contrastive activation addition (CAA) ... or SafeFlow projection" across data budgets (§4.1), and over-refusal on benign prompts "stays at or below 6%, which is close to the base models" (App. E.1). *Our read:* the odd responses to harmful prompts (above) are an outer-alignment cost, not a usefulness cost, since they only appear on requests that should be declined anyway.

### C3c. Alignment tax trend
Not measured. *Our read:* probe fitting is cheap relative to fine-tuning, so the tax probably stays flat, unless stronger models need more expressive probes to keep up, which is the C2d question again.

---

## On Proposed amendment #1 (C1d, Monitorability)

The paper supports the amendment and partly answers it.
- **Support.** Its own frozen-probe runs show that training against a detector can break it, and the Discussion frames the debate: "Training against interpretability tools is debated because models may learn to evade the tools used to audit them without improving their behavior ... Our frozen-probe results illustrate this concern: the training loss can go to zero while harmful compliance persists" (§5).
- **Partial answer.** It checks monitorability with a fresh probe, and for the polytope runs that probe is from a different family (linear) than the one trained against (non-linear) (§4.4). The authors conclude that "training against an internal detector can improve alignment while preserving the ability to independently audit the target property with a simple linear probe" (§4.4).
- **What remains.** Both detectors are probes fit on the same labels, the models aren't trying to hide, and the attacks aren't adaptive (§4.2). *Our read:* if adopted, C1d should ask for a detector from a different method (not just a different probe family) and a model with a reason to evade.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| [Training against probes](libon-2026-training-against-probes.md) | ~ objective says what to avoid, not what to do | ~ hiding from a refit probe requires removing the property (benign fine-tuning undoes it, App. E.3) | ✗ model can't optimize against its own probe (untested) | ✓ cheap probe labels, data-efficient | ✓ utility and over-refusal near base |

---

## Open questions for the comparison

1. Does a detector from a different method (SAE features, attribution graphs as in [Lindsey et al.](lindsey-2025-biology-of-llm.md)) still find the property after training? That is the test that separates "removed" from "re-encoded."
2. What happens with a model that starts out deceptive (a model organism with a hidden goal), or under an attack optimized against the probe (§4.2)?
3. Does the probe-model arms race stay balanced across a sweep of model sizes and many more refit rounds?
4. Is pseudo-compliance a warning sign? A model that appears to fulfill a request while not fulfilling it is harmless here, but it is the output pattern the method was designed to make harder.
5. Is the safety a small update on top of the abliterated base (the authors' untested explanation for E.3), or would training a model that never lost its refusal give a change that survives fine-tuning? This decides whether the trained state is a stable basin (C2b).
6. Hubinger's transparency proposals use transparency to *check* training; this paper uses it to *drive* training. Does driving spend the oversight budget that checking relies on, and does the refit loop buy that budget back?

---

## What the rerun changed

The rerun (criterion 1.1, 2026-10-05) changed no mark. It changed what holds three of them up.
- **C1b ~, weaker.** Appendix E.3, missed in the first pass, shows benign fine-tuning erases the safety on all four Llama 3 8B checkpoints tested (for example 0.09 → 0.83). The mark now rests on the abliteration and monitorability results alone.
- **C2b ~, narrowed.** The property stays *readable* across training, but the *safe behavior* is not a stable basin (E.3), and the retrained regime loses some monitorability in later layers (B.4).
- **C2c ~, now with a map.** Under 1.1 the paper gives a per-step error map in both directions: compounding to total evasion without the refit, bounded with it. Regime: thresholded, threshold unmeasured.
- **C2d ✗, one more reason.** All probes share one labeled set, so their errors are correlated across levels.
- **C2f, unchanged mark, sharper count.** No property is measured at more than two model sizes.
