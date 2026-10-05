# 2. Errors across stacked levels

Reliable computing, error correction, weather prediction, firms, numerical solvers, control theory and the structure of complex systems. Each of these stacks one step on top of itself, and each says exactly when error shrinks, stays bounded or compounds. This is the group to read for **C2c**.

Back to the [library index](README.md). ✔ means two fetches returned the same wording; *(single fetch)* means it was seen once; *(secondary)* means the author's words as quoted in another source, because the original could not be reached.

---

## von Neumann (1956), Probabilistic logics and the synthesis of reliable organisms from unreliable components

**Citation:** von Neumann, J. (1956). Probabilistic logics and the synthesis of reliable organisms from unreliable components. In C. E. Shannon & J. McCarthy (eds.), *Automata Studies*, Princeton University Press, 43–98.
**Read at:** [IAS archive copy of the 1952 Caltech lecture notes](https://static.ias.edu/pitp/archive/2012files/Probabilistic_Logics.pdf). Sections 1 to 10.5.3.2 only; the conclusions (§10.6) could not be read.

**What it is.** How to compute reliably with parts that fail. Without correction, every stage adds error and a deep enough machine produces noise. With a "restoring organ" (a majority vote over bundles of redundant lines) after each stage, error is pulled back toward a fixed point, *provided* each component's error rate is below a threshold.

**Alignment's name for it:** a check, vote or consistency step at each level; "reliability amplification".

- **Errors compound with depth.** §8.2: "Thus it appears that in the general case each operation of the automaton increases the probability of error since ε + 3η > η, so that if the serial depth of the machine (or rather of the process to be performed) is very great, it will be impractical or impossible to obtain any kind of accuracy." ✔
- **Drift to randomness.** §7.1: "after a long time, the memory content of the machine disappears" ✔
- **The restoring organ.** §9.2.2: "What is needed therefore is a new type of organ which will restore the original stimulation level." *(single fetch)*
- **Two attracting fixed points.** §9.2.3.2: "I.e. successive iterates of this process converge to 0 if the original α < 1/2 and to 1 if the original α > 1/2." ✔
- **The threshold.** §8.3.2 or §8.4 (the fetches disagreed): "ε < .0073 is the actual requirement, i.e. the error-level of a single basic organ function must be < .73%." ✔

**Our read.** The standard case for **C2c**. Without a corrective step at every level (**C2a**), depth alone destroys accuracy. With one, error is bounded at any depth, but only below a fixed per-step threshold, and the boundary is sharp (**C2d**). The restoring organ must be the same majority function at every stage (**C2b**). For amplification, recursive reward modeling or recursive summarization: does the method have a restoring step at each level (a check, a vote, a consistency test), and is its per-step error below the level where restoring wins? If not, depth works against it.

---

## Knill, Laflamme and Zurek (1998), Resilient quantum computation

**Citation:** Knill, E., Laflamme, R., & Zurek, W. H. (1998). Resilient quantum computation: error models and thresholds. *Proc. R. Soc. Lond. A* 454: 365–384. [arXiv:quant-ph/9702058](https://arxiv.org/abs/quant-ph/9702058).
**Read at:** the arXiv version (PDF and ar5iv), which may differ slightly from the published text.

**What it is.** The threshold theorem. Encode qubits in an error-correcting code, then encode the encoded qubits again, and again ("concatenation"). If the error per operation is below a threshold, each level squares the error, and arbitrarily long computations become reliable.

**Alignment's name for it:** "each level of the tree is checked by the level above"; "recursive verification".

- **The threshold.** Abstract: "Here we show that arbitrarily accurate quantum computations are possible provided that the error per operation is below a threshold value." ✔
- **The repeated step.** "Concatenation involves applying the same combination of techniques using the abstract qubits as the starting two state systems for encoding." ✔
- **Error from one level to the next.** "the effective error probability or strength is reduced (for sufficiently small p) from p to cp²" ✔
- **Error after many levels.** "The effective error on the encoded state after h iterations of the concatenation procedure is reduced to c^(2^h−1) p^(2^h)." ✔
- **What is assumed.** The abstract rests on "physically realistic assumptions on the errors" ✔, in particular errors that are roughly independent between operations.

**Our read.** The cleanest picture of **C2c** and **C2d** together. Each level maps error $p \to cp^2$. Below $p = 1/c$, error shrinks doubly exponentially with depth; above it, adding levels makes things worse. The threshold is a phase boundary, not a matter of degree. The invariant (**C2b**) is that errors stay roughly independent and bounded at every level. *Correlated* errors, the analogue of an overseer with the same blind spot at every level, break the $p \to cp^2$ argument. For any recursive method, ask: what is its per-level error map, has anyone estimated its $c$, and are its errors independent across levels? One sandwiching result is one point on that map, not the map.

---

## Lorenz (1969), The predictability of a flow which possesses many scales of motion

**Citation:** Lorenz, E. N. (1969). The predictability of a flow which possesses many scales of motion. *Tellus* 21(3): 289–307.
**Read at:** *Not reached.* Tellus and Taylor & Francis were blocked, and the MIT archive copy is an image scan with no text. The quotes below are Lorenz's words **as quoted by** T. Palmer, ["The real butterfly effect and maggoty apples"](https://physicstoday.aip.org/features/the-real-butterfly-effect-and-maggoty-apples), *Physics Today* 77(5) (2024), and by [Durran, Weyn & Gingrich, ECMWF seminar slides (2017)](https://www.ecmwf.int/sites/default/files/elibrary/2017/17618-upscale-and-downscale-error-growth.pdf). Rotunno & Snyder (2008, *J. Atmos. Sci.* 65: 1063) is an open follow-up if a primary source is needed.

**What it is.** The "real butterfly effect". In a flow with motion at many scales (the atmosphere), errors in the smallest scales grow fastest and spread to the next scale up, and so on. Because each smaller scale saturates faster, the total time for the error to reach the largest scale is a convergent sum. Making the initial error smaller buys almost nothing: there is a hard limit on predictability.

**Alignment's name for it:** "errors in sub-answers propagate up the tree"; "better leaf-level oversight will fix it".

- **The abstract.** "It is proposed that certain formally deterministic fluid systems which possess many scales of motion are observationally indistinguishable from indeterministic systems; specifically, that two states of the system differing initially by a small 'observational error' will evolve into two states differing as greatly as randomly chosen states of the system within a finite time interval, which cannot be lengthened by reducing the amplitude of the initial error." ✔ *(secondary)*
- **The conclusion.** "We have proposed that certain formally deterministic fluid systems possessing many scales of motion may be observationally indistinguishable from indeterministic systems, in that they possess an intrinsic range of predictability which cannot be lengthened by reducing the error of observation to any value greater than zero." ✔ *(secondary)*
- **What decides it.** Durran et al.'s summary (not Lorenz's words): for an energy spectrum $\propto k^{-p}$, the predictability time converges to a finite value if $p < 3$.

**Our read.** The counterexample to von Neumann and Knill et al. Here errors cascade *upward*, from fine to coarse scales, and the bottom-level error size stops mattering. For recursive oversight (**C2c**, **C2d**): if errors in fine-grained sub-judgments feed upward faster at each finer level, improving the leaf-level overseer has a ceiling. What decides it is how the error-growth rate scales with level (Lorenz's spectrum slope), not how small the leaf error is. So the useful measurement is a rate per level across several levels (**C2f**).

---

## Williamson (1967), Hierarchical control and optimum firm size

**Citation:** Williamson, O. E. (1967). Hierarchical control and optimum firm size. *Journal of Political Economy* 75(2): 123–138.
**Read at:** *Not reached* (paywalled, no open copy found). Quotes are Williamson's words **as quoted by** [van der Mandele & van Witteloostuijn (2015), "The inevitability and irreversibility of organizational uncontrollability", *Computational and Mathematical Organization Theory* 21: 380–405](https://link.springer.com/article/10.1007/s10588-015-9190-0) (open access). Page numbers are Williamson's, as cited there.

**What it is.** Why firms stop growing. Each level of management passes on only a fraction α of its superior's intentions. Add a level and fidelity multiplies by α again, so control decays geometrically with depth while the gains from size grow more slowly, giving a finite optimal size.

**Alignment's name for it:** delegation chains; "the model at level n is trained to do what level n−1 wanted"; HCH trees.

- **The per-level fraction.** p. 127: "the fraction α of the intentions of a superior effectively satisfied by a subordinate (0 < α < 1)" ✔ *(secondary)*
- **Compounding.** p. 127: "The cumulative loss of control as instructions and information are transmitted across successive hierarchical levels is responsible for this result." ✔ *(secondary)*
- **The limit.** p. 130: "the cumulative effects of control loss are fundamentally responsible for limitations in firm size" ✔ *(secondary)*

**Our read.** The "no restoring organ" baseline in social form: fidelity after $n$ levels is about $\alpha^n$ (**C2c**, compounds). For recursive reward modeling or HCH-style trees, the question is whether anything at each level pushes α back toward 1. Without one, deeper trees lose intent geometrically however capable each node is. It is also a **C3c** point: if fidelity falls with depth, so does the value of adding depth, so the method stops being worth running exactly where it is needed.

---

## Brandt (1977), Multi-level adaptive solutions to boundary-value problems

**Citation:** Brandt, A. (1977). Multi-level adaptive solutions to boundary-value problems. *Mathematics of Computation* 31(138): 333–390.
**Read at:** [AMS free copy](https://www.ams.org/journals/mcom/1977-31-138/S0025-5718-1977-0431719-X/S0025-5718-1977-0431719-X.pdf).

**What it is.** Multigrid. To solve an equation on a fine grid, smooth out the errors that are visible at that grid's scale, then hand the leftover smooth error to a coarser grid, where it looks rough again and can be smoothed there. Recurse. The convergence rate per cycle does not depend on the grid size, so the cost is linear in problem size.

**Alignment's name for it:** "each level catches the errors at its own granularity and passes the rest up".

- **Efficiency independent of scale.** p. 334: "This efficiency does not depend on the shape of the domain, the form of the boundary conditions, or the mesh-size" ✔
- **Linear cost.** Abstract, p. 333: "Interactions between these levels enable us (i) to solve the possibly nonlinear system of n discrete equations in 0(n) operations" ✔ (0(n) is the OCR of O(n))
- **When to hand off.** pp. 337–338: "As soon as the residuals are smoothed out, convergence slows down." ✔ "This is then exactly the point where relaxation sweeps should be discontinued and approximate solution of the (smoothed out) residual equations by coarser grids should be employed." ✔
- **The recursion.** p. 338: "The residual equations are in turn also solved by combining relaxation sweeps with corrections through still coarser grids, etc." ✔
- **The division of labor.** p. 362: "The roll of the finer levels, relative to the coarser ones, is only to liquidate high-frequency error components" *(single fetch; "roll" as printed or OCR'd)*

**Our read.** A positive example of exact scale invariance in an algorithm (**C2a**, **C2e**): one step, applied identically at every level, with a convergence factor that doesn't depend on the number of levels (**C2c**, bounded). It works because every kind of error is visible at *some* level. For recursive summarization or debate, the question is whether each level catches the errors at its own granularity and passes a faithful residual up. An error class that is visible at no level (the [obfuscated arguments](../papers/barnes-2020-obfuscated-arguments.md) worry) is exactly what breaks the multigrid picture (**C2d**).

---

## Lohmiller and Slotine (1998), On contraction analysis for nonlinear systems

**Citation:** Lohmiller, W. & Slotine, J.-J. E. (1998). On contraction analysis for nonlinear systems. *Automatica* 34(6): 683–696.
**Read at:** [MIT Nonlinear Systems Lab preprint](https://web.mit.edu/nsl/www/preprints/contraction.pdf) (the authors' copy; its pagination differs from Automatica's).

**What it is.** A local test for whether a system forgets disturbances: if nearby trajectories always move closer together (contraction), every perturbation dies out exponentially. Contraction is preserved when contracting pieces are combined in parallel, in a hierarchy, or in certain feedback arrangements, so a guarantee for the parts becomes a guarantee for the whole.

**Alignment's name for it:** "if each overseer–model pair is self-correcting, the whole stack is"; "the guarantee composes".

- **Forgetting perturbations.** Theorem 1: "any trajectory, which starts in a ball of constant radius centered about a given trajectory and contained at all times in a contraction region, remains in that ball and converges exponentially to this trajectory." ✔
- **Parallel combination.** §3.8.1: "If both systems are contracting in the same metric, so is any uniformly positive superposition" ✔
- **Feedback combination.** §3.8.2: "The augmented system is contracting if and only if the separated plants are contracting." ✔ **Caution:** this holds for the specific coupling set up in that subsection, not arbitrary feedback.
- **Hierarchy to any depth.** §3.8.3: "By recursion, the result can be extended to systems similarly partitioned in more than two equations." ✔ (the condition is a bounded coupling between levels)

**Our read.** The most direct formal model of "the guarantee composes" (**C2a**, **C2c**, **C2e**). The fine print is **C2b**: every piece must contract *in the same metric*, and the coupling between levels must be bounded or have the right structure. Translated: the overseer at each level must be correcting toward the same target, measured the same way, and no level may be able to push arbitrarily hard on the next. Weak-to-strong and amplification both assume something like this without saying so.

---

## Simon (1962), The architecture of complexity

**Citation:** Simon, H. A. (1962). The architecture of complexity. *Proceedings of the American Philosophical Society* 106(6): 467–482.
**Read at:** [UW course copy](https://homes.cs.washington.edu/~ztatlock/599z-17sp/papers/complexity-simon-62.pdf) and [UVM copy](https://pdodds.w3.uvm.edu/research/papers/others/1962/simon1962a.pdf).

**What it is.** Complex systems, natural and designed, tend to be hierarchies of nearly independent subsystems. The parable of two watchmakers: Tempus builds each watch in one piece and loses everything when interrupted; Hora builds stable subassemblies of ten and loses at most one subassembly.

**Alignment's name for it:** factored cognition; "decompose into independently checkable subquestions".

- **Hierarchy.** p. 468: "By a hierarchic system, or hierarchy, I mean a system that is composed of interrelated subsystems, each of the latter being, in turn, hierarchic in structure until we reach some lowest level of elementary subsystem." ✔
- **The cost of no intermediate forms.** p. 470: "Now if p is about .01-that is, there is one chance in a hundred that either watchmaker will be interrupted while adding any one part to an assembly-then a straightforward calculation shows that it will take Tempus, on the average, about four thousand times as long to assemble a watch as Hora." ✔
- **The condition.** p. 471: "The time required for the evolution of a complex form from simple elements depends critically on the numbers and distribution of potential intermediate stable forms." ✔
- **Near-decomposability.** p. 474: "(a) in a nearly decomposable system, the short-run behavior of each of the component subsystems is approximately independent of the short-run behavior of the other components; (b) in the long run, the behavior of any one of the components depends in only an aggregate way on the behavior of the other components." ✔

**Our read.** The structural precondition behind the rest of this group. Stable intermediate forms turn one long fragile chain into many short checkable steps, so a failure costs one subassembly, not everything (**C2c**, bounded). Near-decomposability is what decomposition-based oversight assumes about tasks (**C2b**). The test for factored cognition, debate or recursive summarization: are the subquestions really nearly decomposable, and are verified sub-answers really stable? If not, the method is Tempus. [Barnes (2020)](../papers/barnes-2020-obfuscated-arguments.md) is a case where they aren't.
