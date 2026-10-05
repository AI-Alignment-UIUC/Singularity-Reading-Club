# An overview of 11 proposals (Hubinger, 2020)

[← criterion](../criterion.md) · [← comparison](../README.md)

## 1. Header

- **Citation.** Evan Hubinger, *An overview of 11 proposals for building safe advanced AI*, 2020. [arXiv:2012.07532](https://arxiv.org/abs/2012.07532); [Alignment Forum version](https://www.alignmentforum.org/posts/fRsjBseRuvRhMPPE5/an-overview-of-11-proposals-for-building-safe-advanced-ai).
- **Role.** *Framework* (it defines the four criteria our C1 and C3 are built on) and *method family* (it grades 11 proposals, each of which we grade too).
- **Sources read.** The Alignment Forum post and the arXiv PDF, both through a fetch tool that answers questions about the text. Quotes below were returned as verbatim by that tool, and the most important ones were cross-checked between the two versions. We did not read a clean full-text dump, so treat quotes as checked against an extraction, not the typeset PDF. Anything we could not confirm is marked *(unverified)*.
- **Section numbers.** In the arXiv version, proposal *n* is section *n*+1, and each has subsections .1 outer alignment, .2 inner alignment, .3 training competitiveness, .4 performance competitiveness (for example STEM AI is §7 and its "vulnerable world" sentence is in §7.4). We cite as "P*n*, §*n*+1.*k*".
- **Author's caveat.** Hubinger says the descriptions are his own versions: "you should treat all the approaches I describe here as *my version* of that approach." So every verdict below is about Hubinger's variant, not the original authors'.

## 2. What the paper does, and its four criteria

The paper takes 11 proposals for building safe advanced AI and, for each, asks the same four questions. It is not a ranking: each proposal gets a qualitative discussion, not a score. These four questions are where our criterion's C1 and C3 come from. In his words (introduction):

- **Outer alignment:** "Outer alignment is about asking why the objective we're training for is aligned—that is, if we actually got a model that was trying to optimize for the given loss/reward/etc., would we like that model?"
- **Inner alignment:** "Inner alignment is about asking the question of how our training procedure can actually guarantee that the model it produces will, in fact, be trying to accomplish the objective we trained it on." (The arXiv extraction omits "actually".)
- **Training competitiveness:** "Training competitiveness is the question of whether the given training procedure is one that a team or group of teams with a reasonable lead would be able to afford to implement without completely throwing away that lead."
- **Performance competitiveness:** "Performance competitiveness, on the other hand, is about whether the final product produced by the proposed process is competitive."

Most proposals pair a training objective (for outer alignment) with an inner-alignment safeguard: *transparency tools* (inspect the model), *intermittent oversight* (an overseer periodically inspects the model), or *relaxed adversarial training* (train the model against the overseer's estimate of catastrophe risk). The grid has no column for how a proposal behaves as capability grows. That is the gap our C2 fills.

*Our read:* as a framework, the paper itself gets **n/a** on every criterion. The marks below grade each proposal as Hubinger describes it.

---

## 3. The 11 proposals

### P1. Reinforcement learning + transparency tools (§2)

"Train a reinforcement learning (RL) agent in an environment where corrigibility, honesty, multi-agent cooperation, etc. are incentivized." The human designs the environment and inspects the agent with transparency tools.

- **Outer:** "If corrigibility, honesty, cooperation, etc. do in fact dominate in the limit, then such an approach would be outer aligned." (§2.1)
- **Inner:** "it seems quite plausible to me that this sort of approach could catch proxy pseudo-alignment, it seems unlikely that it would successfully catch deceptive pseudo-alignment, as it could be very difficult to make transparency tools that are robust to a deceptive model actively trying to trick them." (§2.2) He adds "there is a reasonable chance that it does just work."
- **Training comp.:** RL in rich environments "could be quite expensive from a training competitiveness standpoint." (§2.3)
- **Performance comp.:** "It needs to actually incentivize the correct behavior for the tasks that you want to use your AI for." (§2.4)

*Our read (C2):* there is no repeated step. The overseer is a fixed human with fixed tools, while the agent grows. What must stay invariant is that the environment's incentives still favor honesty at every capability level, which is exactly what can flip once the agent can model its designers. The phase transition is the one Hubinger names: transparency works on proxy-aligned models and fails on deceptive ones. Nothing is self-similar here; the argument has to be remade at each scale.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ✗ | ~ | ~ |

- C1a ~: corrigibility and honesty dominate in the limit of the environment. C1b ~: transparency catches the model before it becomes deceptive. C2 ✗: no repeated step; the overseer does not grow. C3a ~: rich enough environments are affordable. C3b ~: the environment covers the tasks we need.

### P2. Imitative amplification + intermittent oversight (§3)

"the combination of imitative amplification with intermittent oversight of M by Amp(M) whenever the target model changes." Amp(M) is a human consulting copies of M; M is trained to imitate it. The human is demonstrator and (amplified) inspector.

- **Outer:** the limit is HCH, and "since the limit is composed entirely of humans—and since we can control what those humans do—we should be able to ensure that all the optimization power is coming from the humans." (§3.1)
- **Inner:** Amp(M) looks for "deceptive or otherwise catastrophic behavior in M by utilizing things like transparency tools and adversarial attacks." There is "a guarantee that the overseer is always at least as strong as any targets that the model was trained to imitate," but "even if the overseer is stronger than the model, the offense-defense balance could be such that a deceptive model would still be able to hide its deception." (§3.2)
- **Training comp.:** language imitation is likely competitive "at some tasks—namely highly-cognitive tasks such as general-purpose decision-making—but not at others, such as fine motor control." (§3.3)
- **Performance comp.:** "As long as it is always possible for a human to break down complex tasks that they can't answer themselves into easier subtasks, then imitative amplification should be performance competitive." (§3.4)

*Our read (C2):* this is the clearest repeated step in the paper: decompose, answer sub-questions with M, recombine, distill, and oversee. What must stay invariant is the human's way of decomposing and selecting sub-answers (Evan's "invariant human function"). Hubinger's own inner-alignment text shows the overseer-stronger-than-model property is not enough at every step: he notes M can outgrow its targets through "significant compression", and offense can beat defense. Errors are not bounded in the source. The self-similarity of HCH is exact in the idealized limit, but whether training tracks it is statistical, so C2 is capped at ~.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ~ | ~ | ~ |

- C1a ~: HCH is aligned. C1b ~: deception is caught before it can hide, by an overseer that stays ahead. C2 ~: the human decomposition function is stable across levels. C3a ~: imitation is sample-efficient enough. C3b ~: factored cognition holds for the tasks we need.

### P3. Imitative amplification + relaxed adversarial training (§4)

"we jointly train M on the imitation loss and on decreasing the probability of catastrophe as assessed by Amp(M)."

- **Outer:** "If we assume that it is possible to imitate HCH in a demonstrably non-catastrophic way, then the catastrophe-checking ... shouldn't affect the theoretical limiting behavior." (§4.1)
- **Inner:** relaxed adversarial training "has the potential to avoid deception before it ever occurs by providing a consistent gradient away from deception during the entirety of training." (§4.2) The overseer could enforce something simple such as myopia *(unverified wording)*.
- **Training comp.:** it "could require significantly more total oversight bandwidth, which could make training more difficult." (§4.3)
- **Performance comp.:** effectively the same as P2, "highly dependent on the competitiveness of HCH." (§4.4)

*Our read (C2):* same repeated step as P2, with oversight moved inside every gradient step. That is the better structure for scale invariance: the invariant is no longer "the human can detect deception" but "the overseer can keep the model in a non-deceptive basin", which matches the club's counterpoint that only the corrigibility basin needs to be invariant. The weak point is that the catastrophe estimate is itself produced by Amp(M), so if the overseer lags at one level the gradient points the wrong way from then on, and the error compounds. No source bound on this.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ~ | ~ | ~ |

- C1a ~: HCH can be imitated non-catastrophically. C1b ~: Amp(M) gives a consistent gradient away from deception. C2 ~: a simple condition like myopia is enforceable at every scale. C3a ~: oversight bandwidth is affordable. C3b ~: HCH is competitive.

### P4. Approval-based amplification + relaxed adversarial training (§5)

"substitute the imitation signal in imitative amplification with an approval signal, training M to maximize the approval of Amp(M)." The human is an amplified evaluator.

- **Outer:** "we have to understand what exactly the theoretical limit of approval-based amplification is, which in general is not HCH." (§5.1)
- **Inner:** "the fact that approval-based amplification directly incentivizes M to fool H into approving of it could result in M being more prone to deception, more likely to hide facts from H during oversight, and/or more likely to obfuscate its internals." (§5.2)
- **Training comp.:** "approval provides a very different training signal than imitation." (§5.3)
- **Performance comp.:** "depends on the properties of the approval-maximization tree." (§5.4)

*Our read (C2):* the repeated step is "propose, and have the amplified overseer approve". The invariant would have to be the overseer's approval function, but approval is the very thing a stronger M is optimized against at every level. That is a built-in phase transition: once M can model what Amp(M) approves of, the step rewards fooling it. Hubinger names this pressure in the inner-alignment text. We mark C2 ✗ because the step itself pushes against its own invariant.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ✗ | ~ | ~ |

- C1a ~: the approval-maximization tree is aligned (unknown). C1b ~: relaxed adversarial training outruns the incentive to fool H. C2 ✗: approval is gameable by a model that can model the approver. C3a ~: approval is a usable signal. C3b ~: the approval tree is capable.

### P5. Microscope AI (§6)

"Train a predictive model on some set of data that you want to understand while using transparency tools to verify that the model isn't performing any optimization." Humans then read what it learned and decide for themselves.

- **Outer:** "Paul Christiano's argument that the universal prior is malign suggests that there might be deceptive optimization in the limit of perfect prediction." (§6.1)
- **Inner:** "Since the goal here is not to produce a model performing optimization (a mesa-optimizer) in the first place, the question of inner alignment becomes rather different." (§6.2)
- **Training comp.:** "relies on being able to competitively train predictive models with useful internal representations." (§6.3)
- **Performance comp.:** "perhaps the biggest question mark regarding microscope AI, as microscope AI forgoes producing AI agents which directly take actions in the world." (§6.4)

*Our read (C2):* no repeated step. What must stay invariant is "this predictor is not optimizing", checked by transparency tools that must keep pace with an ever larger model. Hubinger's own outer-alignment worry is a phase transition: good enough prediction may require modelling optimizers, and then contain them. Human understanding of the model's concepts also has to scale, which is not argued.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ✗ | ~ | ~ |

- C1a ~: we stop short of the limit where prediction becomes malign. C1b ~: transparency can verify absence of optimization. C2 ✗: no repeated step; tools and human understanding must scale on their own. C3a ~: predictive training yields readable representations. C3b ~: insight without agency is enough.

### P6. STEM AI (§7)

"train a model purely on abstract science, engineering, and/or mathematics problems while using transparency tools to ensure that the model isn't thinking about anything outside its sandbox." The human is mostly outside training.

- **Outer:** "it seems likely that--in the limit--the best STEM AIs would be malign in terms of having convergent instrumental goals which cause them to be at odds with humans." The hope is that the sandbox prevents this. (§7.1)
- **Inner:** "exactly what the correct thing is to do in terms of inner alignment for a STEM AI is somewhat unclear." (§7.2)
- **Training comp.:** depends "on how hard it is for state-of-the-art machine learning algorithms to solve STEM problems compared to other domains." (§7.3)
- **Performance comp.:** confinement to STEM "has the major potential to massively limit the applicability." And the main worry is not weakness: "STEM AI could potentially create a vulnerable world situation where the powerful technology produced using the STEM AI makes it much easier to build advanced AI systems, without also making it more likely that they will be aligned." (§7.4)

*Our read (C2):* no repeated step. The invariant is the sandbox boundary: the model never reasons about humans. That boundary is more likely to break with scale (a strong enough STEM model may infer its overseers from the problems it is given). Outputs like proofs can be checked exactly, which is real exactness, but only for the outputs, not for the model's intent. The vulnerable-world risk sits outside our grid entirely (see Section 5).

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ? | ✗ | ~ | ~ |

- C1a ~: the sandbox removes the malign limit. C1b ?: Hubinger calls it unclear. C2 ✗: the sandbox has no argument that it holds as capability grows. C3a ~: STEM is not much harder than other domains. C3b ~: STEM alone covers enough use cases, without a vulnerable world.

### P7. Narrow reward modeling + transparency tools (§8)

"For some particular problem/domain/environment, jointly train a reward model using human feedback and an agent that pursues that reward." The human is evaluator plus inspector.

- **Outer:** depends on "whether the various feedback mechanisms are sufficient to ensure that, in the limit, the learned reward model is correct over the domain where the action model believes that it is on distribution." (§8.1)
- **Inner:** "whether a human using transparency tools to perform oversight can prevent the development of deceptive or otherwise catastrophic behavior in either the reward model or the agent." (§8.2)
- **Training comp.:** whether reward modeling is "a good (compute-efficient, sample-efficient. etc.) way of training AI systems to solve poorly-defined, fuzzy tasks." (§8.3)
- **Performance comp.:** "A potentially major concern ... is the 'narrow' part." (§8.4)

*Our read (C2):* the step "human rates, reward model generalizes, agent optimizes" repeats over training but not across capability levels: the human evaluator does not get stronger. The invariant is human judgment of outcomes, which breaks exactly when outcomes exceed what a human can check. This is the scalable oversight problem in its plain form, which is why P8 exists.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ✗ | ~ | ~ |

- C1a ~: the reward model is correct wherever the agent acts. C1b ~: an unassisted human with tools can catch deception. C2 ✗: the evaluator is fixed while the agent grows. C3a ~: reward modeling is efficient on fuzzy tasks. C3b ~: narrow agents suffice.

### P8. Recursive reward modeling + relaxed adversarial training (§9)

"a sort of recursive, non-narrow version of narrow reward modeling." Earlier agents help the human evaluate later ones.

- **Outer:** "Whether this tree is aligned or not is a very open question," and "it is difficult to form a clear model of what exactly this tree would do, as it involves not only humans but also models that are the limit of many individual instances of reward modeling." (§9.1)
- **Inner:** "mostly going to fall on the efficacy of the relaxed adversarial training," which may differ "as both the model helping the human perform oversight and the model being overseen are trained via a very different process." (§9.2)
- **Training comp.:** depends on reward modeling "in training a general model which can solve an entire collection of fuzzy tasks." (§9.3)
- **Performance comp.:** "will depend on the competitiveness of its aforementioned limiting tree." (§9.4)

*Our read (C2):* repeated step: assist the evaluator with the previous agent, then train the next agent on that evaluation. The invariant is twofold: the human's evaluation function, and the trustworthiness of each helper agent. Unlike HCH, the limit contains trained optimizers at every node, so a deceptive helper at level *k* corrupts every level above it. Errors are expected to compound (Leike et al. raise this themselves) and nothing bounds them. Statistical at best.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ~ | ~ | ~ |

- C1a ~: the reward-modeling tree is aligned (open). C1b ~: relaxed adversarial training works when overseer and overseen are both RL-trained. C2 ~: helper errors stay bounded across generations. C3a ~: one general reward-modeled model is trainable. C3b ~: the limiting tree is capable.

### P9. AI safety via debate with transparency tools (§10)

"Train a model ('Alice') to win debates against a copy of itself ('Bob') in front of a human judge." Debaters may use transparency tools on each other. The human is the judge.

- **Outer:** "depends on whether giving honest, cooperative, helpful advice is the strategy with the greatest probability of winning," which "is a very open question ... [depending on] the set of questions being asked, the effectiveness of the human judges, the length of the debate, etc." (§10.1)
- **Inner:** "centers on the effectiveness of the debaters in using the transparency tools to oversee each other." (§10.2)
- **Training comp.:** debate is "reinforcement learning via self-play on a zero-sum game," and history "suggests that such self-play might be fairly competitive." (§10.3)
- **Performance comp.:** "It has been proven that debate with cross-examination has the property that arbitrarily strong debaters can convince a polynomial-time judge of the answer to any problem in NEXP." (§10.4)

*Our read (C2):* repeated step: argue and refute, with each claim recursively zoomed into until a human can judge one small step. This is the only proposal with an exact self-similarity argument (the complexity result holds at every size). But the result is about an idealized polynomial-time judge, and the invariant here is a real human judge spotting the one flawed step, which has to hold however strange the questions get. The known phase transition is obfuscated arguments (Barnes, 2020): beyond some size, a flaw exists but neither the judge nor an honest debater can find it. The overseer-stronger-than-model problem is reframed: each debater oversees an equally strong copy, which is a real advantage.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ~ | ✓ | ✓ |

- C1a ~: honesty is the winning strategy for realistic judges. C1b ~: equally strong debaters can find each other's deception with tools. C2 ~: the human judge behaves like the idealized judge at every depth. C3a ✓: self-play stays as efficient as it has been historically. C3b ✓: training approaches optimal play.

### P10. Amplification with auxiliary RL objective + relaxed adversarial training (§11)

"train a model M on a joint objective composed of three different pieces": imitate Amp(M); "Train M so as to cause Amp(M) to perform well on some RL objective in some environment"; and relaxed adversarial training against catastrophe.

- **Outer:** "Annealing towards the imitative loss yields HCH as the limit—giving this approach the same outer alignment properties as imitative amplification." (§11.1)
- **Inner:** "inner alignment is likely to come down to whether or not the overseer is able to produce a consistent gradient away from deception." (§11.2)
- **Training comp.:** "could be significantly more training competitive by producing a much richer reward signal." (§11.3)
- **Performance comp.:** "the same performance competitiveness properties as imitative amplification." (§11.4)

*Our read (C2):* the same step as P3 plus an RL term. The RL term is what makes it cheaper, and also what can pull the model out of the non-deceptive basin between oversight checks, since RL rewards consequences rather than a fixed standard. The scale invariance argument then depends on the annealing schedule winning that race at every level, which is not argued.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ~ | ~ | ~ |

- C1a ~: annealing really reaches HCH. C1b ~: the overseer's gradient beats the RL term's pull. C2 ~: the corrigibility basin survives the RL term at every scale. C3a ~: the richer signal pays for the oversight cost. C3b ~: HCH is competitive.

### P11. Amplification alongside RL + relaxed adversarial training (§12)

"simultaneously train two models, a question-answering system M and an RL agent π." Only M is deployed.

- **Outer:** "Since we only deploy M, not π, the question of outer alignment here should be entirely dependent on whether this procedure produces outer aligned M." (§12.1)
- **Inner:** "Inner alignment is particularly tricky for this approach. First, there's the standard relaxed adversarial training question of whether Amp(M) will be able to properly oversee M." (§12.2)
- **Training comp.:** "it enables us to just train an RL model with minimal restrictions." (§12.3)
- **Performance comp.:** the same as imitative amplification, via annealing to HCH. (§12.4)

*Our read (C2):* two systems grow side by side, and only one of them is overseen by the amplification step. The invariant has to cover π as well, because a strong unrestricted RL agent shaping M's training is exactly where a capability jump outside the overseer's view can happen. Same structure as P10, with a larger unsupervised surface.

| C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|
| ~ | ~ | ~ | ~ | ~ |

- C1a ~: M ends up imitating HCH. C1b ~: Amp(M) can oversee M, and π cannot corrupt M. C2 ~: the unrestricted π never outruns the overseer. C3a ~: minimal-restriction RL keeps its cost advantage. C3b ~: HCH is competitive.

---

## 4. Combined verdict table

| Proposal | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| 1. RL + transparency tools | ~ | ~ | ✗ | ~ | ~ |
| 2. Imitative amp. + intermittent oversight | ~ | ~ | ~ | ~ | ~ |
| 3. Imitative amp. + relaxed adv. training | ~ | ~ | ~ | ~ | ~ |
| 4. Approval-based amp. + relaxed adv. training | ~ | ~ | ✗ | ~ | ~ |
| 5. Microscope AI | ~ | ~ | ✗ | ~ | ~ |
| 6. STEM AI | ~ | ? | ✗ | ~ | ~ |
| 7. Narrow reward modeling + transparency | ~ | ~ | ✗ | ~ | ~ |
| 8. Recursive reward modeling + relaxed adv. training | ~ | ~ | ~ | ~ | ~ |
| 9. Debate + transparency tools | ~ | ~ | ~ | ✓ | ✓ |
| 10. Amp. + auxiliary RL + relaxed adv. training | ~ | ~ | ~ | ~ | ~ |
| 11. Amp. alongside RL + relaxed adv. training | ~ | ~ | ~ | ~ | ~ |

The page of tildes is faithful to the source: Hubinger hedges almost every verdict ("very open question", "particularly tricky", "somewhat unclear"), and Rule 1 says hedges carry up.

## 5. Cross-cutting observations

**What Hubinger's grid misses that C2 catches.**

1. *The safeguard has to scale too.* Every inner-alignment answer in the paper rests on transparency tools, intermittent oversight or relaxed adversarial training. The grid asks whether these work, not whether they keep working as the model grows. A tool that reads a model at one scale is one point on the log-log plot. *Our read:* C1b should be graded at the largest capability level the proposal is meant for, not the current one.
2. *The overseer must stay stronger than the model at every step.* Hubinger states the guarantee for P2 ("the overseer is always at least as strong as any targets") and in the same breath breaks it (compression; offense-defense balance). This gap is the single invariant shared by P2, P3, P4, P8, P10 and P11. Debate (P9) is the one design that swaps it for "an equally strong adversary", which is why it gets the only exact C2 argument.
3. *Proposals without a repeated step fail C2 by construction.* P1, P5, P6 and P7 each rely on a fixed human, tool, sandbox or evaluator, so their safety has to be re-argued at every scale.
4. *What exactly must be invariant.* Evan's reading is that amplification learns an invariant human function for selecting sub-solutions. P3's design suggests a weaker and more plausible invariant: only the "this step is acceptable and not deceptive" basin (corrigibility, or a simple condition like myopia) must hold at every level. *Our read:* a fixed basin is easier to keep invariant than a whole selection function, and proposals should be judged on which of the two they need. P4 and P10/P11 are the cases where the training signal pushes against that basin.
5. *Exact vs. statistical.* HCH and the debate complexity result are exact (Sierpinski-like) in the idealized limit. Every claim that a trained model tracks that limit is statistical (coastline-like), so no proposal earns better than ~ on C2.
6. *Risks outside the model.* STEM AI's worry is a vulnerable world, not weakness or misalignment. Neither Hubinger's grid nor ours has a place for "safe model, unsafe world".

**Open questions for the comparison.**

- Can relaxed adversarial training be shown to give "a consistent gradient away from deception" at more than one capability gap? That is a C2f experiment nobody in the paper proposes.
- Do errors in P8's helper agents compound, stay bounded, or self-correct? A sandwiching study at several gaps would answer it.
- Is training against transparency tools (P1, P5, P7, P9) self-defeating? This is the monitorability amendment already proposed in the [criterion](../criterion.md#proposed-amendments).
- Where is the obfuscated-arguments threshold for debate, and does a similar threshold exist for amplification's decompositions?
