# Scalable Oversight, Discussion 2: Background Primer

A short introduction to the ideas behind the [discussion topics](discussion-2-topics.md). Read this first if the priority readings are new to you. Discussion 1 ([history.md](history.md)) covered the historical roots; this primer covers the modern proposals.

---

## 1. The problem

**Scalable oversight** is the problem of supervising an AI system on tasks where humans can't easily tell whether its output is good. Today we train models largely by having people rate their outputs. That works when a person can check the answer. It breaks down when the task is a 10,000-line code change, a new proof, or a research direction, and it breaks down further once the model is better at the task than any rater.

**Why it matters now.** Anthropic's [When AI builds itself](https://www.anthropic.com/institute/recursive-self-improvement) (2026) reports that Claude writes most of Anthropic's production code and that human review is becoming the bottleneck. That is a concrete version of the old worry about *recursive self-improvement*: AI that improves the next AI faster than people can check it. The question for this session is which proposals could keep humans meaningfully in control of that loop.

---

## 2. Background concepts

**Reward model.** A model trained to predict which outputs a human would prefer. The AI is then trained with reinforcement learning to score highly on it. This is the core of RLHF (reinforcement learning from human feedback). Its weakness: the reward model is only as good as the human judgments it learned from, and a strong optimizer will find outputs that fool it (*reward hacking*).

**Outer vs. inner alignment** (Hubinger's framing).
- *Outer alignment*: is the training objective itself what we want? If we optimized it perfectly, would the result be safe?
- *Inner alignment*: does the model we actually trained pursue that objective, or did it learn some other goal that happened to score well during training? A model that learned its own goals is a *mesa-optimizer*. The worst case is *deceptive alignment*: a model that behaves well because it knows it is being trained or watched.

**Competitiveness.** A safe method only matters if people will use it.
- *Training competitiveness*: is it affordable and practical to train?
- *Performance competitiveness*: is the finished system about as capable as an unsafe alternative?

These four (outer alignment, inner alignment, training and performance competitiveness) are the criteria Hubinger uses to grade all 11 proposals in [An overview of 11 proposals for building safe advanced AI](https://arxiv.org/abs/2012.07532) (2020). We use them as our comparison grid.

**Transparency tools and relaxed adversarial training.** Hubinger usually pairs each proposal with a way to address inner alignment. *Transparency* (interpretability) means looking inside the model to check what it is doing. *Relaxed adversarial training* means an overseer searches for inputs, or descriptions of inputs, on which the model would behave badly, and trains against them.

---

## 3. Amplification: the shared idea behind RRM and debate

**Amplification** ([Christiano, Shlegeris & Amodei, 2018](https://arxiv.org/abs/1810.08575)) starts from a simple observation: a human who can ask a capable assistant for help can answer harder questions than a human alone. So:

1. A human, helped by copies of the current model, breaks a hard question into easier sub-questions and combines the answers. This "amplified" human is a better overseer than the human alone.
2. Train the model to imitate what the amplified human would answer (*distillation*).
3. Repeat. Each round, the overseer gets stronger along with the model.

The bet underneath is **factored cognition**: that hard reasoning can be broken into small pieces, each of which a human can check. The paper tests this on algorithmic tasks where the full answer is easy to compute but is never shown to the learner directly.

---

## 4. The three proposals we compare

### Recursive reward modeling (RRM)
From [Leike et al., *Scalable agent alignment via reward modeling* (2018)](https://arxiv.org/abs/1811.07871). Train an agent on a reward model learned from human feedback. Then use that agent to help humans evaluate the next, harder task, and train a new reward model from those assisted evaluations. Example: to judge a design for a computer chip, a human might use earlier agents that check timing, summarize test results and flag risky components.
- *Relation to amplification:* similar recursive structure, but it trains agents with reward rather than training a model to imitate a human.
- *Main worry:* errors can compound. Each generation of helper trains on the last generation's judgments, and a subtle flaw can be passed along and amplified.

### AI safety via debate
From [Irving, Christiano & Amodei (2018)](https://arxiv.org/abs/1805.00899). Two copies of a model argue about a question in turns. A human judges who gave the most true and useful information. The models are trained to win.
- *Core bet:* in a debate, it is harder to lie than to refute a lie. If that holds, the winning strategy is honesty, even if the judge could never have found the answer alone.
- *Complexity theory argument:* the paper argues that a judge who can only check short arguments (roughly, NP problems) can, with optimal debate, correctly decide much harder questions (PSPACE).
- *Toy experiment:* an image classifier that sees only a few pixels of an MNIST digit becomes much more accurate when two debaters take turns revealing pixels to argue for different labels.
- *Main worries:* human judges can be persuaded by rhetoric instead of truth. A dishonest debater may also build an argument too long or complex for the honest side to find the flaw (the *obfuscated arguments problem*, raised by later work at OpenAI).

### STEM AI (and world models)
Proposal 5 in Hubinger's list. Train a model only on narrow mathematical and scientific problems, in a sandbox, with no data about humans. A model that knows nothing about people can't learn to manipulate them or its overseers.
- *Strengths:* it sidesteps much of the oversight problem. Proofs and many scientific results can be checked independently.
- *Main worry:* performance competitiveness. A STEM-only model can't help with policy, governance or aligning other AIs, which may be what we most need help with. Keeping humans out of the training data entirely may also be hard in practice.
- *"World models":* Hubinger's list doesn't use this term. We use it for the broader idea of an AI that models the world and answers questions without acting in it, sometimes called an *oracle* or *tool AI*. The closest proposal in his list is **microscope AI** (proposal 4): train a predictive model, then use interpretability to learn from what it knows, instead of deploying it as an agent.

---

## 5. At a glance

| | Where the human sits | What it bets on | Main failure mode |
|---|---|---|---|
| **RRM** | Evaluator, helped by earlier agents | Evaluation is easier than doing the task, and assistance scales | Errors compound across generations |
| **Debate** | Judge between two adversaries | Refuting a lie is easier than telling one | Persuasion over truth; obfuscated arguments |
| **STEM AI** | Mostly outside training | A model that doesn't model humans can't manipulate them | Not useful enough for the problems that matter |

---

## 6. Glossary

- **Agent vs. tool/oracle:** an agent takes actions to achieve goals; a tool or oracle only answers questions.
- **Distillation:** training a fast model to copy the outputs of a slower, stronger process.
- **HCH** ("Humans Consulting HCH"): the idealized limit of amplification, a tree of humans each able to consult more such trees.
- **Mesa-optimizer:** a learned model that is itself an optimizer, with its own objective.
- **RLHF:** reinforcement learning from human feedback.
- **Weak-to-strong generalization:** a related research direction (OpenAI, 2023) asking whether a weak supervisor's labels can still teach a strong model the right behavior.

## Further reading

- Hubinger, [An overview of 11 proposals for building safe advanced AI](https://arxiv.org/abs/2012.07532) (2020).
- Leike et al., [Scalable agent alignment via reward modeling](https://arxiv.org/abs/1811.07871) (2018).
- Hubinger et al., [Risks from Learned Optimization](https://arxiv.org/abs/1906.01820) (2019), for inner alignment and mesa-optimizers.
- Amodei et al., [Concrete Problems in AI Safety](https://arxiv.org/abs/1606.06565) (2016), which first named scalable oversight as an open problem.
