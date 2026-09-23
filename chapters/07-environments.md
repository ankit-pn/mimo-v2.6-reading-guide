# Chapter 7 — Four worlds, four kinds of evidence

## The environment defines what “good” means

MiMo-V2.6 trains across code, general professional workflows, visual creation, and cybersecurity. These domains are not simply four prompt categories. Each has a different object of success and therefore a different verification problem. Coding can often use executable tests. A professional workflow may produce a document and update a database. Visual design has many valid answers. Cybersecurity needs to distinguish the intended vulnerability from unrelated crashes. [§4.2, pp. 9–14]

| Domain | What the agent does | Evidence used for reward |
|---|---|---|
| Coding | Modify repositories, implement features, resolve issues | Tests, specification–test audits, repeated execution |
| General | Work across documents, databases, and software tools | Atomic checks plus model-judged rubric items |
| Visual | Build or reproduce rendered artifacts | Runtime/layout rubrics, visual similarity, groupwise judgment |
| Cyber | Reproduce a described real-world vulnerability | Sanitizer report’s bug type and project-level crash frame |

The key design goal is **reward alignment**: the verifier should accept different valid ways of solving a task, reject incomplete work, and do so consistently enough that policy updates respond to the task rather than quirks of the checker.

## Why diversity matters—and complicates interpretation

Training on several domains may encourage broader behaviors: planning, tool use, checking intermediate results, and adapting to different interfaces. Yet the same diversity makes credit assignment more difficult. A single batch mixes different pass rates, token lengths, task durations, and grader types. The Sample Mixer exists partly because a nominal task percentage does not guarantee that the corresponding task actually occupies that percentage of the accepted training data. [§5.1, p. 20; §6.3, pp. 29–31]

The main RL mixture is reported as 68% agentic and competitive coding, 12% general tool use, 13% aesthetic design, 3% context following, and 4% cybersecurity. These are task proportions, not a claim that each domain contributes equal tokens, wall-clock time, or gradient magnitude. [§5.1, p. 20]

## A question to carry forward

If the score improves in all four domains, that supports breadth. But to know whether the model learned transferable agency rather than a set of domain-specific tricks, we need evaluations on held-out tasks, new harnesses, and reliable external measures. The paper tests some of this—especially held-out coding harnesses—but not every dimension equally.

## Check your understanding

Pick one domain and write down (a) the intended capability, (b) the observable proxy used by its reward, and (c) one way that proxy could be wrong.

**Return to the paper:** §4.2; §5.1–5.2. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
