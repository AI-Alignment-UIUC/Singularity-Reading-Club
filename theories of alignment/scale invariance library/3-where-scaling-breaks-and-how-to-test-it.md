# 3. Where scaling breaks, and how to test a scaling claim

Condensed-matter philosophy, fluid mechanics, turbulence, crisis prediction and statistics. These papers are about the edges: the range a scaling law holds over, the exponents that drift, the events that sit off the curve, and the tests that separate a real power law from a straight-looking line. This is the group to read for **C2d**, **C2e** and **C2f**.

Back to the [library index](README.md). ✔ means two fetches returned the same wording; *(single fetch)* means it was seen once. Greek letters lost in PDF extraction are restored in [brackets].

---

## Anderson (1972), More is different

**Citation:** Anderson, P. W. (1972). More is different: Broken symmetry and the nature of the hierarchical structure of science. *Science* 177(4047): 393–396.
**Read at:** [MIT course copy](https://www.mit.edu/~kardar/research/seminars/EPB/PWA-Science72.pdf) and [KIT course copy](https://www.tkm.kit.edu/downloads/TKM1_2011_more_is_different_PWA.pdf).

**What it is.** Knowing the rules for the parts does not let you predict the whole. At each level of scale, new behavior appears that needs new concepts, often through sharp phase transitions that only exist in the large-system limit.

**Alignment's name for it:** "emergent capabilities"; "qualitatively new behavior at scale"; the sharp left turn.

- **The thesis.** p. 393: "The constructionist hypothesis breaks down when confronted with the twin difficulties of scale and complexity." ✔
- **No simple extrapolation.** p. 393: "The behavior of large and complex aggregates of elementary particles, it turns out, is not to be understood in terms of a simple extrapolation of the properties of a few particles." ✔
- **New laws at each level.** p. 393: "Instead, at each level of complexity entirely new properties appear, and the understanding of the new behaviors requires research which I think is as fundamental in its nature as any other." ✔
- **Sharp transitions need size.** p. 395: "The essential idea is that in the so-called N → ∞ limit of large systems (on our own, macroscopic scale) it is not only convenient but essential to realize that matter will undergo mathematically sharp, singular 'phase transitions' to states in which the microscopic symmetries, and even the microscopic equations of motion, are in a sense violated." ✔
- **Reduction is not reconstruction.** p. 393 or 394 (the copies disagreed): "The ability to reduce everything to simple fundamental laws does not imply the ability to start from those laws and reconstruct the universe." ✔

**Our read.** The opposing hypothesis to scale invariance, and the one every C2 verdict should be tested against. It trains the **C2d** habit: before accepting "the same step applied again", ask what new phenomena the next capability level brings. Because transitions only become sharp in the large limit, a small-gap experiment (weak-to-strong, sandwiching) may sit on the "few particles" side of a transition, where modelling the overseer or strategic deception simply doesn't exist yet. The last quote cuts against **C2b**: knowing the local rule for judging one step doesn't let you reconstruct the global property.

---

## Barenblatt: intermediate asymptotics and incomplete similarity

**Target paper (not reached):** Barenblatt, G. I. & Zel'dovich, Ya. B. (1972). Self-similar solutions as intermediate asymptotics. *Annu. Rev. Fluid Mech.* 4: 285–312. Annual Reviews and Cambridge were blocked.
**Substitutes read (Barenblatt's own later open papers):**
- (S1) Barenblatt, Bertsch, Chertock & Prostokishin (2000). Self-similar intermediate asymptotics for a degenerate parabolic filtration-absorption equation. *PNAS* 97(18): 9844–9848. [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC27602/).
- (S2) Barenblatt, Chorin & Prostokishin (1999). Self-similar intermediate structures in turbulent boundary layers at large Reynolds numbers. [arXiv:math/9908044](https://arxiv.org/abs/math/9908044).
- (S3) Barenblatt, Chorin & Prostokishin (2014). Turbulent flows at very large Reynolds numbers: new lessons learned. *Physics-Uspekhi* 57(3): 250–256. [Publisher's PDF](https://ufn.ru/ufn14/ufn14_3/ufn143d.pdf).
- (S4) Barenblatt, Chorin & Prostokishin (1999). The Kolmogorov–Obukhov exponent in the inertial range of turbulence: a reexamination of experimental data. [arXiv:math/9909107](https://arxiv.org/abs/math/9909107).

**What it is.** Two ideas. *Intermediate asymptotics:* a self-similar solution describes a system only in a middle range, after the details of the start are forgotten and before the final state takes over. *Complete versus incomplete similarity:* in complete similarity the exponent is fixed and universal; in incomplete similarity the exponent itself depends on a hidden parameter, so the "law" quietly changes as you move.

**Alignment's name for it:** "the trend holds in the range we tested"; a scaling exponent fitted at current model sizes.

- **Scaling lives in a middle range.** S1, abstract: "Numerical experiments indicate that the self-similar solutions obtained represent intermediate asymptotics of a wider class of solutions when the influence of details of the initial conditions disappears but the solution is still far from the ultimate state: identical zero." ✔
- **Incomplete similarity.** S3, §4: "The particular case of the second type is the incomplete similarity, whose existence is related to the invariance with respect to an additional renormalization group." ✔
- **The hidden parameter stays.** S2, §1: "This means that the influence of the Reynolds number, i.e. both of the viscosity and the external length scale, e.g. the pipe diameter, remains and should be taken into account in the intermediate region." *(single fetch)*
- **A universal law faked by sampling.** S3, §7: "Apparently, the measurements performed (or the data presented) in Nikuradse's work are only those that were close to the envelope, which led the author and those who followed him to assert the validity of the universal logarithmic law." *(single fetch)*
- **The exponent drifts.** S4, abstract: "The data are in fact consistent with incomplete similarity in the inertial range, and with an exponent that depends on the Reynolds number and tends to 2/3 in the limit of vanishing viscosity." *(single fetch)*

**Our read.** The vocabulary **C2e** is missing. "Statistical" self-similarity covers noise around a fixed slope; incomplete similarity is a slope that *moves* with a hidden parameter (for us: the capability gap, the task domain, the overseer's level). [Burns et al.'s](../papers/burns-2023-weak-to-strong.md) slope changing sign across tasks is this kind of result. Intermediate asymptotics says every self-similar regime has two edges (**C2d**), and the Nikuradse story is a warning for **C2f**: data sampled near one envelope can make a law look universal for decades.

---

## Kolmogorov (1941, 1962), the local structure of turbulence and its refinement

**Citations:**
- Kolmogorov, A. N. (1941/1991). The local structure of turbulence in incompressible viscous fluid for very large Reynolds numbers. English translation, *Proc. R. Soc. Lond. A* 434: 9–13 (1991).
- Kolmogorov, A. N. (1962). A refinement of previous hypotheses concerning the local structure of turbulence in a viscous incompressible fluid at high Reynolds number. *J. Fluid Mech.* 13: 82–85.

**Read at:** K41: [JHU course copy](https://www.ams.jhu.edu/~eyink/Turbulence/classics/Kolmogorov41a.pdf) and [UCSD course copy](https://courses.physics.ucsd.edu/2014/Spring/physics281/kolmogorov41.pdf). K62: [UCSD course copy](https://courses.physics.ucsd.edu/2019/Spring/physics235/Kolmogorov%2062-Enter%20Intermittency.pdf).

**What it is.** In 1941 Kolmogorov argued that turbulence is self-similar across a range of scales: energy cascades from big eddies to small ones by the same mechanism at every step, so the statistics depend only on one invariant (the energy flux ε) and the scale. Landau objected that if each step of the cascade is random, the randomness should pile up across steps. In 1962 Kolmogorov accepted this and corrected the exponents, with a correction that grows with the number of steps.

**Alignment's name for it:** "the human's judgment at each level is noisy but unbiased, so it averages out".

- **The range.** K41 §2, p. 10: "the hypothesis of local isotropy is realized with good approximation in sufficiently small domains G of the four-dimensional space (x1, x2, x3, t) not lying near the boundary of the flow or its other singularities." ✔
- **One invariant.** K41 §5, p. 12, second hypothesis of similarity: "the distribution laws Fn are uniquely determined by the quantity [ε] and do not depend on [ν]." ✔
- **The cascade.** K62, p. 82: the 1941 hypotheses rested on "Richardson's idea of the existence in the turbulent flow of vortices on all possible scales [ℓ] < r < L between the 'external scale' L and the 'internal scale' [ℓ] and of a certain uniform mechanism of energy transfer from the coarser-scaled vortices to the finer." *(single fetch)*
- **Landau's objection.** K62, p. 82: "But quite soon after they originated, Landau noticed that they did not take into account a circumstance which arises directly from the assumption of the essentially accidental and random character of the mechanism of transfer of energy from the coarser vortices to the finer: with increase of the ratio L: [ℓ], the variation of the dissipation of energy should increase without limit." ✔

**Our read.** K41 is the model clean scale-invariance argument: one repeated step (**C2a**), one invariant (**C2b**), an exact-looking exponent inside a bounded range (**C2e**). Landau's objection is the template for **C2c**: if each level carries independent random variation, the fluctuations don't average out, they compound with the number of levels. The K62 correction is small at shallow depth and grows with depth. For amplification, distillation or debate trees, per-level noise in an "invariant" human judgment does not stay bounded just because each step looks the same.

---

## Sornette (2009), Dragon-kings, black swans and the prediction of crises

**Citation:** Sornette, D. (2009). Dragon-kings, black swans and the prediction of crises. *International Journal of Terraspace Science and Engineering* 2(1): 1–18. [arXiv:0907.4290](https://arxiv.org/abs/0907.4290).
**Read at:** arXiv and the [UVM course copy](https://pdodds.w3.uvm.edu/files/papers/others/2009/sornette2009a.pdf).

**What it is.** The biggest events in many systems (market crashes, ruptures) are not just the far end of the power-law tail. They are "dragon-kings", produced by a different mechanism, usually positive feedback near a tipping point, and they sit off the curve fitted to everything else.

**Alignment's name for it:** rare catastrophic failures; "we've only seen small, caught misbehaviors so far".

- **A different mechanism.** Abstract: "These dragon-kings reveal the existence of mechanisms of self-organization that are not apparent otherwise from the distribution of their smaller siblings." ✔
- **Wilder than the extrapolation.** §3: "The following examples suggest that, in a significant number of complex systems, extreme events are even 'wilder' than predicted by the extrapolation of the power law distributions in their tail." ✔
- **Amplification at special times.** §4: "The fact, that dragon-kings belong to a statistical population, which is different from the bulk of the distribution of smaller events, requires some additional amplification mechanisms involving amplifying critical cascades active only at special times." ✔
- **Tied to transitions.** Abstract: "We emphasize the importance of understanding dragon-kings as being often associated with a neighborhood of what can be called equivalently a phase transition, a bifurcation, a catastrophe (in the sense of René Thom), or a tipping point." ✔
- **How to detect them.** §3.4: "the identification of the dragon-kings relies on the comparison between distributions of event sizes obtained at different resolution scales." ✔

**Our read.** The sharpest warning against extrapolating a fitted trend into the tail (**C2d**). A clean trend of small, caught misbehaviors across model sizes says little about coordinated deception or self-exfiltration, which would come from a different mechanism and sit off the curve. Sornette also gives a test (**C2f**): check statistically whether the largest observed failures belong to the same population as the rest, and compare distributions measured at different resolutions.

---

## Clauset, Shalizi and Newman (2009), Power-law distributions in empirical data

**Citation:** Clauset, A., Shalizi, C. R. & Newman, M. E. J. (2009). Power-law distributions in empirical data. *SIAM Review* 51(4): 661–703. [arXiv:0706.1062](https://arxiv.org/abs/0706.1062).
**Read at:** the arXiv v2 preprint (sections follow it; not checked against the SIAM print).

**What it is.** Most claimed power laws were found by fitting a straight line on a log-log plot, which is unreliable. The paper gives a proper procedure (maximum likelihood, choosing the range, goodness-of-fit, comparing against alternatives) and applies it to 24 published claims.

**Alignment's name for it:** any scaling-law plot; "the trend is linear on a log scale".

- **Log-log fits mislead.** Abstract: "Commonly used methods for analyzing power-law data, such as least-squares fitting, can produce substantially inaccurate estimates of parameters for power-law distributions, and even in cases where such methods return accurate answers they are still unsatisfactory because they give no indication of whether the data obey a power law at all." ✔
- **The range.** §1: "In practice, few empirical phenomena obey power laws for all values of x. More often the power law applies only for values greater than some minimum xmin." ✔
- **Straight is not enough.** §4: "Being roughly straight on a log-log plot is a necessary but not sufficient condition for power-law behavior." ✔
- **Extrapolation.** §5.3: "In particular, if we wish to extrapolate a fitted distribution far into its tail, to predict, for example, the frequencies of large but rare events like major earthquakes or meteor impacts, then conclusions based on different fitted forms can differ enormously even if the forms are indistinguishable in the domain covered by the actual data." ✔
- **The scorecard.** §6: "The p-values in Table 6.1 indicate that 17 of the 24 data sets are consistent with a power-law distribution. The remaining seven data sets all have p-values small enough that the power-law model can be firmly ruled out." ✔

**Our read.** The methods paper for **C2e** and **C2f**. Before trusting a scale-invariance claim, ask: was the range chosen on principle, is there a goodness-of-fit test, were other forms compared, and does the conclusion depend on extrapolating past the data? The §5.3 sentence is the key one for alignment: forms that agree over the capability gaps we can test can diverge enormously at the gap we care about.

---

## Mitzenmacher (2004), A brief history of generative models for power law and lognormal distributions

**Citation:** Mitzenmacher, M. (2004). A brief history of generative models for power law and lognormal distributions. *Internet Mathematics* 1(2): 226–251.
**Read at:** [the author's Harvard copy](https://www.eecs.harvard.edu/~michaelm/postscripts/im2004a.pdf).

**What it is.** Power laws and lognormals look alike on a plot, and tiny changes in the process that generates the data switch one into the other. Fields have argued about which one they have for decades.

**Alignment's name for it:** "errors drift multiplicatively"; "there is always a floor: the human can reject obviously bad steps".

- **A small change in the process.** §5: "This small change allows one model to produce a power law distribution while the other produces a lognormal." ✔ "As long as there is a bounded minimum that acts as a lower reflective barrier to the multiplicative model, it will yield a power law instead of a lognormal distribution." ✔
- **They look the same.** §2: "Despite its finite moments, the lognormal distribution is extremely similar in shape to power law distributions, in the following sense: If X has a lognormal distribution, then in a log-log plot of the complementary cumulative distribution function or the density function, the behavior will appear to be nearly a straight line for a large portion of the body of the distribution." ✔
- **An old argument.** §1: "A second discovery is the argument over whether a lognormal or power law distribution is a better fit for some empirically observed distribution has been repeated across many fields over many years." ✔
- **Why it matters.** §8: "Also, if one is attempting to predict future behavior based on current data, misrepresenting the tail of the distribution could have severe consequences." ✔

**Our read.** Whether a process is scale-free can hinge on one easily missed detail of the mechanism, such as a floor (**C2b**), and the data over the range we can test often can't tell the two apart (**C2e**). For an oversight method, a human who can always reject obviously bad steps acts like that floor; remove it and errors drift multiplicatively. The lesson for **C2f** is to argue from the generating mechanism, not only from a fitted curve, before extrapolating.
