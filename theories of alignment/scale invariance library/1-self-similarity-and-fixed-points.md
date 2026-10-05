# 1. Self-similarity, fixed points and universality

Fractals, the renormalization group and dynamical systems. These are the papers where "the same step at every scale" was first made precise, and where it was learned which details survive the zoom and which wash out.

Back to the [library index](README.md). Quote labels are explained there: ✔ means two fetches returned the same wording; *(single fetch)* means it was seen once and should be checked against the PDF before it is quoted elsewhere. Math notation inside quotes came through a text-extraction tool and is approximate.

---

## Hutchinson (1981), Fractals and self similarity

**Citation:** Hutchinson, J. E. (1981). Fractals and self similarity. *Indiana University Mathematics Journal* 30(5): 713–747.
**Read at:** [the author's copy at ANU](https://maths-people.anu.edu.au/~john/Assets/Research%20Papers/fractals_self-similarity.pdf). Cited by section, since the PDF's page numbers are not the journal's.

**What it is.** The paper that made "self-similar set" a theorem. Take a finite family of contraction maps (each shrinks distances). Apply all of them to a set and take the union. Repeat. Whatever set you start from, you converge to one unique fractal, the set left unchanged by the step. The Sierpinski triangle from [Discussion 1](../../scalable%20oversight/history.md) is the textbook example.

**Alignment's name for it:** "iterate the procedure"; "the fixed point of amplification" (HCH); "convergence of the distillation loop".

- **What is invariant.** §1: "We say the compact set $K \subset \mathbf{R}^{n}$ is invariant if there exists a finite set $\mathcal{S}=\{S_{1}, \ldots, S_{N}\}$ of contraction maps on $\mathbf{R}^{n}$ such that $K=\bigcup_{i=1}^{N} S_{i} K$" ✔ (one fetch printed "contraction maps on $K \subset \mathbf{R}^n$"; check the journal text)
- **The step alone fixes the result.** §1: "It turns out, somewhat surprisingly at first, that the invariant set $K$ is determined by $\mathcal{S}$. In fact, for given $\mathcal{S}$ there exists a unique compact set $K$ invariant with respect to $\mathcal{S}$." *(single fetch)*
- **Existence and uniqueness.** §3.1(3)(i): "There is a unique closed bounded set $K$ which is invariant with respect to $\mathcal{S}$. Thus $K=\bigcup_{i=1}^{N} S_{i}(K)$. Moreover $K$ is compact." ✔
- **Convergence from any start.** §3.1(3)(viii): "In particular $\mathcal{S}^{p}(A) \rightarrow K$ in the Hausdorff metric." ✔ (for any non-empty bounded starting set $A$)
- **Why errors shrink.** §3.2(1): "$\mathcal{S}$ is a contraction map on $\mathcal{B}$ (respectively $\mathcal{C}$) in the Hausdorff metric." ✔ and "Existence and uniqueness of a closed bounded invariant set $|\mathcal{S}|$ follow from the contraction mapping principle." *(single fetch)*

**Our read.** This is the ideal case for **C2e**: exact self-similarity, proved at every level at once, with nothing to measure. The proof needs exactly one thing: the repeated step is a *contraction*. Then any starting error dies out geometrically (**C2c**) and the end state doesn't depend on where you started (**C2b**). The question it trains: *is the oversight step actually a contraction?* A step that only preserves error, or shrinks it sometimes, gets none of this. §5 adds that being invariant is not enough to be cleanly self-similar: the pieces also must not overlap too much (the "open set condition"). A step can work and still produce a degenerate result.

---

## Mandelbrot (1967), How long is the coast of Britain?

**Citation:** Mandelbrot, B. (1967). How long is the coast of Britain? Statistical self-similarity and fractional dimension. *Science* 156(3775): 636–638.
**Read at:** [Humboldt State course copy](https://gsp.humboldt.edu/olm_2021/Courses/GSP_510/Articles/Mandelbrot1967.pdf) (second copy at gis.humboldt.edu).

**What it is.** The measured length of a coastline depends on the length of the ruler, and keeps growing as the ruler shrinks. The *slope* of log-length against log-ruler, read off measurements at many ruler sizes, is a stable number (the fractal dimension). The coastline is self-similar only statistically.

**Alignment's name for it:** "results at several model sizes"; "the trend holds across scales"; a scaling-law plot.

- **Statistical self-similarity.** Abstract, p. 636: "Geographical curves are so involved in their detail that their lengths are often infinite or, rather, undefinable. However, many are statistically "self-similar," meaning that each portion can be considered a reduced-scale image of the whole." ✔
- **The measurement depends on resolution.** p. 636: "as ever finer features are taken account of, the measured total length increases, and there is usually no clearcut gap between the realm of geography and details with which geography need not be concerned." ✔
- **How it is measured.** p. 636: "Thus, one obtains an estimate of the length to be called L(G). Unfortunately, geographers will disagree about the value of G, while L(G) depends greatly upon G." *(single fetch)*. Richardson's law, $L(G) \sim M G^{1-D}$, came back garbled; check it against the PDF.
- **Exact versus statistical.** p. 637: "self-similar figures are seldom encountered in nature (crystals are one exception). However, a statistical form of self-similarity is often encountered, and the concept of dimension may be further generalized." ✔
- **It is an inductive bet.** p. 637: "Empirical scientists having to be content with less than perfect inductions, I favor the more positive interpretation stated at the beginning of this report." *(single fetch)*

**Our read.** The template for **C2e** and **C2f**. A slope only exists across several rulers, and Mandelbrot calls the self-similar reading an induction, not a theorem. A weak-to-strong or sandwiching result at one capability gap is one value of $L(G)$; it says nothing about $D$. He also shows that the measured "length" of a property can depend entirely on the resolution you probe at, which applies to oversight quality measured on easy versus hard tasks.

---

## Kadanoff (1966), Scaling laws for Ising models near Tc

**Citation:** Kadanoff, L. P. (1966). Scaling laws for Ising models near Tc. *Physics Physique Fizika* 2(6): 263–272.
**Read at:** [APS open PDF](https://journals.aps.org/ppf/pdf/10.1103/PhysicsPhysiqueFizika.2.263). A scan; the words are reliable, the symbols less so.

**What it is.** The block-spin idea. Group the magnet's atoms into blocks, treat each block as one effective spin, and claim that the blocks obey *the same kind of model* as the atoms, with new parameters. Repeat, and you get scaling laws.

**Alignment's name for it:** "a team of overseers plus assistants acts like one stronger overseer"; "the amplified human"; "the distilled model plays H at the next level".

- **The block.** Abstract, p. 263: "The description is based upon dividing the Ising model into cells which are microscopically large but much smaller than the coherence length and then using the total magnetization within each cell as a collective variable." ✔
- **Same form, new parameters.** p. 265: "This sum is, of course, just an Ising model calculation with coupling constant K̃ and effective magnetic field h̃." ✔ (the tildes are uncertain)
- **The window where the step is valid.** p. 264: "We take L to be large, but much smaller than the coherence length, ξ, which describes the range of spin correlations, measured in lattice constants." ✔
- **What is assumed, stated openly.** p. 265: "In writing (7) we are asserting that the correlations among cells can be totally represented by these interactions among near neighbors and that there are no less direct interactions that we need include in (7) as long ranged interactions. This statement, together with the assertion that the cell can be represented by the double valued variable, μ_a, are the two basic assumptions of this model." *(single fetch)*

**Our read.** The clearest example of **C2a** and **C2b**. Every recursive oversight scheme makes Kadanoff's move: coarse-grain, then claim the coarse system has the same form with better parameters. The habit to copy is his honesty: he lists "same form" as one of "the two basic assumptions". Ask of each alignment method what it assumed about the form staying the same, and where it says so. The step also has a validity window (blocks much bigger than one site, much smaller than ξ), which is a **C2d** question: at what capability does the "team acts like one overseer" picture stop holding?

---

## Wilson (1982), The renormalization group and critical phenomena (Nobel lecture)

**Citation:** Wilson, K. G. (1983). The renormalization group and critical phenomena. *Reviews of Modern Physics* 55(3): 583–600. Nobel Lecture, 8 December 1982.
**Read at:** [nobelprize.org PDF](https://www.nobelprize.org/uploads/2018/06/wilson-lecture-2.pdf). Page numbers are the Nobel volume's, not the RMP's.

**What it is.** Kadanoff's step turned into a transformation you can iterate and analyze. Iterate it, and the system flows to a fixed point where atomic details no longer matter (universality). Some deviations from the fixed point shrink with each iteration; a few grow.

**Alignment's name for it:** "small errors wash out at higher levels"; "robust to the details of the human"; on the failure side, "errors compound", "the bias gets amplified".

- **The purpose.** p. ~102: "The 'renormalization group' approach is a strategy for dealing with problems involving many length scales." ✔
- **The repeated step.** p. 111: "If F_L and F_{L+δL} are expressed in dimensionless form, then one finds that the transformation leading from F_L to F_{L+δL} is repeated in identical form many times." ✔
- **Fixed point and universality.** p. 111: "As L becomes large the free energy F_L approaches a fixed point of the transformation, and thereby becomes independent of details of the system at the atomic level." ✔ "This leads to an explanation of the universality of critical behavior for different kinds of systems at the atomic level." ✔
- **The deviation that grows.** p. ~111–112: "The c term designates an instability of the fixed point, namely a departure from the fixed point which grows as L increases." ✔ Then: "The fixed point is reached only if the thermodynamic system is at the critical temperature for which c vanishes; any departure from the critical temperature triggers the instability." *(single fetch)*
- **Truncating the step.** p. 119: "In other words the couplings should have an order of importance, and for any desired but given degree of accuracy only a finite subset of the couplings would be needed." ✔

**Our read.** The vocabulary for **C2b**, **C2c** and **C2d**. Under iteration, deviations split into those that die out (physicists call them *irrelevant*; that dying-out is universality) and those that grow (*relevant*; Wilson's "c term"). Iterating more cannot fix a relevant deviation; you have to sit exactly on the critical surface. For an oversight step, ask which human errors are irrelevant (noise that averages out over levels) and which are relevant (a consistent bias that every level passes on and amplifies). One relevant direction breaks scale invariance no matter how many other errors shrink. This sharpens the "stable basin" option in C2b: the human judgment needs to sit at a fixed point that is *stable in every direction that matters*, not just in most of them.

---

## Feigenbaum (1980), Universal behavior in nonlinear systems

**Citation:** Feigenbaum, M. J. (1980). Universal behavior in nonlinear systems. *Los Alamos Science* 1(1): 4–27. Reprinted with minor additions in *Physica D* 7 (1983): 16–39.
**Read at:** [the Physica D reprint, Tallinn University of Technology course page](https://www.tud.ttu.ee/web/dmitri.kartofelev/mittelindyn/paper4.pdf). **Page numbers are Physica D's.** The 1980 LANL original could not be reached, so wording may differ slightly.

**What it is.** Dynamical systems theory's version of renormalization. As a parameter is turned, many different systems (dripping taps, populations, fluids) double their period again and again on the way to chaos, and the ratio between successive doublings is the same number, δ ≈ 4.669, for all of them. The reason: one step (compose the map with itself and rescale) acts identically at every level.

**Alignment's name for it:** "the same mechanism at every level of the hierarchy"; "only a few properties of the human matter".

- **The cascade.** p. 16–17: "This process of successive period doubling recurs continually (with the range of parameter values for which the period is 2ⁿT becoming successively smaller as n increases) until, at a certain value of the parameter, it has doubled ad infinitum" ✔
- **Universality.** p. 17: "δ = 4.6692016 ...." ✔ "Thus, this definite number must appear as a natural rate in oscillators, populations, fluids, and all systems exhibiting a period-doubling route to turbulence!" ✔ "That is, so long as a system possesses certain qualitative properties that enable it to undergo this route to complexity, its quantitative properties are determined." ✔
- **The same step at every level.** p. 25: "Basically, the mechanism that f^(2^n) uses to period double at λ_(n+1) is the same mechanism that f^(2^(n+1)) will use to double at λ_(n+2)." *(single fetch)*
- **Only the local shape matters.** p. 25: "in the infinite period-doubling limit, all functions with a quadratic extremum will have identical behavior." *(single fetch)*
- **Stable directions and one unstable one.** p. 30: "Indeed, it turns out that infinitesimal deformations (conjugacies), of g determine stable directions, while a unique unstable direction, h, emerges with a stability rate (eigenvalue) precisely the δ of eq. (3)." ✔
- **How fast the limit arrives, and how to test it.** p. 31: "Thus, typically after the first two or three period doublings, this asymptotic theory is already accurate to within several percent." ✔ p. 39: "A measurement of δ from its fundamental definition would, of course, be altogether convincing." ✔

**Our read.** Universality comes out of a repeated step (**C2a**), and the invariant that must survive is small: only "the nature of f's maximum" (**C2b**). That is the strongest outside support for the weaker reading of C2b, that what must stay fixed may be one qualitative property of human judgment rather than the whole function. The theory is asymptotic but already accurate after two or three levels (**C2e**), and Feigenbaum's test is the right shape for **C2f**: measure the ratio between successive levels, not one level.

---

## Stanley (1999), Scaling, universality, and renormalization: three pillars

**Citation:** Stanley, H. E. (1999). Scaling, universality, and renormalization: Three pillars of modern critical phenomena. *Reviews of Modern Physics* 71(2): S358–S366.
**Read at:** [Academia.edu upload](https://www.academia.edu/5447709/Scaling_Universality_and_Renormalization_Three_Pillars_of_Modern_Critical_Phenomena). The author's site and APS were unreachable, so check wording against RMP before quoting elsewhere.

**What it is.** A short review that works as a glossary for the three ideas above, plus the standard empirical test for scaling: data collapse.

- **The three pillars.** §III: "The recent past of the field of critical phenomena has been characterized by several important conceptual advances, three of which are scaling, universality, and renormalization." *(single fetch)*
- **Data collapse, the test.** §IV.C: "In Fig. 1, the scaled magnetization M_H is plotted against the scaled temperature ε_H, and the entire family of M(H=const,T) curves "collapse" onto a single function." *(single fetch)*
- **Universality classes.** §V: "Two systems with the same values of critical-point exponents and scaling functions are said to belong to the same universality class." ✔ "Empirically, one finds that all systems in nature belong to one of a comparatively small number of such universality classes." ✔
- **Renormalization.** §VI.C: "The critical point can be mapped onto a fixed point of a suitably chosen transformation on the system's Hamiltonian." ✔
- **Where the simple picture breaks.** §VI.C: "For d<d_1, the classical theory breaks down in the immediate vicinity of the critical point because statistical fluctuations neglected in the classical theory become important." ✔

**Our read.** For **C2f**, data collapse is a better standard than a single trend line: curves taken at different settings (here, different capability gaps or overseer strengths) should fall on one curve after rescaling, and if they don't, the claimed scaling is wrong. For **C2b**, universality says only a few coarse features decide the behavior; an alignment method claiming scale invariance should say which features of the overseer–model setup it thinks fix its "class". For **C2d**, the breakdown sentence is a general pattern: a mean-field (averaged) argument fails where fluctuations stop being small.

---

## Bak, Tang and Wiesenfeld (1987), Self-organized criticality

**Citation:** Bak, P., Tang, C., & Wiesenfeld, K. (1987). Self-organized criticality: An explanation of 1/f noise. *Physical Review Letters* 59(4): 381–384.
**Read at:** [UCSD course copy](https://courses.physics.ucsd.edu/2016/Spring/physics235/BTW-PRL.pdf).

**What it is.** The sandpile. Add grains one at a time; the pile organizes itself into a critical state, with no tuning, where avalanches of every size occur with power-law frequency.

**Alignment's name for it:** "training pushes the system to the edge of what the overseer catches"; "most failures are small and caught".

- **No tuning needed.** Abstract and p. 381: "We show that dynamical systems with spatial degrees of freedom naturally evolve into a self-organized critical point." ✔ "The scaling properties of the attractor are insensitive to the parameters of the model." ✔ "This robustness is essential in our explaining that no fine tuning is necessary to generate 1/f noise (and fractal structures) in nature." ✔
- **Minimal stability.** pp. 381–382: "We call such a state locally minimally stable." ✔ "The 'clusters' of minimally stable states must be defined dynamically as the spatial regions over which a small local perturbation will propagate." ✔
- **The power law, measured.** p. 382–383: "The curve is consistent with a straight line, indicating a power law D(s) ~ s^−τ, τ ≈ 0.98." ✔
- **The cutoff, checked by varying size.** p. 383: "The falloff at the largest cluster sizes is due to finite-size effects, as we checked by comparing simulations for different array sizes." ✔

**Our read.** The counterpoint to Wilson. Here the scale-free state is an *attractor* (a stable basin, **C2b**), but that state is minimally stable: a small local perturbation can spread to any size (**C2c**). Robustness of the exponent is not safety. An optimization process pushing against an overseer could settle at the edge of what the overseer catches, where most failures are small and a few are as large as the system allows. The cutoff check (vary the system size and see if the falloff moves) is a model **C2f** experiment.
