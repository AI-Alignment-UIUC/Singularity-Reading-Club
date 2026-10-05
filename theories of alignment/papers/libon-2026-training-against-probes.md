# Training against probes (Libon et al., 2026)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Lena Libon, Alexander Panfilov, Ben Rank, Xin Chen, Jonas Geiping and Maksym Andriushchenko, "Alignment via Training Against Probes Without Losing Monitorability," arXiv [2609.38645](https://arxiv.org/abs/2609.38645) (v1 submitted 29 Sep 2026, v2 revised 1 Oct 2026).

**Role:** method.

**Sources read (criterion v1.0, Rule 0).**
- Read: the arXiv abstract page (abstract copied in full) and the arXiv HTML version (https://arxiv.org/html/2609.38645), queried twice for section structure, method, results, monitorability and discussion.
- Could not open: the arXiv PDF and the v1/v2 HTML pages (the fetch proxy returned HTTP 429, rate limited, on every further request), and the alphaxiv PDF mirror (blocked by robots.txt). So we did **not** read the Discussion section or Section 4.4 word for word. Quotes below came back from the HTML version through a summarizing fetch tool. Where the tool's section label disagreed with the text we could check (two sentences it attributed to the abstract are not in the abstract), the quote is marked *(section uncertain)*. Anything we could not see in quotation marks is marked *(unverified)* or labelled as ours.
- Models tested: Llama 3 8B Instruct (standard and abliterated), Mistral 7B Instruct v0.1, and Qwen3-14B (a reasoning model).

---

## The method in one paragraph

Most alignment training scores what a model *outputs*. The authors argue that this only checks whether a response "*looks* aligned, but never inspect[s] how the model *represents* the response it is producing" (§1), and that a capable model could pass such checks without changing underneath. The abstract states the bet in one line: "Such superficial compliance could be harder when the objective is defined on model internals rather than outputs." The method, *probe-guided fine-tuning*, trains probes (linear or non-linear, one or several per layer) to detect an undesired property in the model's activations, then uses the probe scores as the training signal: "At each fine-tuning step, we generate on-policy completions, read out their activations, and score them with the probes. These scores are the supervisory signal for the model update" (§3.1). The loss is "ℒ_FT(θ)=ℒ_probe+β ℒ_KL, where ℒ_probe penalizes violations of constraints z_k(h_t)≤-1" (§3.4), with a KL term keeping the model close to the original. The key design choice is the probe update regime: "In the *frozen* regime, we hold the initial probes fixed. In the *continuously updated* regime, they track the model, continuing from their current weights for N steps after each model update" (§3.3). Two objectives are tested, harmlessness and honesty, against DPO and inference-time steering.

The paper's own one-sentence summary: "Training against probes thus offers a way to shape what models represent rather than only what they output, which may become increasingly important as models get better at making their outputs look aligned" (Abstract).

---

## C1. Alignment

### C1a. Outer alignment: **~**
*Load-bearing assumption:* the probe direction is the concept we care about (harmfulness, dishonesty), not a correlate of it that a model can move away from while keeping the behavior.

**Steelman.** The objective is "no internal representation of the undesired property," regularized toward the original model. If the probe really tracks harmful intent, a model that perfectly optimized this would not just refuse in words; it would not be *computing* the harmful plan. That is a closer target than "the rater approved," which rewards what an overseer believes is aligned.

**Failure mode the paper itself demonstrates.** With frozen probes the objective is clearly not outer aligned: "the probe loss goes to zero while StrongREJECT stays above 0.40" (§4.1), and "Frozen probes are instead evaded as activations move across their fixed decision boundaries, allowing the probe loss to fall without reducing harmful compliance" (App. B.5). The authors call the frozen objective "easily exploitable" (Abstract). The continuously updated version is better because the target keeps moving with the model.

*Our read:* the objective is really "be undetectable by the best probe we can refit," which equals "lack the property" only if the property has to be linearly (or simply) represented to be used. That is an empirical bet, so the mark is **~**.

### C1b. Inner alignment: **~**
*Load-bearing assumption:* a model that hides a property from a continuously refit probe family has to remove the property, rather than re-encode it in a form outside that family (non-linear, distributed, or spread across tokens).

**Steelman.** This is the first method in our set aimed squarely at deceptive alignment. The introduction is motivated by it: models "can fake alignment" and "may manipulate their own training through gradient or exploration hacking" (§1). Output-based training cannot see the difference between a model that is aligned and one that is acting aligned; a probe can, at least in principle. The paper also reports robustness that output training lacks: probe-guided fine-tuning is "substantially more robust against jailbreak and abliteration attacks" (Abstract), which is some evidence the change goes deeper than surface refusals.

**The Goodhart question.** Does training against a detector remove the property or teach the model to hide it from the detector? The paper answers half of this. It admits the risk directly: "training against probe feedback can induce detector evasion rather than behavioral improvement," depending on "detector quality, the training algorithm, and regularization" (§2). Its evidence that continuously updated probes do *not* just get evaded is that harmfulness drops on behavioral benchmarks while the concept stays decodable: "as representations rotate and shift during fine-tuning, continuously updated probes track the changing harmfulness direction" (§4.1). The checker, though, is the same kind of tool as the trainer: a freshly fit linear probe. We did not see a test with a detector from a different family (sparse autoencoder features, attribution graphs, a non-linear probe held out from training). The fetch tool reported that the authors "don't explicitly state that non-linear probes could fail or that models might successfully hide properties" *(tool summary, not a quote)*.

*Our read:* the models tested (7B to 14B) were not deceptive to begin with, so the experiment shows the method can shape a cooperative model's representations without destroying linear readability. It does not show what happens when a model has an incentive to keep a goal hidden. That is the case C1b is about, so **~**, not ✓.

### C1c. Where the human sits
Human as **label author for the probe datasets** (what counts as harmful or dishonest) and as designer of the probe family. No human is in the training loop itself; the probes are the evaluator. This is closer to Constitutional AI than to debate: the human writes the definition once, and an automated judge applies it.

---

## C2. Scale invariance

### C2a. The repeated step
Alternate two updates: **refit the probe on the current model's activations, then train the model against the refit probe** (§3.3). This is a genuine repeated local step, and it is adversarial, like debate, except the two players are a probe and the model rather than two copies of the model.

### C2b. What must stay invariant: **~**
*Load-bearing assumption:* the property remains representable in the probe family (linear, or the chosen non-linear class) at every capability level, and the human labels defining it stay meaningful.

The paper's evidence is that it does over the training run: the concept stays "linearly encoded after fine-tuning, meaning oversight is not lost by our method" (Abstract). *Our read:* this is invariance across *training steps* of one model, not across *capability levels*. The linear representation hypothesis is a reasonable bet for 7B to 14B models; whether a much stronger model, under pressure, keeps its harmful-intent representation in a linear subspace is open.

### C2c. Error behavior: **~**
Within a run, the paper measures the key error, the gap between probe loss and actual behavior, and shows it is large for frozen probes and small for updated ones (§4.1). It does not measure how this gap changes as models get larger, or across many rounds of probe-model alternation. *Our read:* errors here behave like an arms race: bounded only while the probe refit keeps up with the model's drift.

### C2d. Phase transitions: **✗ (not addressed)**
*Load-bearing assumption:* the model cannot model its own probe.

The obvious threshold is a model that can represent the probe-refit process itself and learn to keep the property in a form the next refit will miss (a form of gradient hacking, which the introduction names as a motivation but which, as far as we could read, the method is not tested against). A second threshold: a model capable enough that the harmful concept is no longer linearly separable from benign neighbors. Echoing the club's recursive summary, this is also self-certification at one remove: the same kind of detector both trains the model and certifies that training did not break the detector. *Our read:* the paper doesn't discuss either threshold, so the mark is ✗ by the criterion's definition (known failure mode, not addressed).

### C2e. Exact or statistical
**Statistical.** The monitorability result is an empirical observation over three model families at 7B to 14B, not an argument that holds at every level.

### C2f. Measurability
Good, in principle. The setup is cheap enough to run on a ladder of model sizes, and the paper already has three. That is three nearby points on a log-log plot, not a slope. A scaling sweep (and a held-out detector at each scale) would be a direct box-counting test.

**C2 verdict: ✗** (the weakest of C2b to C2d, as the criterion requires). *Our read:* this is harsher than the paper deserves on C2a, since it has a clean repeated step and measures drift within a run. The mark would rise to ~ (the statistical cap) if the authors tested a model with an incentive to evade the refit probe, or a held-out detector, across several model sizes.

---

## C3. Competitiveness

### C3a. Training competitiveness: **✓**
*Load-bearing assumption:* labelled data for the probe can be collected cheaply for the properties we care about.

Probes are small, the training uses on-policy completions with a KL penalty (§3.4), and no extra human labeling is needed during training. *Our read:* the cost is in the same range as an online RL fine-tuning run plus small probe updates.

### C3b. Performance competitiveness: **✓**
*Load-bearing assumption:* the utility benchmarks used reflect real use.

Updated probes "substantially reduce harmfulness and improve honesty while preserving utility," and the method "achieves better safety-utility trade-offs than DPO and inference-time steering" (Abstract).

### C3c. Alignment tax trend
Not measured. *Our read:* probe training cost grows slowly with model size, so the tax probably stays flat, unless stronger models need more expressive (and more expensive) probes to keep up, which is the same question as C2d.

---

## On Proposed amendment #1 (C1d, Monitorability)

The paper supports the *need* for the amendment more strongly than its own title suggests. It shows, in its own experiments, that the naive version of training against a detector destroys the detector's value: "the probe loss goes to zero while StrongREJECT stays above 0.40" (§4.1), and the authors frame preserved monitorability as a result to be checked, not assumed (the title, §4.4, and "training against probe feedback can induce detector evasion rather than behavioral improvement," §2). It reports that a "freshly trained probe reaches AUROC comparable to one trained on the base model" *(section uncertain; tool attributed it to the abstract and to App. B.3/B.4)*, presented as evidence "against the categorical view that interpretability signals should never be used for training" *(section uncertain)*.

What it does not support is reading "monitorability preserved" as settled. The detector used to check is the same family as the detector trained against. *Our read:* if adopted, C1d should ask specifically whether monitorability is checked with a **detector held out from training** (a different probe family or interpretability method), since a same-family refit can only show that one door is still open.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| [Training against probes](libon-2026-training-against-probes.md) | ~ probe direction = the concept | ~ hiding from a refit probe requires removing the property | ✗ model can't model its own probe (unaddressed) | ✓ cheap probe labels | ✓ utility benchmarks representative |

---

## Open questions for the comparison

1. Does a detector from a different family (SAE features, attribution graphs as in [Lindsey et al.](lindsey-2025-biology-of-llm.md), a held-out non-linear probe) still find the property after training? This is the test that separates "removed" from "re-encoded."
2. What happens with a model that starts out deceptive (a model organism with a hidden goal), rather than a cooperative instruct model?
3. Does the probe-model arms race stay balanced across a model-size sweep, and across many more refit rounds?
4. How does this compare with Hubinger's transparency-based proposals, which use transparency to *check* training rather than to *drive* it? Using the tool as the gradient may spend the oversight budget those proposals rely on.
5. Is "train against interpretability" one more instance of the club's Gödel worry: a system certified by the same instrument that shaped it?
