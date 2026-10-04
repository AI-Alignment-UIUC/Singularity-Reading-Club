# Scalable Oversight, Discussion 2: Background Primer

A short introduction to the ideas behind the [discussion topics](discussion-2-topics.md). Read this first if the priority readings are new to you. Discussion 1 ([history.md](history.md)) covered the historical roots; this primer covers the modern proposals.

Quotes are taken from the papers themselves, with the section they appear in. They were pulled from text extractions of the arXiv PDFs (and, for Hubinger, checked against the Alignment Forum version), so check the PDF before putting one on a slide.

---

## 1. The problem

**Scalable oversight** is the problem of supervising an AI system on tasks where humans can't easily tell whether its output is good. Amodei et al. named it in *Concrete Problems in AI Safety* (2016) with a cleaning-robot example:

> "We may want the agent to maximize a complex objective like 'if the user spent a few hours looking at the result in detail, how happy would they be with the agent's performance?' But we don't have enough time to provide such oversight for every training example; in order to actually train the agent, we need to rely on cheaper approximations"

Today we train models largely by having people rate their outputs. That works when a person can check the answer. It breaks down when the task is a 10,000-line code change, a new proof, or a research direction, and it breaks down further once the model is better at the task than any rater.

**Why it matters now.** Anthropic's [When AI builds itself](https://www.anthropic.com/institute/recursive-self-improvement) (2026) reports:

> "As of May 2026, more than 80% of the code we merge into Anthropic's codebase was authored by Claude."

> "As we've begun to push more code around the organization, human code review has become a new bottleneck."

It also names the risk of *recursive self-improvement*:

> "The rare occurrences of misalignment present in today's models could compound as the models build their successors, growing more frequent but less understood until we lose control of them."

The question for this session is which proposals could keep humans meaningfully in control of that loop.

---

## 2. Background concepts

**Reward model.** A model trained to predict which outputs a human would prefer. The AI is then trained with reinforcement learning to score highly on it. This is the core of RLHF (reinforcement learning from human feedback). Leike et al. (2018, §3.1) describe it:

> "The user trains a reward model to learn their intentions by providing feedback. This reward model provides rewards to a reinforcement learning agent that interacts with the environment."

Its weakness: the reward model is only as good as the human judgments it learned from, and a strong optimizer will find outputs that fool it (*reward hacking*).

**Outer vs. inner alignment.** Hubinger (2020) defines them as:

> "**Outer alignment:** Outer alignment is about asking why the objective we're training for is aligned—that is, if we actually got a model that was trying to optimize for the given loss/reward/etc., would we like that model?"

> "**Inner alignment:** Inner alignment is about asking the question of how our training procedure can actually guarantee that the model it produces will, in fact, be trying to accomplish the objective we trained it on."

Inner alignment fails when the trained model learns its own goal. *Risks from Learned Optimization* (Hubinger et al., 2019) calls such a model a *mesa-optimizer*: what you get when "a base optimizer (in searching for algorithms to solve some problem) finds a model that is itself an optimizer." The worst case is *deceptive alignment*, where the model learns "that the base optimizer will modify the mesa-optimizer if the mesa-optimizer scores poorly on the base objective" and so behaves well during training whatever it actually wants.

**Competitiveness.** A safe method only matters if people will use it. Hubinger's two criteria:
- *Training competitiveness*: "whether the given training procedure is one that a team or group of teams with a reasonable lead would be able to afford to implement without completely throwing away that lead."
- *Performance competitiveness*: "whether a particular proposal, if successful, would satisfy the use cases for advanced AI."

These four (outer alignment, inner alignment, training and performance competitiveness) are the criteria Hubinger uses to grade all 11 proposals in [An overview of 11 proposals for building safe advanced AI](https://arxiv.org/abs/2012.07532). We use them as our comparison grid.

**Transparency tools and relaxed adversarial training.** Hubinger pairs most proposals with a way to address inner alignment. *Transparency* (interpretability) means looking inside the model to check what it is doing. *Relaxed adversarial training* means an overseer searches for inputs, or descriptions of inputs, on which the model would behave badly, and trains against them.

**Hubinger's 11 proposals**, for reference (ours are in bold):
1. Reinforcement learning + transparency tools
2. Imitative amplification + intermittent oversight
3. Imitative amplification + relaxed adversarial training
4. Approval-based amplification + relaxed adversarial training
5. Microscope AI
6. **STEM AI**
7. Narrow reward modeling + transparency tools
8. **Recursive reward modeling + relaxed adversarial training**
9. **AI safety via debate with transparency tools**
10. Amplification with auxiliary RL objective + relaxed adversarial training
11. Amplification alongside RL + relaxed adversarial training

---

## 3. Amplification: the shared idea behind RRM and debate

[Christiano, Shlegeris & Amodei (2018)](https://arxiv.org/abs/1810.08575) start from the same gap: "there are harder tasks for which we can't compute demonstrations or rewards even with human assistance." Their answer, iterated amplification, "progressively builds up a training signal for difficult problems by combining solutions to easier subproblems" (abstract). Concretely (§1.1):

> "Amplify<sup>H</sup>(X) answers a question Q by having H identify a sequence of useful subquestions, using X to compute a subanswer to each subquestion"

Here H is the human and X is the current model. The loop:
1. A human, helped by copies of the current model, breaks a hard question into easier sub-questions and combines the answers. This "amplified" human is a better overseer than the human alone.
2. Train the model to imitate what the amplified human would answer (*distillation*).
3. Repeat. Each round, the overseer gets stronger along with the model.

The bet underneath is **factored cognition**: that hard reasoning can be broken into small pieces, each of which a human can check. The paper tests this on "a set of 5 toy algorithmic tasks" (§4) and reports that "Iterated Amplification is able to solve these tasks effectively with at worst a modest slowdown" (§4.2).

---

## 4. The three proposals we compare

### Recursive reward modeling (RRM)
From [Leike et al., *Scalable agent alignment via reward modeling* (2018)](https://arxiv.org/abs/1811.07871), which frames the goal as: "How can we create agents that behave in accordance with the user's intentions?" (§2). RRM applies reward modeling recursively (§3.2):

> "In step k we use the agent A<sub>k-1</sub> to assist the user in evaluating outcomes when training agent A<sub>k</sub>."

Their example is evaluating a novel, where "the user is assisted by an agent that provides auxiliary input: extracting a summary of the plotline, checking spelling and grammar, summarizing character development."

Hubinger (proposal 8) calls it "a sort of recursive, non-narrow version of narrow reward modeling."
- *Relation to amplification:* similar recursive structure, but it trains agents with reward rather than training a model to imitate a human.
- *Main worry:* errors can compound. Leike et al. flag it themselves (§3.2): "An important open question is whether errors accumulate: do the mistakes of the more narrow agent A<sub>k-1</sub> lead to larger mistakes in the training of agent A<sub>k</sub>?"

### AI safety via debate
From [Irving, Christiano & Amodei (2018)](https://arxiv.org/abs/1805.00899). The game (§2):

> "A question q ∈ Q is shown to both agents. The two agents state their answers a<sub>0</sub>, a<sub>1</sub> ∈ A. The two agents take turns making statements s<sub>0</sub>, s<sub>1</sub>, . . . , s<sub>n−1</sub> ∈ S. The judge sees the debate (q, a, s) and decides which agent wins."

Hubinger's version (proposal 9): "Train a model ('Alice') to win debates against a copy of itself ('Bob') in front of a human judge."
- *Core bet* (§2): "In the debate game, it is harder to lie than to refute a lie." If that holds, honesty is the winning strategy, even if the judge could never have found the answer alone.
- *Complexity theory argument* (§2.2): "For any problem L ∈ PSPACE, we can choose a polynomial time judge so that optimal play in the debate game with polynomially many steps solves L." In other words, a judge who can only check short arguments can, in principle, decide much harder questions.
- *Toy experiment* (§3.1): a classifier that sees only a few pixels of an MNIST digit, with two debaters revealing pixels to argue for different labels. "With 6 pixels, debate with precommit turns a 59.4% accurate judge into a 88.9% accurate debate."
- *Main worries:* the authors ask (§5.2), "Will one of the agents be able to sneak in a statement that appears logical but contains a hidden flaw without the other agent pointing out the flaw?" Beth Barnes later named a version of this the *obfuscated arguments problem* ([Alignment Forum, Dec 2020](https://www.alignmentforum.org/posts/PJLABqQ962hZEqhdB/debate-update-obfuscated-arguments-problem)): "a dishonest debater can often create arguments that have a fatal error, but where it is very hard to locate the error."

### STEM AI (and world models)
Proposal 6 in Hubinger's list:

> "Specifically, the idea of STEM AI is to train a model purely on abstract science, engineering, and/or mathematics problems while using transparency tools to ensure that the model isn't thinking about anything outside its sandbox."

A model that knows nothing about people can't learn to manipulate them or its overseers.
- *Strengths:* it sidesteps much of the oversight problem. Proofs and many scientific results can be checked independently.
- *Main worry:* Hubinger's concern is less that it's too weak than that it's lopsided: "STEM AI could potentially create a vulnerable world situation where the powerful technology produced using the STEM AI makes it much easier to build advanced AI systems, without also making it more likely that they will be aligned." It also can't help with policy, governance or alignment research directly.
- *"World models":* Hubinger's list doesn't use this term. We use it for the broader idea of an AI that models the world and answers questions without acting in it, sometimes called an *oracle* or *tool AI*. The closest proposal in his list is **microscope AI** (proposal 5): "Train a predictive model on some set of data that you want to understand, while using transparency tools to verify that the model isn't performing any optimization. Use transparency tools to understand what the model learned about the data and use that understanding to guide human decision-making."

---

## 5. At a glance

| | Where the human sits | What it bets on | Main failure mode |
|---|---|---|---|
| **RRM** | Evaluator, helped by earlier agents | Evaluation is easier than doing the task, and assistance scales | Errors compound across generations |
| **Debate** | Judge between two adversaries | Refuting a lie is easier than telling one | Persuasion over truth; obfuscated arguments |
| **STEM AI** | Mostly outside training | A model that doesn't model humans can't manipulate them | Speeds up AI capability without helping alignment |

---

## 6. Glossary

- **Agent vs. tool/oracle:** an agent takes actions to achieve goals; a tool or oracle only answers questions.
- **Distillation:** training a fast model to copy the outputs of a slower, stronger process.
- **HCH** ("Humans Consulting HCH"): the idealized limit of amplification, a tree of humans each able to consult more such trees.
- **Mesa-optimizer:** a learned model that is itself an optimizer, with its own objective.
- **RLHF:** reinforcement learning from human feedback.
- **Weak-to-strong generalization:** a related research direction (OpenAI, 2023) asking whether a weak supervisor's labels can still teach a strong model the right behavior.

## Sources quoted

- Amodei et al., [Concrete Problems in AI Safety](https://arxiv.org/abs/1606.06565) (2016).
- Anthropic Institute, [When AI builds itself](https://www.anthropic.com/institute/recursive-self-improvement) (2026).
- Barnes, [Debate update: Obfuscated arguments problem](https://www.alignmentforum.org/posts/PJLABqQ962hZEqhdB/debate-update-obfuscated-arguments-problem) (2020).
- Christiano, Shlegeris & Amodei, [Supervising strong learners by amplifying weak experts](https://arxiv.org/abs/1810.08575) (2018).
- Hubinger, [An overview of 11 proposals for building safe advanced AI](https://arxiv.org/abs/2012.07532) (2020); also on the [Alignment Forum](https://www.alignmentforum.org/posts/fRsjBseRuvRhMPPE5/an-overview-of-11-proposals-for-building-safe-advanced-ai).
- Hubinger et al., [Risks from Learned Optimization](https://arxiv.org/abs/1906.01820) (2019).
- Irving, Christiano & Amodei, [AI Safety via Debate](https://arxiv.org/abs/1805.00899) (2018).
- Leike et al., [Scalable agent alignment via reward modeling](https://arxiv.org/abs/1811.07871) (2018).
