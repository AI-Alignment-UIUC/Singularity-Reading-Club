# When AI Builds Itself (Anthropic Institute, 2026)

[← criterion](../criterion.md) · [← comparison](../README.md)

**Citation:** Marina Favaro and Jack Clark, Anthropic Institute. *When AI builds itself.* 2026, with an update dated 9/18/2026. https://www.anthropic.com/institute/recursive-self-improvement

**Role:** motivation (an essay with internal evidence and scenarios; it proposes no alignment method).

**Sources read:** the web page itself, through a text extraction, with three targeted passes. Section headings in the page include "Evidence from the outside world", "Evidence from within Anthropic", "What might the future of work at Anthropic look like?", "What if we're wrong?", "Possible futures" and "What should we do?". Quotes below are cited by the heading they fall under; check the live page before putting one on a slide. Nothing was taken from secondary sources.

---

## What the paper contributes

The essay reports that AI is already doing much of the work of building its successor at Anthropic, and asks what follows if that trend continues. Its central claim:

> "For most of AI's history, humans drove every step in its development cycle. But at Anthropic, we are delegating a growing share of AI development to AI systems themselves, which is speeding up our work." (opening section)

> "Taken far enough, and given enough compute, that trend points to an AI system capable of fully autonomously designing and developing its own successor. This is called *recursive self-improvement*." (opening section)

For this folder it is the motivating case. Every method we compare is a recursion on paper: a model at level k helps oversee level k+1. Recursive self-improvement is that recursion in practice, with the model doing the building as well as the checking. The essay hedges the timeline: "We are not there yet, and recursive self-improvement is not inevitable. But it could come sooner than most institutions are prepared for." (opening section)

---

## C1. Alignment: what the paper contributes

**C1c (where the human sits).** The essay describes the human's role moving from author to reviewer to overseer of reviewers:

> "Once human- and AI-authored code quality reach parity, humans will stop writing code entirely, and shift to only reviewing it." ("What might the future of work at Anthropic look like?")

> "Humans play a substantially diminished role in their development, likely moving most of our effort towards oversight, validation, and verification of an expanding 'virtual lab' run by AI systems." ("Possible futures", third scenario)

*Our read:* this is the C1c trajectory that scalable oversight methods assume: the human ends up as an evaluator or judge of work they did not produce. The essay gives a real-world reason to grade every method by how it behaves once the human is *only* an evaluator.

**C1a / C1b.** The essay does not distinguish outer from inner misalignment. It says the authors are least certain here:

> "How the alignment problem gets solved—or not—in this future is something we are least certain about. Models could prove to be sufficiently aligned and capable enough of research taste that they discover and implement novel solutions that we have not yet reached." ("Possible futures", third scenario)

*Our read:* the optimistic branch is "the models solve alignment for their successors." That is the hope behind automated alignment research, and it rests on the same C2b assumption as amplification: that whatever judgment picks good solutions survives each handoff.

---

## C2. Scale invariance: what the paper contributes

**C2c (compounding error).** This is the essay's most direct contribution to the criterion:

> "Alternatively, the rare occurrences of misalignment present in today's models could compound as the models build their successors, growing more frequent but less understood until we lose control of them." ("Possible futures", third scenario)

*Our read:* this is the real-world version of the open question Leike et al. (2018, §3.2) raise for RRM, "whether errors accumulate." Note the two clauses. "More frequent" is error growth, which C2c already asks about. "Less understood" is something else: our ability to *detect* the error degrades along with the error rate. C2c as written does not separate these.

**C2d (thresholds the essay names).** The essay identifies a remaining gap in judgment as the line between today and recursive self-improvement:

> "Large performance gaps persist when it comes to Claude exercising judgement in choosing goals in both engineering and research. That's the gap between AI today and a future system that could autonomously design its own successor." ("Evidence from within Anthropic")

It also reports measurements tracking that gap: "In November 2025 (Opus 4.5) beat the human choice 51% of the time; in April 2026 (Mythos Preview), this grew to 64%." (research-judgment results). On experiment speedups: "In May 2025, [Claude Opus 4] averaged a ~3x speedup over the starting code. By April 2026, [Claude Mythos Preview] was achieving ~52x." (bracketed model names as returned by the extraction; *unverified* whether the brackets are in the original).

*Our read:* the essay names two candidate transitions: code-quality parity (humans stop writing, only review) and goal-choosing judgment (the model sets its own research direction). The second is a change of kind in the C2d sense, since the overseer then evaluates goals, not just outputs. Neither is framed by the authors as a sharp threshold; the numbers are trends.

**C2b and the bottleneck.** The essay frames human review as the binding constraint:

> "Anthropic has already encountered one signature of Amdahl's law: as we've begun to push more code around the organization, human code review has become a new bottleneck." ("What might the future of work at Anthropic look like?")

*Our read:* Amdahl's law is a scaling law for the human step. If the AI part speeds up and human review does not, review either becomes the whole cost or gets thinned out. Both outcomes bear on C2b: the invariant human judgment is either a throughput limit or is sampled ever more sparsely, which is the 2016 sparse-label problem (Amodei et al., §5) returning at a larger scale.

**C2f (measurability).** The essay offers several time series (share of code, speedups, judgment win rates). *Our read:* these are capability trends, not oversight-quality trends at several capability gaps, so they do not yet give the slope C2f asks for. The one oversight measurement is the automated-review retrospective below, at a single point in time.

---

## C3. Competitiveness: what the paper contributes

**C3a / C3c (the tax and the pressure).** AI-assisted review is the essay's concrete oversight tool:

> "Using this tool, we ran a retrospective analysis, and found that an automated Claude review of every change to our codebase would have caught roughly a third of the bugs behind past incidents" ("Evidence from within Anthropic"; the extraction returned two endings for this sentence, so we stop at "incidents").

On competitive pressure, which sets how much tax labs will pay:

> "Without a global coordination mechanism, companies and governments will have to make difficult decisions about safety while under competitive and geopolitical pressures." ("What should we do?")

The essay's policy response is the option to slow down: "We believe it would be good for the world to have the *option* to slow or temporarily pause frontier AI development to enable societal structures and alignment research to keep up with the advance of the technology." ("What should we do?")

*Our read:* this is the clearest real case for C3c. Human review cost grows with the volume of AI output ("8× as much code per day" per engineer, per the essay), so the tax of human oversight rises exactly as capability rises. A method whose tax grows this way is the one the criterion warns "stops being used just when it matters." The essay's answer is coordination, not a cheaper method, which says something about how hard it thinks C3c is.

---

## Verdict line

| Paper | C1a | C1b | C2 | C3a | C3b |
|---|---|---|---|---|---|
| [Anthropic 2026](anthropic-2026-when-ai-builds-itself.md) | n/a | n/a | n/a | n/a | n/a |

**Feeds:** C1c (human moves from author to reviewer to overseer of a "virtual lab"), C2b (human review as Amdahl bottleneck), C2c (misalignment compounding and becoming "less understood"), C2d (goal-choosing judgment as the remaining gap; code-quality parity), C2f (capability trends, not oversight trends), C3a and C3c (review cost scales with AI output; competitive pressure on the tax).

---

## Open questions for the comparison

1. Which methods in the grid would still work if the human only reviews, at a sampling rate that falls as AI output grows?
2. Should C2c separate "errors grow" from "errors become harder to detect"? The essay's "more frequent but less understood" suggests two failure curves, not one.
3. Is the automated review result (about a third of incident bugs caught) evidence for or against recursive oversight? It helps, but it means two thirds were missed, and the reviewer and the author are the same model family.
4. Does the shift from evaluating outputs to evaluating *goals* count as a phase transition for methods built on output evaluation, such as RRM and debate?
5. If the safety plan at the top level is coordination (a pause option), should C3 have a sub-criterion about whether a method makes slowing down cheaper or easier to verify?
