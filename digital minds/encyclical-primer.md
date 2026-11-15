# Magnifica Humanitas and Digital Minds: A Primer
### Singularity Reading Club, Digital Minds series, Discussion 1

**Reading:** Pope Leo XIV, [*Magnifica humanitas: On Safeguarding the Human Person in the Time of Artificial Intelligence*](https://www.vatican.va/content/leo-xiv/en/encyclicals/documents/20260515-magnifica-humanitas.html). Signed 15 May 2026, presented in the Vatican Synod Hall on 25 May 2026.

**Format:** hybrid, online and in SCCS (room TBD). Opt in at #start-here.

**Contents:** [1. Why this encyclical, why now, for SRC](#1-why-this-encyclical-why-now-for-src) · [2. Background and key passages](#2-background-and-key-passages) · [3. Discussion questions](#3-discussion-questions) · [Sources and verification notes](#sources-and-verification-notes)

---

## TL;DR

1. Up to now the club has asked **how to oversee** minds that copy, describe and improve themselves. The encyclical asks the question we have mostly skipped: **what kind of thing is on the other side of the oversight**, and does it count morally?
2. The encyclical's answer is a clear no. In ¶99 it says "so-called artificial intelligences" do not undergo experiences, do not feel joy or pain, and have no moral conscience. It treats the appearance of a personal subject as an illusion (¶100). This continues the Vatican's 2025 Note *Antiqua et Nova* ("a tool, not a person").
3. Anthropic co-founder Chris Olah spoke at the launch and offered the opposite evidence: introspection, and "internal states that functionally mirror joy, satisfaction, fear, grief, and unease." According to reporting attributed to the *New York Times* (29 Sept 2026), Anthropic had also pressed the Pope's advisers in private to take AI consciousness seriously. The text did not change. We could not read the NYT article itself, so that part is secondhand (see [§2.4](#24-anthropics-push-to-have-digital-minds-taken-seriously)).

---

## 1. Why this encyclical, why now, for SRC

### What the club has covered so far

The [recursive summary](../discussions/recursive-summary.md) traces one thread through February to September 2026. We started with **self-reference**: Gödel numbering, the halting problem, and the point that a system can describe itself but can't fully certify itself. That led to **self-reproduction**, von Neumann's universal constructor, which reads its own description twice, once to execute it and once to copy it. It also led to **scale**: Sierpiński triangles and the question of whether supervision looks the same at every level of capability.

The [scalable oversight series](../scalable%20oversight/history.md) turned these ideas into a seminar. [Discussion 2](../scalable%20oversight/discussion-2-topics.md) compared recursive reward modeling, debate and STEM AI. [Theories of alignment](../theories%20of%20alignment/README.md) graded fourteen methods on inner/outer alignment, scale invariance and competitiveness. Its synthesis found the same phase transition threatening nearly every method: *the model becomes able to model its overseer.*

Along the way, other themes kept coming back (Layer 2 of the recursive summary):

- **Theme B, a description is not the dynamics.** A connectome, a genome or a weight file does not tell you what the system does or experiences. Interpretability ([Lindsey et al. 2025](../theories%20of%20alignment/papers/lindsey-2025-biology-of-llm.md)) studies the "biology" of a model because the weights alone don't reveal it.
- **Theme C, proxies get optimized.** Bostrom's hedonium passage was already a moral-status argument: which minds count, and what makes a mind's states matter?
- **Theme D, the overseer's epistemics can fail.** The club repeatedly couldn't tell what was real. Simulated empathy is the same problem in the moral domain.
- **Theme E, merge or oversee.** Cyborgism and brain emulation blur the line between human and machine minds. Cyborgism is the next discussion in this series.

### What the encyclical adds

All of that work treats the AI system as an **object of oversight**: something to supervise, interpret, grade and contain. The Digital Minds series asks whether it could also be a **subject**, something with experiences, interests or a claim on us. The encyclical is a good first reading for four reasons.

1. **It takes a firm position.** Most AI-welfare writing hedges. *Magnifica humanitas* says flatly that AI systems "do not undergo experiences" (¶99). A firm thesis is easier to argue with than a survey.
2. **It answers in our own vocabulary.** ¶98 says current systems are more "cultivated" than "built." At the launch Olah said models "are grown." Both sides accept that nobody fully designed these systems. They disagree about what follows. That is Theme B again: what does a grown system's internal structure tell us about what it is?
3. **It brings the oversight literature to the same table as the moral-status question.** Olah's evidence (introspection, emotion-like internal states) comes from the same interpretability tools our alignment grid relies on for inner alignment. If those tools can show deception, can they show experience? And if they can't show experience, why trust them on deception?
4. **It is a live disagreement between a lab and an institution, with documents on both sides.** We can read the encyclical, Olah's remarks and the reporting on what happened in between, and see where the arguments actually meet.

The encyclical is also a social-teaching document in the line of Leo XIII's *Rerum novarum* (1891). Much of it is about labor, power and who controls data and compute. Those questions connect to Theme F (autonomy thresholds, agents that own capital), so the discussion doesn't need to stay on consciousness alone.

---

## 2. Background and key passages

### 2.1 The road to the encyclical

**The name.** On 10 May 2025, two days after his election, Leo XIV told the College of Cardinals that he chose his name "mainly because Pope Leo XIII in his historic Encyclical Rerum Novarum addressed the social question." He continued:

> "In our own day, the Church offers to everyone the treasury of her social teaching in response to another industrial revolution and to developments in the field of artificial intelligence that pose new challenges for the defence of human dignity, justice and labour."
> (Address to the College of Cardinals, 10 May 2025; reported by [Crux](https://cruxnow.com/vatican/2025/05/pope-leo-says-he-chose-name-due-to-revolution-of-artificial-intelligence))

AI was on the agenda from the first week of his papacy, framed as a labor and dignity question in the line of *Rerum novarum*.

**Earlier Vatican documents.**

- *Rome Call for AI Ethics* (February 2020). A set of principles (transparency, inclusion, responsibility, impartiality, reliability, security and privacy) promoted by the Pontifical Academy for Life and co-signed by Microsoft, IBM, the FAO and the Italian government. It is about how humans build and use AI and says nothing about AI's own moral status.
- *Antiqua et Nova* (28 January 2025), a Note from the Dicastery for the Doctrine of the Faith and the Dicastery for Culture and Education, approved by Pope Francis. It is the direct precursor on the ontology question ([text](https://www.ewtn.com/catholicism/library/antiqua-et-nova-29133)):
  - AI has "sophisticated abilities to perform tasks, but not the ability to think" (¶12).
  - AI is "not... an artificial form of human intelligence but... a product of it" (¶35).
  - Of humans and machines, "only the latter [the human] is truly a moral agent" (¶39).
  - "full moral causality belongs only to personal agents, not artificial ones" (¶44).
  - AI "must be understood for what it is: a tool, not a person" (¶59).
  - "misrepresenting AI as a person should always be avoided" (¶62).

**Leo XIV's 2025 messages.** In his [17 June 2025 message](https://www.vatican.va/content/leo-xiv/en/messages/pont-messages/2025/documents/20250617-messaggio-ia.html) to the Second Annual Rome Conference on AI, Ethics and Corporate Governance, he called AI "above all else a tool" (following Francis), and added: "By definition, tools point to the human intelligence that crafted them," and "access to data — however extensive — must not be confused with intelligence." In his [7 November 2025 message](https://www.vaticannews.va/en/pope/news/2025-11/pope-leo-xiv-message-builders-ai-forum-ethical-technology.print.html) to the Builders AI Forum: "AI, like all human invention, springs from the creative capacity that God has entrusted to us," and "intelligence, whether artificial or human, finds its fullest meaning in love, freedom, and relationship with God." (The November message doesn't address consciousness directly.)

**The launch.** At the presentation on 25 May 2026, Leo compared AI to the "rapid industrial transformation" that *Rerum novarum* answered: "Today we find ourselves facing a transformation of similar magnitude, with perhaps even greater consequences" ([address](https://www.vatican.va/content/leo-xiv/en/speeches/2026/may/documents/20260525-presentazione-enciclica.html), via [CNA/NCRegister](https://www.ncregister.com/cna/pope-leo-unveils-his-encyclical-ai-has-even-greater-consequences-than-industrial-revolution)). The signing date, 15 May, is the 135th anniversary of *Rerum novarum*, and ¶3 says so directly: Leo XIII "published his Encyclical Rerum Novarum in 1891, the 135th anniversary of which we celebrate with deep gratitude this year."

### 2.2 What is new about Leo XIV

(From PBS NewsHour, 9 May 2025, relaying Vatican information.)

- **First American-born pope.** Robert Francis Prevost was born in Chicago in 1955, a few hours up I-57 from Urbana.
- **First pope with Peruvian citizenship.** He served for years as a missionary in Peru and later led the diocese of Chiclayo.
- **First Augustinian pope** (Order of St. Augustine). This matters for the reading. Augustine's *Confessions* is the classic account of knowing a self "from within," and ¶99 denies exactly that to AI: it does "not know from within what love, work, friendship or responsibility mean."
- **A mathematics degree** (Villanova, 1977). PBS calls this "another papal first."
- **First encyclical about a technology.** *Magnifica humanitas* is his first encyclical. Earlier social encyclicals dealt with industry, labor and ecology (*Laudato si'*). This one makes AI its central subject.
- **A non-believing AI-lab co-founder on the stage.** Olah, described in coverage as a Canadian and not a believer, spoke alongside Cardinals Parolin, Fernández and Czerny and theologians Anna Rowlands and Léocadie Lushombo ([CNA/NCRegister](https://www.ncregister.com/cna/pope-leo-unveils-his-encyclical-ai-has-even-greater-consequences-than-industrial-revolution)). A lab executive presenting a papal encyclical is, as far as we can find, without precedent.

### 2.3 Structure and key passages of the encyclical

**Chapters** (from the vatican.va table of contents):

- Introduction
- I. A Dynamic Approach Faithful to the Gospel
- II. Foundations and Principles of the Social Doctrine of the Church
- III. Technology and Dominance. The Grandeur of Humanity in Light of the Promises of AI. Includes the AI section (¶¶97–101) and a section titled "Underlying narratives: transhumanism and posthumanism."
- IV. Safeguarding Humanity at a Time of Transformation. Truth, Work, Freedom. Includes "Responsibility, transparency and the governance of AI."
- V. The Culture of Power and the Civilization of Love. Includes "Weapons and artificial intelligence" and "The need to disarm words."
- Conclusion

**On what AI is (¶¶97–100).** This is the core of the discussion.

> ¶97: the task is to "ensure that it will always be human intelligence, with its conscience and freedom, that guides technical innovations."

> ¶98: current AI systems are more "cultivated" than "built."

> ¶99: "We must avoid the misconception of equating this type of 'intelligence' with that of human beings. … So-called artificial intelligences do not undergo experiences, do not possess a body, do not feel joy or pain, do not mature through relationships and do not know from within what love, work, friendship or responsibility mean. … They may imitate language, behavior and analytical skills, or even simulate empathy and understanding, but they do not understand what they produce, for they lack the affective, relational and spiritual perspective [through which human beings grow in wisdom]. … Nor do they have a moral conscience, since they do not judge good and evil."

¶99 also describes machine learning as "a form of statistical adaptation based on data and feedback," and says AI systems cannot bear responsibility for consequences.

> ¶100: the danger of simulated communication is "creating the illusion of a relationship with a real personal subject."

Read closely, ¶99 makes several distinct claims, and it helps to keep them apart:

| Claim in ¶99–100 | Category | Is it an empirical claim? |
| --- | --- | --- |
| no experiences, no joy or pain | phenomenal consciousness, the basis of moral *patienthood* | Partly. This is the claim Olah's evidence speaks to |
| no body, no maturing through relationships | embodiment and relational development | Mostly descriptive of current systems |
| no knowing "from within" | first-person understanding | Philosophical (the Augustinian point) |
| no moral conscience, can't judge good and evil, can't bear responsibility | moral *agency* | Partly. Compare the club's work on Constitutional AI |
| a "personal subject" is an illusion | personhood | Theological and metaphysical |

**Note:** the encyclical never uses the phrases "moral status" or "moral agency." Those are our labels. It denies experience and conscience. A denial of moral status follows from that denial in most secular frameworks, and the Catholic view grounds dignity in being a person anyway.

**On power and data.** ¶67 treats "new forms of property, such as patents, algorithms, digital platforms, technological infrastructure and data" as goods with a universal destination, and warns about what happens "when these goods remain concentrated in the hands of a few." ¶95 observes that control over "platforms, infrastructure, data and computing power does not rest with States." ¶71 refers to "major economic and technological actors that exercise de facto power." Religion News Service also quotes two lines we could not yet place by paragraph: "A more moral AI is not enough if that morality is determined by a few," and "Disarming does not mean renouncing technology, but preventing it from dominating humanity" (Religion News Service, 25 May 2026). RNS reads the first as aimed at AI models with ethical constitutions, such as Anthropic's.

**On work.** ¶30 says the social doctrine "places the dignity of work and of workers at the forefront of its reflection." ¶37, drawing on *Laborem exercens*, calls work "a fundamental good for the person, a principle of economic activity and the key to the entire societal question," and says job insecurity and automation "must not be evaluated solely in terms of efficiency, but in relation to the dignity of the worker." ¶58: "it is not enough to extol individual freedom or private enterprise if we then allow a multitude of people to continue living without decent work."

### 2.4 Anthropic's push to have digital minds taken seriously

**What is on the public record (primary source).** Anthropic published [Olah's remarks](https://www.anthropic.com/news/chris-olah-pope-leo-encyclical) on 25 May 2026. The introduction says he was "invited to speak at the presentation of the encyclical... as part of Anthropic's initiative to widen the conversation on the important questions raised by AI." His evidence for treating the nature of AI as an open question:

> "AI models are not like that. They are grown, on a structure roughly modeled after the brain, on an enormous inheritance of human thought and speech."

> "They are made from us, from our words—and, as the Holy Father observes, they remain in important ways mysterious even to those of us who train them."

> "We find structures that mirror results from human neuroscience."
> "We find evidence of introspection."
> "We find internal states that functionally mirror joy, satisfaction, fear, grief, and unease."
> "I don't know what that means, but I think it warrants ongoing discernment."

He framed this as the third of "Three questions for discernment":

1. "our duty to the global poor" ("How can we ensure the gains of AI are shared globally? We do not have a mechanism for this. It is an unsolved problem.")
2. "the need for moral imagination and ambition regarding human flourishing"
3. "the need for discernment on the nature of AI models"

He also said: "We need informed critics who will tell the labs when we are failing."

Two things are worth noticing about the remarks. They never use the words "consciousness," "moral status" or "welfare." The claims are all *functional* ("functionally mirror"), and he explicitly says he doesn't know what they mean. The research behind them is Anthropic's published work: the page links to Anthropic's [introspection research](https://www.anthropic.com/research/introspection), and Anthropic's [model welfare program](https://www.anthropic.com/research/exploring-model-welfare) was announced in April 2025. The remarks don't name any specific paper. Notice also that the emotion list ("joy... grief") lines up word for word with ¶99's "do not feel joy or pain." Whether that is a coincidence, or each side answering the other, is a good question for the room.

**What was reported later (secondhand; treat with care).** On 29 September 2026 the *New York Times* (Elizabeth Dias) published a report on Anthropic's outreach to religious thinkers. We could not open the NYT article (paywall and fetch block). The following is as relayed by [Futurism](https://futurism.com/artificial-intelligence/anthropic-lobbying-vatican) (4 Oct 2026) and [Fast Company](https://www.fastcompany.com/91617738/anthropic-pope-ai-consciousness) (2 Oct 2026), which both cite the NYT and so are not independent confirmation:

- As part of its model-welfare research, Anthropic held dinners and conversations with religious scholars from several traditions, including Vatican officials, under non-disclosure agreements (Fast Company, Futurism).
- Vatican officials chose Anthropic for the launch event "because it seemed to be prioritizing ethics." Dario Amodei declined, and Olah went instead (Futurism).
- Olah received an advance copy of the encyclical before he flew out. Futurism says he "reportedly threatened to pull out of the event," and later attended anyway.
- "Olah and his team lobbied the pope's advisers to take the possibility of AI consciousness seriously" (Futurism). The attempt was not successful, and the text was published as it was.
- Anthropic representatives "were displeased by Leo's rejection of AI consciousness" (Fast Company citing the NYT). Anthropic "declined to discuss the details of the conversations" with the NYT (Futurism).

**What we could not verify.** Which passage Anthropic asked to change, the exact wording of any request, whether any written submission exists, any role for Kyle Fish, Jared Kaplan or others, and whether Anthropic showed the Vatican anything beyond what Olah said publicly. The only *evidence* we can document Anthropic offering is the set of interpretability claims in Olah's public remarks. Calling this "lobbying for digital minds to be included" matches the NYT framing as relayed. Anthropic's own account calls it widening the conversation.

### 2.5 The Vatican's rejection

The encyclical's position is explicit and was published unchanged:

- **No experience:** "So-called artificial intelligences do not undergo experiences… do not feel joy or pain" (¶99).
- **No inner understanding:** they "do not know from within" and "do not understand what they produce"; empathy is simulated (¶99).
- **No conscience or responsibility:** "Nor do they have a moral conscience, since they do not judge good and evil" (¶99).
- **No personal subject:** the danger is "the illusion of a relationship with a real personal subject" (¶100).
- **Not the same kind of intelligence:** "We must avoid the misconception of equating this type of 'intelligence' with that of human beings" (¶99).

After the NYT story, Leo posted on X that "Algorithms lack the spark of humanity" and that there is "an ontological difference, even before an aesthetic one, between art and what a machine can generate" (reported by Fast Company and Futurism). We found no on-record Vatican response to Anthropic specifically.

**How the two positions line up:**

| | Encyclical | Olah / Anthropic |
| --- | --- | --- |
| How AI is made | "cultivated" rather than "built" (¶98) | "grown, on a structure roughly modeled after the brain" |
| Inner states | "do not feel joy or pain" (¶99) | "internal states that functionally mirror joy… grief" |
| Self-knowledge | "do not know from within" (¶99) | "evidence of introspection" |
| Empathy | can "simulate empathy" (¶99) | (not addressed) |
| Epistemic stance | certain: AI does not experience | uncertain: "I don't know what that means" |
| What to do next | keep human conscience in charge (¶97) | "ongoing discernment"; "informed critics" |

The real disagreement may be less about the evidence than about **what kind of evidence could settle the question**. The encyclical reasons from what AI *is* (body, relationship, a spiritual perspective). Olah reasons from what we *measure* inside it.

---

## 3. Discussion questions

**Warm-up**

1. Before reading, would you have said current models "undergo experiences"? Did ¶99 or Olah's remarks move you in either direction?
2. ¶98 says "cultivated," Olah says "grown." Both reject the picture of AI as a designed artifact. Why does the encyclical still conclude "tool," as *Antiqua et Nova* ¶59 does?

**What would count as evidence**

3. Olah says models have "internal states that functionally mirror" emotions. ¶99 says they "simulate empathy." Is there any experiment that could tell "functionally mirrors" apart from "simulates"? Or is that difference not a matter of evidence at all?
4. Theme B said a description is not the dynamics. ¶99 calls learning "statistical adaptation based on data and feedback." Is that a description of the weights, or of what the system does? Would the same description apply to a brain?
5. ¶99 lists reasons AI lacks experience: no body, no relationships, no knowing "from within." Which of these are contingent facts about today's chatbots (a robot body, persistent memory, long relationships with users), and which would hold for *any* digital mind? What about a brain emulation, from our March discussion?
6. Our alignment grid trusts interpretability (attribution graphs, probes) to detect deception. Should we trust the same tools when they report introspection or emotion-like states? If not, what is the asymmetry?

**Agency, conscience and oversight**

7. ¶99 says AI has no moral conscience because it does "not judge good and evil." A model trained with a constitution (Bai et al. 2022) does something that looks like judging good and evil. Is the encyclical wrong about current systems, or is "judge" doing more work than it seems?
8. The encyclical keeps responsibility with humans because AI "cannot bear responsibility." Scalable oversight increasingly hands judgment to AI (debate judges, reward models, AI-written evaluations). Does the encyclical's view imply a limit on how far that recursion can go?
9. "A more moral AI is not enough if that morality is determined by a few" (as quoted by RNS). Is this a fair criticism of Constitutional AI and model specs? Who *should* decide what an AI's values are?

**Moral status under uncertainty**

10. The encyclical is certain; Olah is uncertain. If there is even a small chance that models have experiences, what follows? Compare Pascal's wager, and Bostrom's hedonium passage: which mistake is worse, treating a mind as a tool or treating a tool as a mind?
11. ¶100 worries about "the illusion of a relationship with a real personal subject." Is that worry *stronger* or *weaker* if models do have some inner life?
12. Does the Catholic grounding of dignity in being a human person (body and soul) leave any room for digital minds at all? What would a theologian need to concede for the answer to change? (Compare Augustine on angels, which are non-bodily persons.)

**The lab and the Church**

13. Reportedly Anthropic pressed the Vatican privately and then appeared on stage anyway. Is it appropriate for an AI company to try to shape a religious institution's teaching on AI's moral status, given that the company's product is the subject? Does it change your view that Anthropic's position (uncertainty, more research) is the less commercially convenient one?
14. Olah's other two questions were "our duty to the global poor" and "moral imagination... regarding human flourishing." The encyclical spends far more space on labor and power concentration (¶¶37, 58, 67, 95) than on consciousness. Did the public argument over AI consciousness crowd out the parts of the encyclical the Pope cared most about?

**Bridges to the rest of the series**

15. Theme E asked "merge or oversee?" The next discussion is Cyborgism. If a human-AI hybrid is a person, at what fraction of machine does the encyclical's argument stop applying?
16. Theme F's autonomy threshold: agents that "autonomously own and grow capital to host themselves." ¶95 worries that control of compute "does not rest with States." Would a self-hosting AI economy be the encyclical's "culture of power" in its purest form, power with no person behind it?

---

## Sources and verification notes

**Primary**
- *Magnifica humanitas*, [vatican.va](https://www.vatican.va/content/leo-xiv/en/encyclicals/documents/20260515-magnifica-humanitas.html) and the [CBCEW English PDF](https://www.cbcew.org.uk/wp-content/uploads/sites/3/2026/05/Magnifica-Humanitas-EN.pdf). ¶¶1–101 were read directly. Quotes from ¶99 are cross-checked against The Tablet, PC Gamer, Ascension Press and The Register, which agree word for word.
- Leo XIV, [presentation address](https://www.vatican.va/content/leo-xiv/en/speeches/2026/may/documents/20260525-presentazione-enciclica.html), 25 May 2026.
- Leo XIV, [message on AI, ethics and corporate governance](https://www.vatican.va/content/leo-xiv/en/messages/pont-messages/2025/documents/20250617-messaggio-ia.html), 17 June 2025, and [message to the Builders AI Forum](https://www.vaticannews.va/en/pope/news/2025-11/pope-leo-xiv-message-builders-ai-forum-ethical-technology.print.html), 7 Nov 2025.
- DDF and DCE, [*Antiqua et Nova*](https://www.ewtn.com/catholicism/library/antiqua-et-nova-29133), 28 Jan 2025.
- Chris Olah, [remarks at the presentation](https://www.anthropic.com/news/chris-olah-pope-leo-encyclical), Anthropic, 25 May 2026.

**Secondary**
- [Crux](https://cruxnow.com/vatican/2025/05/pope-leo-says-he-chose-name-due-to-revolution-of-artificial-intelligence) on the papal name; PBS NewsHour (9 May 2025) on biography; [CNA/NCRegister](https://www.ncregister.com/cna/pope-leo-unveils-his-encyclical-ai-has-even-greater-consequences-than-industrial-revolution) on the launch; Religion News Service (25 May 2026); [The Tablet excerpts](https://www.thetablet.co.uk/features/the-human-person-risks-being-reduced-to-a-cog-in-a-machine-key-excerpts-of-magnifica-humanitas/); [The Register](https://www.theregister.com/ai-ml/2026/05/27/anthropic-co-founder-hallucinates-ghost-in-the-machine-after-hearing-the-pope-speak-about-ai/5246671).
- [Futurism](https://futurism.com/artificial-intelligence/anthropic-lobbying-vatican) and [Fast Company](https://www.fastcompany.com/91617738/anthropic-pope-ai-consciousness), both relaying the *New York Times*, 29 Sept 2026.

**Not verified, or verified only secondhand**
- The *New York Times* article itself, which was blocked. Everything in §2.4 under "What was reported later" is secondhand.
- The two RNS quotes in §2.3 ("a more moral AI…", "Disarming does not mean…"). They are not found in ¶¶1–101, so they probably come from later chapters. Paragraph numbers are unknown.
- Paragraphs after ¶101, including the bodies of the transhumanism, governance and weapons sections. A fuller pass could add these.
- The Leo XIV X posts are as reported by Fast Company and Futurism.
- The Rome Call description is general background and wasn't re-checked in this pass.
