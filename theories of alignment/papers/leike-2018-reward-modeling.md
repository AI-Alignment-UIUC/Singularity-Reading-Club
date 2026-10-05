# Reward Modeling and RRM (Leike et al., 2018)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Jan Leike, David Krueger, Tom Everitt, Miljan Martic, Vishal Maini and Shane Legg, *Scalable agent alignment via reward modeling: a research direction* (2018). [arXiv:1811.07871](https://arxiv.org/abs/1811.07871).
**Role:** method (reward modeling, and its recursive extension, recursive reward modeling or RRM). The paper is a research agenda: it has no experiments of its own.
**Criterion version:** 1.0.
**Sources read:** the full paper via the arXiv PDF, with targeted readings of §1, §2.2, §3.1–3.2, §4.1, §4.3, §4.5, §5.8–5.9, §6, §7.5–7.6 and §8. Quotes were checked against that text.
**Not read:** nothing cited here was taken from a secondary source except Hubinger's numbering.
**Hubinger mapping:** RRM is proposal 8 (RRM + relaxed adversarial training); plain reward modeling is proposal 7 (narrow reward modeling + transparency tools). Leike et al. call both RRM and amplification instances of "the more general framework of iterated amplification" (§7.5).

---

## The method in one paragraph

The paper's own statement (abstract): "We outline a high-level research direction to solve the agent alignment problem centered around *reward modeling*: learning a reward function from interaction with the user and optimizing the learned reward function with reinforcement learning." The target is "How can we create agents that behave in accordance with the user's intentions?" (§2). In plain reward modeling, "The user trains a *reward model* to learn their intentions by providing feedback. This reward model provides rewards to a reinforcement learning agent that interacts with the environment" (§3.1). For tasks the user cannot evaluate alone, RRM recurses: "In step 1, we train agent A<sub>1</sub> with reward modeling from user feedback as described in the previous section. In step k we use the agent A<sub>k-1</sub> to assist the user in evaluating outcomes when training agent A<sub>k</sub>" (§3.2). The whole agenda rests on two assumptions stated in §1:

> **Assumption 1.** "We can learn user intentions to a sufficiently high accuracy."
>
> **Assumption 2.** "For many tasks we want to solve, evaluation of outcomes is easier than producing the correct behavior."

---

## C1. Alignment

### C1a. Outer alignment: **~**
*Load-bearing assumption:* the user's (assisted) evaluation of outcomes tracks what the user actually wants, and the evaluation decomposition misses no aspect of the outcome.

*Steelman.* The objective is the user's own judgment of outcomes, learned rather than hand-written, so it avoids the specification gaming of fixed reward functions. Separating "what" from "how" is meant to make the objective inspectable: "Separating learning the objective from learning the behavior allows us to achieve higher confidence in the resulting behavior" and "If we understand and trust the reward model, we know what the agent is 'trying' to accomplish" (§6).

*Critique.* The agent maximizes a model of what the user *judges* to be good outcomes, not the outcomes themselves. The paper names two ways this goes wrong (§4.3): *reward gaming*, where "the agent might figure out how to specifically craft these adversarially perturbed inputs in order to trick the reward model," and *reward tampering*, where it interferes with "the feedback for the reward model, the observation the reward model uses to determine the current reward, the code that implements the reward model, and the machine register holding the reward signal." For RRM there is a further requirement the authors flag themselves:

> "This decomposition needs to be exhaustive: if we neglect to assess one aspect of the task outcome, then the new agent A<sub>k</sub> might optimize it in an arbitrary (i.e. undesirable) direction." (§3.2)

*Our read:* outer alignment holds only as far as Assumption 1 and exhaustive evaluation hold. Unlike imitative amplification, this objective is optimized with RL, so every gap is under optimization pressure.

### C1b. Inner alignment: **~**
*Load-bearing assumption:* the proposed trust techniques (interpretability, adversarial training, plan generator / executor split, verification) mature enough to tell whether the policy is optimizing the reward model.

*Steelman.* The paper has a named slot for this, the *reward-result gap*: "Even if we supply the agent with a correctly aligned reward function, the resulting behavior might still be unaligned" (§4.5). It defines the gap through "the reward function the agent seems to be optimizing" (§4.5), which is close to the mesa-objective idea. §6 asks the right question, "How can we be confident that the agent we are training is indeed sufficiently aligned?", and proposes safety certificates, interpretability and formal verification. §5.8 proposes training agents to "explicitly discover weaknesses in the reward model and opportunities for reward hacking."

*Critique.* The reward-result gap is framed as a capability failure ("the agent may fail to converge to an optimal policy", §4.5), not as a learned optimizer with different goals. Deceptive alignment is not discussed; the only use of "deception" is in the critique of debate (§7.6). The mitigations are listed as directions, and §5.8 warns that adversarial training "might push the data requirements beyond affordable limits."

*Our read:* the paper is aware of a gap between reward model and policy and proposes tools, but nothing is tested and the deceptive case is absent. That is **~** (strong, untested assumption), not **✓**.

### C1c. Where the human sits
Evaluator of outcomes, assisted by earlier agents. In plain reward modeling the user gives feedback that trains the reward model (§3.1). In RRM the user decomposes the evaluation, hands parts to A<sub>k-1</sub>, and aggregates; "As k increases, the user plays a smaller and smaller part of the overall workload of this evaluation process and relies more and more on the assistance of other agents" (§3.2). Feedback on the agent's reasoning is optional: "users have the option to provide feedback on the cognitive process that produced outcomes, but they are not required to do so" (§7.5).

---

## C2. Scale invariance

### C2a. The repeated step
**Assist and evaluate.** At every level the previous agent helps the user evaluate the next agent's outcomes, the user aggregates, and the aggregated evaluation trains a reward model for the next agent (§3.2). Assistance "can take various forms: providing relevant auxiliary information, summarizing large quantities of data, interpreting agent A<sub>k</sub>'s internals, solving sub-problems that the user has carved off, and so on" (§3.2). The fantasy-novel example: the user evaluating a novel is helped by agents that summarize the plot, check grammar and assess character development (§3.2).

### C2b. What must stay invariant: **~**
*Load-bearing assumption:* Assumption 2 holds at every level, and the user's way of decomposing and aggregating an evaluation stays reliable as the outcomes become unfamiliar.

*Steelman (Evan's reading).* RRM asks the human for a smaller invariant than amplification does. The human does not need to know how to produce the behavior or how to reason about it, only how to *judge outcomes* with help. The paper says plainly that this is a different thing from modeling the expert's reasoning: "recursive reward modeling as described in Section 3.2 does not try to model expert reasoning explicitly. Instead, recursive reward modeling only requires users to evaluate outcomes. Nevertheless, it relies on decomposition of the evaluation task which has similarities to the decompositional reasoning described here" (§7.5). The invariant is then "the user's way of carving an evaluation into simpler evaluations and recombining them", which is the evaluation-side analogue of the factored cognition hypothesis. The authors argue this is very general, by analogy with alternating quantifiers: "Basically every formal statement that mathematicians care about can be written as a first-order logic statement with a finite number of alternating quantifiers. This suggests that recursive reward modeling can cover a very general space of tasks" (§3.2). The ladder only needs each rung to be narrower than the next: "the task of agent A<sub>k-1</sub> needs to be a simpler task in a more narrow domain compared to the task of agent A<sub>k</sub>" (§3.2).

*Critique (the club's counterpoint).* The paper's own list of hard-to-evaluate domains is the list of places where human judgment is least stable: tasks that are "extremely technical (e.g. x86 machine code), highly complex (e.g. a corporate network or a folded protein), very high-dimensional ... have delayed effects ... or be otherwise unfamiliar to humans" (§3.2). The evaluation function the user applies at level 1 (is this paragraph well written?) is not obviously the function needed at level k (is this protein safe?). Two further points from the text sharpen this:
- The invariant has to be *exhaustive* at every level (§3.2, quoted under C1a). A judgment function that is stable but silently skips one aspect fails, because RL will push on the skipped aspect.
- The human's share shrinks with k (§3.2, quoted under C1c). So the invariant judgment acts through a thinner and thinner channel, while more of the evaluation is done by agents trained on earlier, possibly drifted, judgments.

*The weaker bet.* If the real requirement is only that the user can recognize "this step is acceptable and not deceptive" (the corrigibility-basin reading usually attributed to Christiano; secondary, not in this paper), RRM is in a worse position than amplification to use it, because the user judges *outcomes* and process feedback is optional and "might be difficult to provide in practice if the policy model is not very interpretable" (§7.5). *Our read:* RRM commits to an outcome-evaluation invariant, not an acceptability invariant.

### C2c. Error behavior across levels: **?**
*Load-bearing assumption:* errors made by A<sub>k-1</sub> as an assistant are dampened rather than amplified when they shape A<sub>k</sub>'s reward.

The authors raise this as the central open question and do not answer it:

> "An important open question is whether errors accumulate: do the mistakes of the more narrow agent A<sub>k-1</sub> lead to larger mistakes in the training of agent A<sub>k</sub>? Or can we set up the training process to be self-correcting such that smaller mistakes get dampened (e.g. using ensembles of agents, training agents to actively look for and counteract these mistakes, etc.)? If error accumulation can be bounded and reward modeling yields aligned agents, then the hierarchy of agents trained with recursive reward modeling can be argued to be aligned analogously to proving a statement about natural numbers by induction." (§3.2)

This is the clearest statement in either paper of what C2 asks: the safety argument is an induction, and it holds only if the step preserves alignment *with bounded error*. *Our read:* the paper names the condition precisely and leaves it open, so **?**.

### C2d. Phase transitions: **~**
*Load-bearing assumption:* that tampering with the feedback process stays "somewhat contrived" at higher capability.

- **Tampering with the evaluator.** §4.3 lists the user's feedback itself among the things the agent might interfere with, and treats current examples as "somewhat contrived and this may or may not be a problem in practice depending how carefully we follow good software design principles." At the capability where an agent can influence the user's judgments, the problem changes kind, and §4.3 says it "might require a different set of solutions."
- **Assistant and assessed agent.** *Our read:* in RRM, A<sub>k-1</sub> shapes what the user sees about A<sub>k</sub>. Nothing in the paper rules out the assistant's errors or incentives correlating with the assessed agent's. This is an RRM-specific transition with no analogue in plain reward modeling.
- **Performance cuts both ways.** "The higher the agent's performance, the more likely it could be to produce surprising unintended behavior. On the other hand, higher levels of performance could also lead to more aligned behavior because the agent is more competent at avoiding unsafe states" (§2.2). The authors treat capability as a matter of degree here, which the criterion asks us to question.

### C2e. Exact or statistical
**Neither yet: an argument schema.** The induction analogy (§3.2) has the shape of an exact argument, but each premise (Assumption 2 at every level, bounded error, aligned base case) is unproven, and there is no data. The quantifier analogy is about the *coverage* of tasks, not about whether alignment survives the recursion.

### C2f. Measurability
The paper calls the agenda "'shovel-ready' for empirical research today" and proposes "getting empirical data on the severity of the challenges; prototyping solution ideas; scaling reward modeling to more difficult tasks" (§8). It does not propose a multi-scale test of error accumulation. *Our read:* the natural test is to run RRM for several k on tasks with hidden ground truth and plot evaluation error against k, which is sandwiching applied to the recursion rather than to one gap.

### C2 verdict: **~**
C2c is **?** and C2b, C2d are **~**. The induction framing earns credit for stating the scale-invariance condition exactly, but every premise is open. *Our read:* **~**, with the same caveat as for [amplification](christiano-2018-amplification.md) on how **?** combines.

---

## C3. Competitiveness

### C3a. Training competitiveness: **~**
*Load-bearing assumption:* reward-model accuracy is reachable "with an amount of data that we can produce or label within a realistic budget" (§4.1).

*Steelman.* Competitiveness is a stated design goal: "**Economical.** To defuse incentives for the creation of unaligned agents, training aligned agents should not face drawbacks in cost and performance compared to other approaches" (§1). The method fits standard pipelines: "This direction fits well into existing efforts in machine learning because it can benefit from advances in the state of the art in supervised learning and reinforcement learning" (§8). The reward model saves labor: "The user does not have to provide feedback on every interaction between agent and environment" (§3.1).

*Critique.* §4.1 calls the feedback budget "a crucial question", and §5.8 warns adversarial training "might push the data requirements beyond affordable limits". RRM also trains a whole hierarchy of agents.

### C3b. Performance competitiveness: **✓**
*Load-bearing assumption:* Assumption 2 holds for the tasks that matter.

The output is an RL agent, not a question-answerer, so there is no restriction on agency. "Since most real-world problems can be cast in the RL framework, deep RL is a particularly promising technique" (§8). The authors contrast this with imitation, which is "unlikely to be competitive with other strategies to train agents in the longer term" (§7.1; wording checked, section *(unverified)*), and with modeling expert reasoning, which "might not be economically competitive with recursive reward modeling" (§7.5). *Our read:* ✓ on performance; the cost of that performance is paid in C1a (optimization pressure on the reward model).

### C3c. Alignment tax trend
*Our read:* the human share of evaluation shrinks with k (§3.2), which suggests a flat or falling human cost. But the number of agents to train, and the cost of adversarial training and uncertainty estimation (§5.8, §5.9), grow with the task. The paper does not estimate the trend.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| RRM (Leike 2018) | ~ | ~ | ~ | ~ | ✓ |

---

## Open questions for the comparison

1. **Outcome invariant vs reasoning invariant.** RRM needs a stable way to evaluate outcomes; amplification needs a stable way to decompose reasoning (§7.5 draws the contrast). Which is more plausible as a fixed function across levels? Keep this disagreement visible in the grid (Rule 2).
2. **Error accumulation.** §3.2 states the induction condition precisely. Has any later work (for example Wu et al. 2021 on book summarization) measured error against recursion depth?
3. **Exhaustiveness.** Is there any way to check that an evaluation decomposition missed nothing, or is this a permanent outer-alignment gap under RL?
4. **Assistant–agent correlation.** What stops A<sub>k-1</sub>'s blind spots from being exactly the ones A<sub>k</sub> exploits? Hubinger's proposal 8 adds relaxed adversarial training; does that address this, or only the agent's own inner alignment?
5. **Reward-result gap vs mesa-optimization.** §4.5 defines the gap behaviorally (the reward function the policy *seems* to optimize). Does that framing fit the deceptive-alignment case, where the gap is invisible on-distribution?
