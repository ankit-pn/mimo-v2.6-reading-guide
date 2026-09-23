# Chapter 9 — When “correct” is harder to define

## General workflows: local realism, explicit checks

General-agent tasks combine files, databases, and software tools exposed through APIs, command lines, GUIs, or MCP. The authors build resettable local environments using real-world files and local software mocks, so rollouts avoid network variability and external service limits. A planning agent defines the workspace, tools, and data; multiple agents populate files and databases; a reviewer checks both individual artifacts and cross-file consistency. [§4.2.2, pp. 11–12]

Tasks use atomic binary rubric items. Deterministic properties—such as a database value or file format—can be checked in code. More open-ended deliverable quality can be judged by an LLM. Repeated judgments and cross-model agreement help surface ambiguous rubric items, while rollout review checks whether the rubric is too strict or too permissive. Negative checks catch unintended changes to unrelated files or databases. [§4.2.2, p. 12]

## Visual work: compare and judge

Visual tasks include websites, interactive applications, games, 3D scenes, slides, SVG, video, and Figma designs. The paper separates **open-ended design** from **high-fidelity replication**. For open-ended work, fixed rules cannot fully represent aesthetics; the authors combine pointwise rubrics (runtime correctness, instruction adherence, layout integrity, basic aesthetics) with groupwise comparison of rendered artifacts. For replication, the reference image permits rule-based similarity metrics, supplemented by an LLM’s holistic assessment. [§4.2.3, p. 13]

The distinction is useful: when many solutions can satisfy a request, ask whether each meets a minimum standard and whether some are better than others. When there is a target image, measure resemblance directly—but remember pixel similarity can penalize harmless variation or reward superficial resemblance.

## Cybersecurity: verify the intended failure

In vulnerability reproduction, a random crash is not success. Xiaomi extracts a vulnerability type and project-level crash location from sanitizer reports and accepts a proof-of-concept only if both match. The task description and verifier share this source of truth. This avoids known weaknesses in differential testing against patched binaries and the run-to-run variability of LLM-only verdicts. The task environment includes the source tree and compiled fuzzing harness. [§4.2.4, pp. 13–14]

This is an unusually clean example of a domain-specific verifier: it defines the capability narrowly enough to check deterministically. It also measures vulnerability reproduction, not every broader skill implied by “cybersecurity.”

## Check your understanding

For each of the three domains, name one quality dimension captured by the verifier and one dimension it may leave out.

**Return to the paper:** §4.2.2–4.2.4. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
