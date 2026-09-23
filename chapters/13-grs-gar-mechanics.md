# Chapter 13 — GRS and GAR, worked from the reward upward

## GRS: write the rubric from the task, not the winning attempt

For selected high-pass-rate tasks, GRS first collects multiple offline attempts. A grader studies the task specification, repository, and rollouts, then writes two rubric families:

- **Solution criteria:** requirements, edge cases, and consistency with the codebase.
- **Behavior criteria:** whether the agent gathered useful evidence and checked its changes.

The paper explicitly warns against turning one observed successful design choice into a universal requirement. A good rubric should be grounded in the task; different valid implementations should remain possible. Later attempts receive a solution score \(S_i^{sol}\) and a behavior score \(S_i^{beh}\). Their final reward is

\[
R_i = R_i^{test} \times S_i^{sol} \times S_i^{beh}.
\]

Because the base test reward is multiplicative, a failed attempt stays at zero. Passing attempts can still differ in quality, including when every attempt passes. [§4.3.1, pp. 17–18, Eq. (2)]

## GAR: move learning pressure among successful attempts

GAR handles other code tasks through online group comparison. The grader sees successful and failed trajectories together, and ranks passing patches on five dimensions: approach suitability, implementation precision, minimality, avoidance of unrelated effects, and craftsmanship in the repository’s conventions. It may inspect source and run targeted tests. Confirmed dependence on leaked or external answers resets the effective reward to zero. [§4.3.2, p. 18]

Let a group contain rewards \(R_i\), with group mean \(\bar R\). The initial sequence advantage is \(A_i=R_i-\bar R\). For passing members, a quality factor \(f_i\in(0,1]\) downweights lower-quality solutions. The paper rescales those positive advantages by a common factor \(\lambda\), chosen so that their total positive-advantage mass is preserved. Failed members keep their pre-redistribution values; the group mean is then removed so the final group advantages sum to zero. In plain terms: **move some of the group’s positive learning credit from weaker passes to stronger passes without changing the total positive credit available to passing behavior.** [§4.3.2, pp. 18–19, Eq. (3)]

The common rescaling is capped in practice to avoid excessive amplification. If grader output is unusable, the system falls back to original advantages. These safeguards matter: groupwise quality is a noisy judgment, not an exact physical quantity.

## What the ablation suggests

In code-only Flash RL, GAR’s presence is associated with pass-rate gains continuing through step 52 while turn counts stay roughly stable and total token length grows gradually. Without GAR, turns and length rise quickly, more rollouts hit length limits, and pass-rate improvement is harder to sustain. Maintainer-oriented audits also find more broad workarounds without online grading. This supports the proposed mechanism, though the experiment is still one training comparison rather than a universal guarantee. [§4.3.2, pp. 18–19, Fig. 8]

## Check your understanding

Why does GAR redistribute advantage instead of simply multiplying every passing trajectory’s advantage by its quality score? What is preserved by the normalization step?

**Return to the paper:** §4.3.1–4.3.2; Eqs. (2)–(3); Fig. 8. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
