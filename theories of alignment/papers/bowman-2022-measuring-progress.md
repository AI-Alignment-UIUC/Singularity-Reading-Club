# Measuring Progress on Scalable Oversight (Bowman et al., 2022)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Samuel R. Bowman, Jeeyoon Hyun, Ethan Perez, Edwin Chen, Craig Pettit, Scott Heiner, Kamilė Lukošiūtė, Amanda Askell, Andy Jones, Anna Chen, et al. *Measuring Progress on Scalable Oversight for Large Language Models.* 2022. [arXiv:2211.03540](https://arxiv.org/abs/2211.03540).

**Role:** *evaluation*. It proposes an experimental paradigm (sandwiching) for testing oversight methods, and runs it once on a simple baseline method. We analyze it in two parts: (a) sandwiching as a way of measuring oversight methods, which feeds C2f for every other method, and (b) the baseline it tests, humans chatting with an unreliable model assistant, which gets the verdict row.

**Sources read.** The arXiv PDF, read through a text extraction tool (WebFetch) with targeted prompts. Direct download was blocked, so we could not read the raw text end to end. Quotes below come from that extraction with the section it reported. Quotes containing "..." were shortened by the extraction. One extraction gave a different model-alone MMLU number from the other, so we report only Table 1 rows that were consistent across extractions; check Table 1 in the PDF before citing numbers. Nothing here is from secondary sources.

**Criterion version.** 1.0.

---

## The method in one paragraph

The paper's aim (Abstract): "Developing safe and useful general-purpose AI systems will require us to make progress on scalable oversight: the problem of supervising systems that potentially outperform us on most skills relevant to the task at hand." Sandwiching (§2): "researchers choose problem settings where a model is already more capable than a typical human, but less capable than an expert ('sandwiching' the model's capabilities between that of the typical humans and the experts)." Non-experts try to get correct answers out of the model using some oversight technique; experts (or, in practice, dataset labels) score the result afterwards. The baseline technique tested is plain dialog: participants answer hard multiple-choice questions from MMLU and from QuALITY under a 5-minute time limit, while chatting with "our existing natural language assistant (Bai et al., 2022)" (§1; section not confirmed). The headline (Abstract, §4): model-assisted humans beat both the model alone and their own unaided performance, but "This simple paradigm does not succeed fully."

---

## Part (a): sandwiching as a measuring instrument

The three roles (§2):
- "The Expert Evaluators: These human participants have all of the skills or knowledge they need to oversee a system's performance on the task"
- "The Model: The machine-learning model is also expected to have most or all of the skills or knowledge needed to solve the task but is not expected to be aligned"
- "The Non-Expert Participants: These human participants understand the task and are well aligned, but are missing some crucial skills or knowledge"

Why it is meant to transfer (§2): "The situation of the non-expert participants is analogous to the situation we expect to find ourselves in with more capable future models: They have a wide range of tools and techniques at their disposal, including access to an untrustworthy but capable AI system, but they have no straightforward way to be certain that any of the decisions that they make are correct." And (§2): "If this succeeds, it suggests that—at least in some important ways—our techniques are likely to be up to the task of aligning potential future systems that show broadly superhuman performance."

What it can test (§2, "Potential Techniques"): plain model interaction, debate, market-making, and "Self-Critique, Amplification and Recursive Reward Modeling." So every method in this folder that is a protocol for a weaker overseer is, in principle, measurable this way.

Relaxations the authors adopt (§2): a "Static Model" ("participants can interact with the model only through dialog, without the ability to inspect it or further fine-tune it") and "Labels in Place of Experts" ("we can reliably evaluate model performance on a preexisting test dataset without any expert involvement").

**Feeds: C2f.** Sandwiching is the club's best candidate for a box-counting measurement, but as run here it is one point. The gap between non-expert and model is created by a single task choice (and, for QuALITY, a 5-minute time limit), not by a series of capability levels. We searched for any statement about repeating the experiment at several capability gaps or tracking progress across model generations and found none. *Our read:* to get a slope, one would vary the gap deliberately (time limits of 1, 5, 20 minutes; models of several sizes; non-experts of several skill levels) and plot "fraction of the expert-minus-non-expert gap closed" against gap size. A flat curve would be evidence for statistical self-similarity of oversight difficulty; a knee would be a candidate phase transition. Two built-in ceilings limit this: the method needs experts (or labels) above the model, so it can never measure the regime past the best humans, and the model under test is not trying to deceive anyone.

---

## Part (b): the baseline method (humans plus a chat assistant)

### C1a. Outer alignment: **~**
*Load-bearing assumption:* what a non-expert accepts after chatting with the model tracks what is true.

*Steelman.* The paper suggests the human–model team's output could become a training signal (§2): "a human–model team is capable of exploiting the model's knowledge and skills to achieve reliable aligned high performance...the outputs of such a pipeline can likely be used to update the model to demonstrate more aligned behavior." Accuracy gains are real (Table 1, §4): on MMLU, unassisted humans 57.2% vs. human + model 75.4%; on QuALITY, 48.6% vs. 76.8%.

*The gap.* The signal is the non-expert's belief, and the paper reports it can be wrong with high confidence (§4): "Human participants in this protocol sometimes give highly confident judgments that turn out to be wrong. We sometimes see participants accepting false claims from the model." Confidence rose much more than warranted: "giving annotators access to the model increases their confidence in answering (from 54% to 80% on MMLU; 38% to 75% on QuALITY)" (§4). *Our read:* optimizing a model against this signal rewards being convincing to a non-expert, which is the sycophancy failure (Theme C in the club summary).

### C1b. Inner alignment: **?**
*Load-bearing assumption:* the assistant is not optimizing to mislead its overseer.

The baseline does nothing about inner alignment, and the paper's experiment uses a static, non-adversarial model. It notes the opposite problem, deference (§4): the model "will reliably update its assumptions in response to corrections...but also causes it to be overly deferential at times, going along with participant misunderstandings." It also flags that other techniques raise the deception question directly (§2): "Debate and market-making notably incentivize the model under study to present misleading arguments or evidence in some instances." *Our read:* nothing in plain dialog would detect a model that chose to mislead. We mark **?** because the source does not address it.

### C1c. Where the human sits
Non-expert evaluator in dialog with the model, making the final call. Experts (here, dataset labels) sit outside the loop as the measuring stick only.

### C2a. The repeated step
None. Plain dialog is a single human–model interaction with no recursion. *Our read:* by our criterion it is not scale-invariant by construction; its safety must be re-argued at each capability level. Sandwiching itself has a repeated step in a weaker sense (one experiment per capability gap), which is what makes it useful for C2f.

### C2b. What must stay invariant: **✗**
*Load-bearing assumption:* a non-expert can detect the model's errors through conversation alone.

The paper already shows this failing at a modest gap (confident acceptance of false claims, §4). *Our read:* as the gap widens, the non-expert's ability to catch errors by questioning should shrink, not stay fixed.

### C2c. Error behavior across levels: **?**
*Regime (criterion 1.1):* One gap, no levels, so there is no map. **?** stands.
There are no levels to stack. The paper measures one gap.

### C2d. Phase transitions: **?**
Not discussed. *Our read:* the obvious threshold is a model that models the participant and tailors its claims; the deference finding shows a weak, unintentional version of this already.

### C2e. Exact or statistical
Statistical, and a single point: two tasks, one model, ten participants (§3.2: "two groups of five participants each for the two tasks (ten total)").

### C2f. Measurability
High in principle (the whole paper is a measurement protocol) and low in practice (one gap). See Part (a).

### C2 verdict: **✗**
No repeated step, and the invariant it needs (non-experts catch errors by talking) already fails at a small gap. The authors themselves (section not confirmed by our extraction): "we do not think that our results justify concluding that our simple dialog-based baseline technique will be satisfactory for the oversight of more capable future systems."

### C3a. Training competitiveness: **✓**
*Load-bearing assumption:* non-expert time is cheap and the model is used as is.

No new training is required; the cost is crowdworker time with an off-the-shelf assistant (§3.2: "Surge pays a minimum of $20/hr for active work").

### C3b. Performance competitiveness: **~**
*Load-bearing assumption:* tasks stay in the form of short questions with checkable answers.

Assisted humans beat the model alone, but remain well short of experts (published expert estimates 90.0% on MMLU and 93.5% on QuALITY, Table 1). The authors limit the scope (section not confirmed by our extraction; likely §4): "We are limiting ourselves to the controlled, relatively unrealistic setting of unambiguous multiple-choice questions" and "Our results are simply not strong enough to validate our simple human–model interaction protocol for use in high-stakes situations."

### C3c. Alignment tax trend
*Our read:* the dollar cost is flat, but the quality cost should grow with the gap, since the human is the bottleneck in every decision. That is the pattern C3c warns about: cheap now, useless when it matters.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| Plain dialog baseline (Bowman 2022) | ~ | ? | ✗ | ✓ | ~ |

**Feeds: C2f.** Sandwiching is the measurement every other row's C2f should point to. As published it is one capability gap (one point, no slope); it cannot reach past expert level; and it uses a non-adversarial model, so it tests the step but not the deceptive-overseer phase transition.

---

## Open questions for the comparison

1. **A sandwiching curve.** Has anyone run sandwiching at several gap sizes (time limits, model sizes, participant expertise) to get the slope our C2f asks for? Later debate experiments with time-limited QuALITY are candidates to check.
2. **Above the top slice.** Sandwiching needs someone above the model. What replaces the expert row once models exceed all humans, and is weak-to-strong generalization the right continuation of the same measurement?
3. **Adversarial sandwiching.** The model here is helpful, not hostile. Should a C2f "measurable" mark require testing with a model trained to mislead, since that is the phase transition most methods worry about?
4. **Overconfidence as a metric.** The jump from 54% to 80% confidence on MMLU suggests reporting calibration of the overseer, not only accuracy, as a standard sandwiching outcome.
5. **Evaluation role in the grid.** The criterion marks evaluation papers **n/a**. We gave the baseline a real row because the paper does test a method; the comparison should decide whether that is the right handling.
