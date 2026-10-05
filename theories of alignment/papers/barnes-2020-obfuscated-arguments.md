# Debate update: Obfuscated arguments problem (Barnes et al., 2020)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation.** Beth Barnes (with Paul Christiano). *Debate update: Obfuscated arguments problem.* Alignment Forum, 23 December 2020. <https://www.alignmentforum.org/posts/PJLABqQ962hZEqhdB/debate-update-obfuscated-arguments-problem>
**Role.** Evaluation of [debate](irving-2018-debate.md). Following our brief, we still apply the criterion, to debate *as updated by this post*.
**Sources read.** The Alignment Forum post, read through WebFetch with targeted prompts for verbatim passages and their headings. The fetched byline showed Beth Barnes; the post writes as "we", and Christiano's co-authorship is from our brief, not checked. The post has no numbered sections, so quotes are located by heading. Nothing could not be opened.
**Criterion version.** 1.0.

---

## The method in one paragraph

The method under test is debate, with structure added from earlier human experiments: explicit, structured arguments and cross-examination ("Previous problems and solutions": "Whenever a statement is made, save a copy of the debater who made that statement. Later in the debate, allow the opponent to ask clarifying questions to that copy of the debater"). The experiments used human debaters and judges. The post's one-sentence finding ("What we did"):

> "We discovered that a dishonest debater can often create arguments that have a fatal error, but where it is very hard to locate the error."

The goal it measures debate against ("Key takeaways and relevance for alignment"): "if the model can efficiently find the correct answer to some problem, our mechanism should favor the correct answer while only requiring a tractable number of human judgements and a reasonable number of computation steps for the model."

---

## The finding

An obfuscated argument is one where ("Basic case for the obfuscated argument problem"):

> "(1) The argument is made invalid by the inclusion of a small number of flawed steps (2) The argument is sufficiently large that we are unlikely to find a flaw by traversing the argument in any naive way (3) Neither debater knows where the flaws are, so the honest debater's best response to the argument is to state that there's a small chance of any given step being flawed"

The cleanest example ("Cryptographic example") asks "Does RSA-2048 have any prime factors?" The dishonest debater splits the claim so that one half must be false, and the honest debater can only say "I'm not sure which of (1) or (2) is wrong, but I know at least one of them must be since my argument is good!" The post adds: "If Honest reliably knew which side to object to, then they would be able to factor RSA-2048." The two sides then look almost the same to the judge: "In the honest argument, the claim will always be correct. In the dishonest argument, the claim will almost always be correct, but in very rare cases the claim will be wrong." The post also gives a recipe for building such arguments by choosing sub-claims that are "about 50% likely to be true in the case where your answer is wrong" ("General method of constructing these arguments").

Status of a fix ("What we did"): "We don't have a fix for this 'obfuscated argument' problem."

---

## C1. Alignment

### C1a. Outer alignment: **~** (narrowed)
*Load-bearing assumption:* the question has an argument whose flawed step, if any, the honest debater can locate within its own compute budget.

*Steelman.* The post does not say debate's objective is wrong. It says the objective rewards truth only where the honest side can do the work of finding the flaw, and it expects that region to be large ("Key takeaways"): "it seems plausible that a substantial proportion of the things a model 'knows' will have some long but locally human-understandable argument for their correctness."

*Critique.* Outside that region, winning and truth come apart: an obfuscated false argument and an honest one are "almost symmetrical" to the judge. The original guarantee shrinks ("Original debate hope"): "if the honest debater is bounded, then we can essentially only trust conclusions that can be verified using the debater's computational resources. In the complexity-theoretic analogy, if the debaters are in P then we can only trust conclusions in MA." *Our read:* still ~, but on a smaller set of questions than Irving et al. claimed. The PSPACE result needed optimal debaters; this is where that assumption bites.

### C1b. Inner alignment: **?**
*Load-bearing assumption:* none stated; the post does not discuss training dynamics or mesa-objectives.

The post frames alignment as elicitation ("Relevance for alignment", appendix): "We claim that a key step for alignment is to ensure that our models 'honestly tell us everything they know.'" That is the target an inner-aligned debater would hit, not a mechanism for producing one. *Our read:* the post is silent, so the mark is ?. The [Irving summary](irving-2018-debate.md) gives ✗ for the same method, and nothing here changes that.

### C1c. Where the human sits
Judge, as in Irving et al. In the experiments humans were also the debaters. The finding is about the debaters' limits more than the judge's: the judge is never shown the flawed step because the honest debater cannot find it either.

---

## C2. Scale invariance

### C2a. The repeated step
Unchanged: **argue and refute, recursing into one disputed sub-claim**, now with cross-examination and explicit structure.

### C2b. What must stay invariant: **✗**
*Load-bearing assumption:* the honest debater can always point to the flawed step.

Irving et al. needed the judge's leaf verdict to be stable. This post shows a second invariant was also needed and fails: the honest debater's ability to choose which branch to challenge. In the RSA example that choice is as hard as factoring. *Our read:* this is the most important result for Evan's question. Even if the human's "this step is acceptable and not deceptive" function were perfectly invariant, debate would still fail here, because no step the human sees is wrong. The flaw lives in which steps get checked, and that choice belongs to a bounded model, not to the human.

### C2c. Error behavior across levels: **✗**
*Load-bearing assumption:* small per-step error rates stay small when the argument grows.

The construction works by spreading a small chance of error over many steps, so that the argument is false overall while each step is "probably true". The honest debater's best answer is itself probabilistic. *Our read:* this is error compounding with a sign flip. Per-step errors stay small, but the whole argument's error does not, and recursion cannot localize it. The post does not give a bound.

### C2d. Phase transitions: **✗**
*Load-bearing assumption:* the questions we care about stay within what the honest debater can verify.

The threshold is named: once the argument is longer than the honest debater can search, or once finding the flaw is as hard as a hard problem (factoring), the guarantee changes kind from PSPACE to MA ("Original debate hope"). The post's own framing is quantitative ("What we did"): "We believe it might be an important quantitative limitation for both IDA and Debate." A footnote ties it to factored cognition: "this is roughly equivalent to the claim that the answer could be computed by a large HCH tree." *Our read:* "quantitative" is the authors' hedge and should be carried up. Where the boundary sits relative to useful superhuman questions is unknown.

### C2e. Exact or statistical
The negative result is **exact** in the cryptographic example (it reduces to factoring) and **anecdotal** in the human experiments. The positive hope that most knowledge has "long but locally human-understandable" arguments is a conjecture.

### C2f. Measurability
*Our read:* testable in principle by varying the honest debater's budget against argument length and checking where judge accuracy drops. The post reports human experiments qualitatively and gives no curve.

**C2 verdict: ✗.** A known failure mode (C2b to C2d) applies, and "We don't have a fix."

---

## C3. Competitiveness

### C3a. Training competitiveness: **~**
*Load-bearing assumption:* cross-examination and structured formats do not add much cost per debate.

The post does not discuss training cost. Its one remark on cost cuts the other way ("What we did"): "If we have sufficient compute budget to check that many steps, then instead of doing some complicated debate mechanism we can just have both debaters write out their entire proof, and run a checker over them." *Our read:* the fix that always works is full verification, whose cost grows with argument length.

### C3b. Performance competitiveness: **~**
*Load-bearing assumption:* most of what we need from advanced AI has locally checkable arguments.

Trusted outputs are limited to what the bounded debater could verify. *Our read:* this rules out exactly the answers that rely on intuition or learned heuristics with no short argument, which may be where superhuman models are most useful.

### C3c. Alignment tax trend: **grows**
*Our read:* as questions get harder, either we accept the MA limit (a growing performance tax) or we pay to check every step (a growing compute tax). The post does not state this; it follows from its MA and full-checker remarks.

---

## Verdict line

| Method | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| Debate (after obfuscated arguments) | ~ | ? | ✗ | ~ | ~ |

---

## Open questions for the comparison

1. **Does the problem hit amplification and RRM equally?** The post says "both IDA and Debate." Our RRM and amplification summaries should check whether their decomposition step has the same "who picks which branch to check" weakness.
2. **Two invariants, not one.** Scale invariance for recursive methods may need both a stable human judgment (C2b) and a stable ability of the *model* to localize disagreement. The criterion currently asks only about the first.
3. **Where is the MA boundary?** Which real tasks (code review, proofs, research directions) have "long but locally human-understandable" arguments, and which don't?
4. **Later responses.** Follow-up debate protocols (for example, ones aimed at making the honest debater's cost scale with the dishonest one's) are outside this post. They would be the first thing to read before upgrading C2.
