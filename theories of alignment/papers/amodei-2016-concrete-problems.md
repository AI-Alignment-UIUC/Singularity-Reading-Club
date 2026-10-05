# Concrete Problems in AI Safety (Amodei et al., 2016)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation:** Dario Amodei, Chris Olah, Jacob Steinhardt, Paul Christiano, John Schulman, Dan Mané. *Concrete Problems in AI Safety.* arXiv:1606.06565, 2016. https://arxiv.org/abs/1606.06565

**Role:** motivation (a problem statement, not a method).

**Sources read:** the arXiv PDF (https://arxiv.org/pdf/1606.06565), through a text extraction, with targeted passes over the abstract, Section 2, Section 4 ("Avoiding Reward Hacking") and Section 5 ("Scalable Oversight"). The quotes below come from that extraction; check the PDF before putting one on a slide. Nothing was taken from secondary sources.

---

## What the paper contributes

The paper turns "AI accidents" into five research problems that can be worked on with today's machine learning, and sorts them by where they come from. Two problems come from a wrong objective (side effects, reward hacking), one from an objective that is too expensive to evaluate (scalable oversight), and two from the learning process itself (safe exploration, distributional shift). In the authors' words (Abstract):

> "We present a list of five practical research problems related to accident risk, categorized according to whether the problem originates from having the wrong objective function ("avoiding side effects" and "avoiding reward hacking"), an objective function that is too expensive to evaluate frequently ("scalable supervision"), or undesirable behavior during the learning process ("safe exploration" and "distributional shift")."

For this folder the paper matters because it is where *scalable oversight* gets its name and its first precise framing, and because its list of reward-hacking causes is a list of outer-alignment failure modes that every method in the grid has to face. It proposes research directions, not a solution, so it gets no marks.

---

## C1. Alignment: what the paper contributes

**C1a (outer alignment).** Section 4 gives the basic definition of an outer-alignment failure, written before the outer/inner vocabulary existed:

> "Formal rewards or objective functions are an attempt to capture the designer's informal intent, and sometimes these objective functions, or their implementation, can be 'gamed' by solutions that are valid in some literal sense but don't meet the designer's intent." (§4)

It then lists the sources of the gap. Three of them are directly useful as questions to ask every method under C1a:

- **Goodhart's law:** "a designer chooses an objective function that is seemingly highly correlated with accomplishing the task, but that correlation breaks down when the objective function is being strongly optimized." (§4) This is the reason C1a asks what the objective rewards *under strong optimization*, not on average.
- **Abstract rewards:** "Sophisticated reward functions will need to refer to abstract concepts ... These concepts concepts will possibly need to be learned by models like neural networks, which can be vulnerable to adversarial counterexamples." (§4; the doubled "concepts" is in the source extraction; we have shortened the quote at the ellipsis.) This is the reward-model weakness that RRM and RLHF inherit.
- **Environmental embedding:** "Sufficiently broadly acting agents could in principle tamper with their reward implementations, assigning themselves high reward 'by fiat.'" (§4)

**The scalable oversight gap is a C1a problem.** Section 5 frames the overseer's cheap signal as a proxy for an expensive true objective, and says the gap feeds reward hacking:

> "These cheaper signals can be efficiently evaluated during training, but they don't perfectly track what we care about. This divergence exacerbates problems like unintended side effects (which may be appropriately penalized by the complex objective but omitted from the cheap approximation) and reward hacking (which thorough oversight might recognize as undesirable)." (§5)

*Our read:* this sentence is the cleanest statement of why C1a must ask "truth, or what the overseer believes is true?" Every scalable-oversight method in the grid is an attempt to make the cheap signal track the expensive one more closely.

**C1b (inner alignment).** The paper does not discuss learned optimizers or deception. Its closest material is distributional shift (a learning-process failure), which we did not read in detail. *Our read:* for C1b this paper contributes nothing direct; Hubinger et al. (2019) fills that role.

**C1c (where the human sits).** The paper's formalization puts the human as an **expensive, sparse evaluator**. Section 5 recasts oversight as semi-supervised RL, where "the agent can only see its reward on a small fraction of the timesteps or episodes" (§5). This is the baseline role that later methods try to amplify.

---

## C2. Scale invariance: what the paper contributes

**The definition that C2 builds on.** The scalable-oversight problem as stated is about the cost of evaluation, not about a capability gap:

> "We may want the agent to maximize a complex objective like 'if the user spent a few hours looking at the result in detail, how happy would they be with the agent's performance?' But we don't have enough time to provide such oversight for every training example; in order to actually train the agent, we need to rely on cheaper approximations, like 'does the user seem happy when they see the office?' or 'is there any visible dirt on the floor?'" (§5)

*Our read:* in 2016 "scalable" meant scaling the *amount* of oversight (label efficiency), not scaling oversight to a system smarter than the overseer. The true objective here is still something a human *could* evaluate given a few hours. The later framing (Irving, Christiano, Leike) assumes the human could not evaluate it even with unlimited time. This distinction is worth carrying into C2: a method that solves the 2016 problem (sparse labels) has not thereby solved the capability-gap problem, which is the one where phase transitions live.

**C2c / C2d (errors and thresholds).** The paper says reward hacking gets worse with agent complexity, which is the earliest version of the C2 concern:

> "Just as the probability of bugs in computer code increases greatly with the complexity of the program, the probability that there is a viable hack affecting the reward function also increases greatly with the complexity of the agent and its available strategies." (§4, "Complicated Systems")

> "However, the problem may become more severe with more complicated reward functions and agents that act over longer timescales." (§4)

Section 2 adds autonomy as a qualitative step:

> "Systems that simply output a recommendation to human users, such as speech systems, typically have relatively limited potential to cause harm. By contrast, systems that exert direct control over the world, such as machines controlling industrial processes, can cause harms in a way that humans cannot necessarily correct or oversee." (§2)

*Our read:* the paper describes difficulty growing smoothly with complexity (a degree change) plus one change of kind: the move from recommending to acting, and the related ability to tamper with one's own reward ("environmental embedding"). Reward tampering is a candidate entry for the C2d list ("self-modify"), and the autonomy step is a candidate phase transition the criterion does not yet name explicitly.

**C2f (measurability).** The suggested experiments measure label efficiency at a single scale: "If the reward is provided only on a random 10% of episodes, can we still learn nearly as quickly as if it were provided every episode?" (§5, Potential Experiments). *Our read:* this varies the labelling budget, not the capability gap, so it is one point on the C2 plot, not a slope.

---

## C3. Competitiveness: what the paper contributes

The paper defines scalable oversight *as* a cost problem, so it frames C3 from the start: the true objective exists, but we "don't have enough time to provide such oversight for every training example" (§5). It also notes that safety problems carry engineering cost even now: "even for existing systems these problems can necessitate substantial additional engineering effort to achieve good performance, and can often go undetected when they occur in the context of a larger system" (§4).

*Our read:* this is the seed of C3a (human labor as the binding cost) and C3c (the alignment tax). It does not say how the cost changes with capability, so it gives no trend for C3c.

---

## Verdict line

| Paper | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| [Amodei 2016](amodei-2016-concrete-problems.md) | n/a | n/a | n/a | n/a | n/a |

**Feeds:** C1a (reward-hacking causes as the checklist for "what does the objective reward under strong optimization"), C1c (human as sparse, expensive evaluator), C2c (hack probability grows with agent complexity), C2d (reward tampering; recommend-to-act autonomy step), C2f (label-efficiency experiments are single-scale), C3a (human evaluation time as the cost driver).

---

## Open questions for the comparison

1. Which methods in the grid solve the 2016 problem (sparse but possible evaluation) and which address the harder one (evaluation impossible for the unaided human)? The grid should not credit the first as the second.
2. Goodhart's law says correlation "breaks down when the objective function is being strongly optimized." Does any method bound how strongly the policy optimizes against the overseer's signal, or does the bound shrink as capability grows?
3. Is "recommend vs. act" a phase transition worth adding to C2d explicitly, alongside "model the overseer" and "self-modify"?
4. Reward tampering ("assigning themselves high reward by fiat") is an outer-alignment failure in 2016 vocabulary, but in a recursive setting where the model trains its successor it becomes tampering with the next model's reward. How does that connect to the Anthropic (2026) case?
