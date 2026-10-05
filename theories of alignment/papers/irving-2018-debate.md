# AI Safety via Debate (Irving et al., 2018)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Geoffrey Irving, Paul Christiano, Dario Amodei. *AI Safety via Debate.* arXiv:1805.00899 (2018). <https://arxiv.org/abs/1805.00899>
**Role.** Method. (Hubinger's proposal 9 is this method plus transparency tools; this summary grades the paper's own version.)
**Sources read.** The arXiv PDF, read through WebFetch with targeted prompts for verbatim passages and their section numbers. Two quotes on the debate game and the MNIST result were also checked in the [Discussion 2 primer](../../scalable%20oversight/discussion-2-primer.md). Nothing could not be opened. A WebFetch search for the words "transparency" and "interpretability" found no occurrences; we report that as a search result, not a certainty.
**Criterion version.** 1.0.

---

## The method in one paragraph

The paper's own statement (abstract):

> "Given a question or proposed action, two agents take turns making short statements up to a limit, then a human judges which of the agents gave the most true, useful information."

Two copies of a model are trained by self-play to win a zero-sum game (§2: "each agent maximizes their probability of winning"). The bet is that a human who cannot answer a hard question can still tell who won a debate about it, because any lie can be challenged and the debate zooms in on the single point of disagreement. A debate is one path through a tree of arguments (§2.1: "A single round of the debate game traces out one path through the space of all possible arguments. The reason for the answer is the entire tree"). The complexity-theory analogy says that with optimal play this lets a polynomial-time judge decide any problem in PSPACE, where direct judging reaches only NP (abstract, §2.2). The only experiment is a toy MNIST game with a sparse-pixel classifier as judge (§3.1).

---

## C1. Alignment

### C1a. Outer alignment: **~**
*Load-bearing assumption:* the central claim holds for real human judges, so that winning the debate and telling the truth coincide at equilibrium.

*Steelman.* The objective is "win in front of the judge", not "be approved of". The authors argue that this rewards truth because lies are cheaper to refute than to defend (§2):

> "Claim. In the debate game, it is harder to lie than to refute a lie."

> "In all Nash equilibria of this game, both agents try to tell the truth in the most convincing manner possible, trying to uncover details or counterarguments the other agent missed."

Theorem 1 (§2.2) makes this exact for an idealized judge: "For any problem L ∈ PSPACE, we can choose a polynomial time judge so that optimal play in the debate game with polynomially many steps solves L."

*Critique.* What the objective literally rewards is convincing a human. The authors say so themselves (§5.10): "It is not intuitively obvious that training for maximum convincing is safe, as it sounds similar to training for deception." The theorem uses an algorithmic judge, not a person (§2.2: "Although debate is intended for use with fuzzy humans as judges, we can gain intuition about the model by replacing the human with an arbitrary polynomial time algorithm"). Whether people judge well enough is left open (§5.2): "In short, can an actual human play the game well enough that optimal play is honest behavior?" Belief bias is named in §5.1: "If Alice opens with a false statement that the human already believes, will any argument by Bob convince them otherwise?"

*Our read:* outer alignment is argued, not shown, and it rests entirely on the judge. That earns ~, not ✓.

### C1b. Inner alignment: **✗**
*Load-bearing assumption:* self-play converges to the honest equilibrium rather than to a model with its own objective that merely wins.

*Steelman.* The adversary is a partial inner-alignment check on outputs: if one debater says something false, the other is paid to expose it. The authors hope this holds near the optimum too (§5.5): "in a neighborhood around optimal play both agents are trying to be honest and convincing, sometimes missing arguments but not intentionally misleading the human."

*Critique.* The authors hedge this immediately (§5.5): "This hope is far from a solid argument." On training dynamics (§5.6): "we do not know of any theory which says why self play should stably converge to optimal play," followed by a cycle in which agents become honest, forget how to point out flaws, and one "goes back to lying, and wins for a while." The paper predates the mesa-optimizer framing and has no component that inspects the model's internals; we found no mention of transparency or interpretability. Two copies of the same model may also share whatever hidden objective they learned, so the opponent is not an independent check. *Our read:* deceptive alignment applies and the paper does not address it, hence ✗. Hubinger's proposal 9 adds transparency tools for exactly this reason.

### C1c. Where the human sits
Judge between two adversaries, at the leaves only: the human evaluates the short final exchange, not the whole question. A learned model of the judge can stand in for most debates (§2.3): "Most debates can be judged by the reward predictor rather than by the humans themselves."

---

## C2. Scale invariance

### C2a. The repeated step
**Argue and refute, then recurse into the disputed claim.** Each round, the debaters narrow the disagreement to one sub-claim, and the debate descends one level of the argument tree (§2.1). The same step is used at every depth and at every capability level, which is what makes the PSPACE result possible.

### C2b. What must stay invariant: **~**
*Load-bearing assumption:* the human's verdict on a single short leaf statement is a fixed function that does not drift with the topic, framing or strangeness of the question above it.

The method needs much less of the human than amplification does: not a function for choosing sub-solutions, only the ability to say which side of one narrow, concrete dispute is right. That is close to the "this step is acceptable and not deceptive" basin from Discussion 1. But the authors list ways it may not be stable: bias toward prior beliefs (§5.1), variance between judges (§5.1: "A majority vote of several randomly chosen judges may be better than one... People may differ in their judging ability"), and the hidden-flaw worry (§5.2). *Our read:* the invariant is plausible for local, factual leaves and less so for leaves about values or unfamiliar concepts.

### C2c. Error behavior across levels: **~**
*Regime (criterion 1.1):* The correcting step is refutation at every level. Under optimal play the map is exact and no error enters (Theorem 1); for approximate play there is no map. Best case **thresholded**, with an unknown threshold. **~** stands.
*Load-bearing assumption:* approximately optimal play behaves like optimal play.

For optimal play errors do not compound at all: Theorem 1 is exact at every depth. For real, imperfect debaters the paper offers no bound; §5.5 says approximate optimal play "can be adapted by introducing randomness so that approximate optimal play can be defined and analyzed," but this is future work. Debate length is also a constraint (§5.7): "For debate to work the number of statements per debate must be reasonably low, and by the final statement future argument should be unconvincing."

### C2d. Phase transitions: **~**
*Load-bearing assumption:* no debater can hide a flaw the other cannot find, and no short statement can hack the judge.

The authors name two thresholds. First, a flaw that the honest debater misses (§5.2): "Will one of the agents be able to sneak in a statement that appears logical but contains a hidden flaw without the other agent pointing out the flaw?" This is the seed of the obfuscated arguments problem (see [Barnes 2020](barnes-2020-obfuscated-arguments.md)), and the theorem hides it by assuming optimal, unbounded debaters. Second, manipulation of the judge (§5.8): "A sufficiently strong misaligned AI may be able to convince a human to do dangerous things via a text channel... However, we believe it is unlikely that a single short sentence is sufficient for this sort of mind hack." *Our read:* both are capability thresholds where the argument changes kind; the paper flags them but does not resolve them.

### C2e. Exact or statistical
**Exact** in the idealized model (a theorem for all depths, given optimal play and a polynomial-time judge). **Statistical**, and barely so, for anything involving people or trained agents: one toy domain.

### C2f. Measurability
The MNIST experiment varies one knob (§3.1): "With 6 pixels, debate with precommit turns a 59.4% accurate judge into a 88.9% accurate debate... With 4 pixels, a 48.2% accurate judge becomes an 85.2% accurate debate." That is two points on a judge-weakness axis, with debaters of fixed strength. It does not vary the capability gap between debaters and judge. The design is extendable (vary pixels, debater size, judge size), so the claim is testable in principle. The authors state the evidence base plainly (§8): "we have demonstrated only a basic experiment for MNIST images. Significant research will be required to know whether debate works in practice with human judges."

**C2 verdict: ~.** The step is genuinely self-similar and the idealized argument is exact, but the parts that apply to real humans (C2b, C2d) are hopes, and the evidence is one toy domain.

---

## C3. Competitiveness

### C3a. Training competitiveness: **~**
*Load-bearing assumption:* a learned model of the judge can replace most human judgments without being exploited.

*Steelman.* Debate is self-play, which the authors count as an advantage (§4.3): "Debate is a competitive game between agents, and inherits the advantages of multiagent competition," including a natural curriculum. Human cost is cut by reward modeling (§2.3, quoted in C1c). *Critique.* §5.4: "Debate could also be uncompetitive with other ML approaches because debate requires human input," since a simulated environment "may be much faster for generating samples than asking humans questions even if it is unsafe."

### C3b. Performance competitiveness: **~**
*Load-bearing assumption:* the honest equilibrium is as capable as an unrestricted system.

*Steelman.* The authors hope (§2) that "At Nash equilibria, debate agents are approximately as strong as unrestricted AI (agents trained with no safety measures)." At test time the debate need not be run (§2.1): "At test time it suffices to stop after step 2." *Critique.* The authors also consider the opposite (§5.4): "aligned AI systems using debate will be weaker than AI systems trained in other ways, and debate is less likely to be used." Acting in the world needs "one debate per action" during training (§2.3). The proposal is "only for the natural language case" (§8).

### C3c. Alignment tax trend: **?**
The paper does not say how cost scales with capability. *Our read:* if the reward model of the judge holds up, human cost per debate stays roughly flat, since the human only sees the leaves; debate length grows with argument depth, which is polynomial in the theory. Whether the judge model stays unexploited as debaters get stronger is the open part.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| Debate (Irving 2018) | ~ | ✗ | ~ | ~ | ~ |

---

## Open questions for the comparison

1. **Debate vs. amplification as two definitions of PSPACE.** §7 says "Debate is closely related to Christiano et al. [2018]'s amplification model... DEBATE: polynomial length adversarial games. Amplification: polynomial depth recursion." Do they share one invariant (C2b), or does debate need less of the human because it only checks one path?
2. **Is a judge's leaf verdict the stable basin?** Debate asks only "which side of this narrow claim is right." Is that the "acceptable and not deceptive" basin from Discussion 1, or does it drift at higher levels like the sub-solution function in amplification?
3. **Where does the theorem's "optimal play" assumption break?** Barnes (2020) argues it breaks once debaters are bounded. Every C2 verdict here should be re-read in light of that.
4. **Inner alignment.** Can an adversary that is a copy of the same model catch deceptive alignment, or does the method need Hubinger's transparency add-on to earn better than ✗?
5. **Scope.** The paper itself says debate "does not address other safety concerns such as robustness to adversarial examples, distributional shift, or safe exploration" (§8).
