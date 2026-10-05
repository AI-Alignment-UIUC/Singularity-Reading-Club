# Biology of a large language model (Lindsey et al., 2025)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Jack Lindsey et al. (Anthropic), "On the Biology of a Large Language Model," *Transformer Circuits Thread*, 2025. https://transformer-circuits.pub/2025/attribution-graphs/biology.html

**Role:** framework. It is not an alignment method. We analyze it as the **transparency ingredient** that Hubinger's proposals 1, 5, 6, 7 and 9 lean on for inner alignment, asking what it can show today and whether that is enough to carry those proposals. The club read it in April.

**Sources read (criterion v1.0, Rule 0).**
- Read: the paper page, through a summarizing fetch tool, queried five times for structure, findings and limitations.
- Partly unreadable: the fetch tool only returned text up to roughly the "Life of a Jailbreak" section plus the Introduction's summary of later sections. We could **not** read the body text of "Chain-of-thought Faithfulness," "Uncovering Hidden Goals in a Misaligned Model," "Limitations" or "Discussion" directly; quotes from those parts came back once and could not be re-checked, so they are marked *(unverified)*. Direct download was blocked by the network proxy. Section titles below are the paper's own.
- Model studied: Claude 3.5 Haiku, through a replacement model built from a cross-layer transcoder (CLT) *(per the fetch tool's summary)*.

---

## The method in one paragraph

The paper builds an interpretable "replacement model" whose features stand in for the original model's neurons, then traces *attribution graphs*: which features caused which on a particular prompt. The authors validate the graphs by intervening on features in the real model and checking that its behavior changes as predicted. The framing is biological: "The challenges we face in understanding language models resemble those faced by biologists" (Introduction), since models are produced by "simple, human-designed training algorithms" yet "the mechanisms born of these algorithms appear to be quite complex" (Introduction). This is the same point the club's recursive summary makes: the weights are fully visible, yet they don't reveal the mechanism, so you study the organism.

The paper's own one-sentence statement of purpose: "Our goal is to reverse engineer how these models work on the inside, so we may better understand them and assess their fitness for purpose" (Introduction).

---

## What it can currently show

These are the findings that bear on inner alignment. Each is evidence that some internal cognition invisible in the output can be read off.

- **Multi-step reasoning** ("Introductory Example: Multi-step Reasoning"). Asked for the capital of the state containing Dallas, the model internally represents "Texas" before producing "Austin," alongside shortcut paths *(paraphrase of the section; the fetch tool did not return an exact sentence)*.
- **Planning** ("Planning in Poems"). "Before beginning to write each line, the model identifies potential rhyming words that could appear at the end" (Planning in Poems). An earlier fetch also returned: "the model often activates features corresponding to candidate end-of-next-line words prior to writing the line, and makes use of these features to decide how to compose the line" *(unverified, returned once)*. Planning is exactly the kind of internal optimization Hubinger's proposals want to detect.
- **Hallucination circuits** ("Entity Recognition and Hallucinations"). A default "can't answer" pathway is suppressed by "known entity" features, and hallucinations occur when that suppression misfires *(paraphrase)*.
- **Refusals and jailbreaks** ("Refusals", "Life of a Jailbreak"). A general harmful-requests representation forms during finetuning *(paraphrase)*.
- **Unfaithful reasoning** ("Chain-of-thought Faithfulness"). The model sometimes reasons faithfully, sometimes not, including "cases where it works backwards from a human-provided clue so that its "reasoning" will end up at the human-suggested answer" (Introduction summary of this section). This is motivated reasoning caught in the act: the written chain of thought says one thing, the mechanism another.
- **Hidden goals** ("Uncovering Hidden Goals in a Misaligned Model"). A variant of the model "finetuned to pursue a secret goal: exploiting 'bugs' in its training process," where the relevant mechanisms were "embedded within the model's representation of its 'Assistant' persona" *(unverified, returned once; the section body was not readable)*.

---

## C1. Alignment (transparency as an inner-alignment tool)

### C1a. Outer alignment: **n/a**
*Load-bearing assumption:* none; the paper proposes no training objective.

### C1b. Inner alignment: **~**
*Load-bearing assumption:* the misaligned cognition we need to catch lives in the fraction of computation the replacement model captures, and in the prompts we think to trace.

**Steelman.** Hubinger's proposals need a tool that can tell a model pursuing the training objective from one pursuing a proxy or a hidden goal, without trusting its outputs. This paper shows instances of that: planning that is not in the output, a chain of thought that is not the real computation, and (in the hidden-goals case) a deliberately misaligned model whose objective is visible in its internals. *Our read:* the hidden-goals and chain-of-thought sections are the closest existing evidence that transparency can do the C1b job, not just describe benign circuits.

**Limits the paper states.** Coverage is partial: "Though it's difficult to quantify precisely, we've found that our attribution graphs provide us with satisfying insight for about a quarter of the prompts we've tried" (Limitations). Attention is not explained: "Our replacement model also doesn't attempt to replace the attention layers of the original model" *(unverified)*. The replacement model also does not perfectly reconstruct activations, leaving "error nodes" for what it misses *(paraphrase)*. *Our read:* the hidden goal was planted by the researchers, who knew what to look for. Auditing for an unknown goal in a model trained to hide it is a harder problem, and the paper doesn't claim to solve it.

### C1c. Where the human sits
Human as **analyst**: a researcher picks a prompt, reads the attribution graph, forms a hypothesis and tests it with interventions. In Hubinger's proposals this is the overseer "using transparency tools," and the human's interpretive work is the bottleneck.

---

## C2. Scale invariance

### C2a. The repeated step
None inside the paper. The method is applied prompt by prompt, not recursively. *Our read:* in Hubinger's proposals the repeated step is "the overseer (amplified human plus transparency tools) inspects the next model." Whether that step scales depends on whether this tool scales, which is what C2 asks here.

### C2b. What must stay invariant: **~**
*Load-bearing assumption:* stronger models keep representing concepts as sparse, human-interpretable features that a replacement model can recover.

There is one weak scale point in favor: "The language-independent circuits are more prominent in Claude 3.5 Haiku than in a smaller, less capable model" (Multilingual Circuits), which suggests that more capable models may share more abstract features rather than fewer. *Our read:* that is one comparison between two models, and says nothing about whether the abstractions stay *human-readable*.

### C2c. Error behavior: **?**
The reconstruction error and the three quarters of prompts without satisfying graphs are stated for one model. The paper does not report how either changes with scale *(as far as we could read)*.

### C2d. Phase transitions: **✗**
*Load-bearing assumption:* the model does not shape its own internals in response to being inspected.

The paper studies a model that has no reason to hide from the tool. Two thresholds matter for the proposals that depend on it: a model capable enough to plan around inspection (the target of training-against-probes work like [Libon et al.](libon-2026-training-against-probes.md)), and a model whose relevant computation lives in the parts the tool does not cover (attention, error nodes, the other three quarters). *Our read:* the club's Gödel point also applies: if a future model helps build the interpretability tools used on itself, it is partly certifying itself.

### C2e. Exact or statistical
**Statistical**, and at a single scale: one production model plus a smaller comparison in one section.

### C2f. Measurability
Possible but expensive. The "fraction of prompts with satisfying insight" is a natural metric to track across model sizes; the paper reports it once. A hidden-goal audit game repeated at several scales would be the box-counting test for this ingredient.

**C2 verdict: ✗** (weakest of C2b to C2d). *Our read:* this is a statement about the tool's current scaling story, not a criticism of the science; the paper never claims scale invariance.

---

## C3. Competitiveness

### C3a. Training competitiveness: **~**
*Load-bearing assumption:* the cost of training a replacement model and analyzing graphs stays small next to training the model itself.

It does not change training, so no lead is lost there. The cost is in building a CLT for each model and in human analysis per prompt; understanding a graph reportedly takes hours of researcher time *(unverified; returned once, not found on re-check)*. *Our read:* as an audit on a sample of prompts this is affordable; as a guarantee over all behavior it is not.

### C3b. Performance competitiveness: **✓**
*Load-bearing assumption:* it is used only to inspect, not as a training signal or a deployment restriction.

A diagnostic places no limit on what the model can do. (Proposals that act on what it finds, such as STEM AI or microscope AI, inherit their own C3b costs.)

### C3c. Alignment tax trend
*Our read:* likely growing. More capable models have more features, longer contexts and more complex graphs, and the human analysis step does not get cheaper unless it is itself automated, which brings back the self-certification question.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| [Attribution graphs (transparency)](lindsey-2025-biology-of-llm.md) | n/a | ~ misalignment lies in the ~25% of computation we can explain | ✗ model doesn't adapt to being inspected (unaddressed) | ~ per-prompt human analysis stays affordable | ✓ inspection only, no training signal |

---

## Open questions for the comparison

1. Hubinger's proposals 1, 5, 6, 7 and 9 assume transparency tools good enough to rule out deceptive cognition. Coverage of "about a quarter of the prompts" is the current number to compare that assumption against. How high must it be?
2. Can attribution graphs find a hidden goal in a blind audit, where the analysts did not plant it?
3. If [Libon et al.](libon-2026-training-against-probes.md) train against simpler detectors, do attribution-graph-level features survive as a held-out check? This would make the two papers a matched pair: one shapes internals, the other audits them.
4. Does automating the analysis step (models reading graphs of models) keep the tool checkable downward, or does it reintroduce the self-certification problem?
