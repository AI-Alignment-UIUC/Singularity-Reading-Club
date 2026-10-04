# Scalable Oversight, Discussion 2: Comparing Three Proposals

New to these ideas? Start with the [primer](discussion-2-primer.md).

Discussion 1 ([history.md](history.md)) deliberately stayed away from current methods. This session brings them in. We compare three of the proposals in Evan Hubinger's *An overview of 11 proposals for building safe advanced AI* (2020, [arXiv:2012.07532](https://arxiv.org/abs/2012.07532)):

- **Recursive reward modeling (RRM)**: train a reward model from human feedback, then use agents trained on it to help humans evaluate the next, harder task. Hubinger pairs it with relaxed adversarial training (proposal 8).
- **AI safety via debate**: two copies of a model argue opposite sides and a human judges who was more honest and helpful. Hubinger pairs it with transparency tools (proposal 9).
- **STEM AI / world models**: train a model only on narrow math and science problems in a sandbox, with no data about humans, so it never learns to model or manipulate us (proposal 6). We stretch this to the broader idea of building a model of the world rather than an agent acting in it (compare Hubinger's microscope AI, proposal 5).

## Priority readings

1. [When AI builds itself](https://www.anthropic.com/institute/recursive-self-improvement) (Anthropic Institute, 2026). Claude now writes most of Anthropic's production code, and human review is becoming the bottleneck. This is the motivating case: oversight of AI that improves AI.
2. [Supervising strong learners by amplifying weak experts](https://arxiv.org/abs/1810.08575) (Christiano, Shlegeris & Amodei, 2018). Iterated amplification, the cousin of RRM: break a hard question into easier ones a human can judge.
3. [AI Safety via Debate](https://arxiv.org/abs/1805.00899) (Irving, Christiano & Amodei, 2018).

## Hubinger's four criteria (our comparison grid)

| | Outer alignment | Inner alignment | Training competitiveness | Performance competitiveness |
|---|---|---|---|---|
| **RRM** | | | | |
| **Debate** | | | | |
| **STEM AI** | | | | |

*Outer alignment*: would the training objective produce a safe model if optimized perfectly? *Inner alignment*: does the model we actually get pursue that objective? *Training competitiveness*: is it affordable to train? *Performance competitiveness*: is the result as useful as an unsafe alternative?

Suggested exercise: fill in the grid as a group before the open discussion, then argue about the cells people disagree on.

## Discussion topics

1. **Where does the human sit?** In RRM the human is helped by earlier agents. In debate the human is a judge. In STEM AI the human is mostly absent from training. Which placement scales best as capability outruns us, and which fails most quietly?

2. **Decomposition vs. adversarial pressure.** RRM and amplification assume hard evaluations break into easy ones. Debate assumes it is easier to spot a lie than to find the truth. Which assumption do you find more plausible? Can you name a task where one holds and the other fails?

3. **Errors that compound.** In RRM each generation of helper trains on the last generation's judgments, so small errors can stack. "When AI builds itself" describes the same loop in the real world. Does debate avoid this, or does the judge just become the weak link instead?

4. **Is STEM AI oversight at all, or an escape from it?** STEM AI avoids the hard problem by never letting the model reason about humans. What do we give up? Hubinger's worry is that it could speed up building advanced AI "without also making it more likely that they will be aligned." A STEM-only model also can't help with governance. Could a world model that answers questions, without acting, be a middle ground?

5. **The recursive self-improvement test.** Take Anthropic's scenario, where AI writes most of the code for the next AI. Which of the three methods could you actually deploy there today? What would each need to work?

6. **Callback to Discussion 1.** Von Neumann's constructor reads its description twice: once as instructions and once as data copied without interpretation. Debate and RRM both ask a model to explain itself to an overseer. Is there an analogue of "copy without interpreting" that would let us check a system without trusting its own account of itself? (Gödel's limit on self-certification is relevant here.)

## Open question to close on

If you had to bet on one of the three for the first system that is clearly better than its developers at AI research, which would it be, and what result would change your mind?
