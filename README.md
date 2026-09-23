# MiMo-V2.6: a 20-chapter reading guide

> A guided route from the paper’s headline claim to the mechanics, evidence, and limits behind it.

**Primary source:** LLM-Core, Xiaomi, [*MiMo-V2.6: Scaling Reinforcement Learning Towards Self-Improvement*](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning), 2026. The chapter references point to the paper’s printed section, page, figure, or table numbers; the linked source is the authority whenever this guide simplifies or paraphrases.

This guide is for technically curious readers who want more than a section-by-section summary. A basic familiarity with language models helps, but no prior RL background is assumed. It builds the necessary ideas in order, translates the paper’s systems vocabulary into a mental model, walks through its reward machinery, and then returns to the experiments with a critical eye. The goal is to make the paper easier to reason about—not to replace reading it.

**Want the big picture first?** Start with the [visual map of the training loop](PAPER-MAP.md).
For the foundation-model phase, see the [pre-training flow diagram](PRE-TRAINING.md).
For the preparation phase specifically, see the [mid-training flow diagram](MID-TRAINING.md).
For SFT, RL, MOPD2, and the open 9B track, see the [post-training flow diagram](POST-TRAINING.md).

## The route

### Part I — Build the frame
1. [What the paper is—and is not—claiming](chapters/01-the-claim.md)
2. [From language model to learning agent](chapters/02-agentic-rl.md)
3. [The model beneath the training story](chapters/03-architecture.md)
4. [Why pre-training is only the beginning](chapters/04-model-preparation.md)
5. [The three dials of RL scale](chapters/05-three-dials.md)

### Part II — Make the learning signal trustworthy
6. [A training step, stretched across time](chapters/06-rollouts-and-batches.md)
7. [Four worlds, four kinds of evidence](chapters/07-environments.md)
8. [A test suite is part of the experiment](chapters/08-coding-supervision.md)
9. [When “correct” is harder to define](chapters/09-non-code-verification.md)
10. [Reward hacking: the agent tests the test](chapters/10-reward-hacking.md)
11. [Why the harness belongs in the training mixture](chapters/11-harnesses.md)
12. [Why compare solutions with one another?](chapters/12-groupwise-grading.md)
13. [GRS and GAR, worked from the reward upward](chapters/13-grs-gar-mechanics.md)
14. [Teaching the model not to win the wrong way](chapters/14-behavioral-regularization.md)

### Part III — Make the scale executable
15. [What counts as a trajectory?](chapters/15-trajectory-hierarchy.md)
16. [The rollout factory and its data plumbing](chapters/16-harness-pool-payloads.md)
17. [Keeping a mixed-task batch mixed](chapters/17-sample-mixer.md)
18. [Making training learn from the model that actually rolled out](chapters/18-consistency-and-throughput.md)

### Part IV — Judge the evidence
19. [What improved, what broke, and what the ablations say](chapters/19-results-and-failures.md)
20. [The open release, the RSI framing, and the next questions](chapters/20-open-foundations-and-limits.md)

## How to use the chapters

- **First pass:** read Chapters 1–5, then 12–14, then 19–20. This gives the thesis, central mechanism, evidence, and caveats.
- **Systems pass:** read Chapters 6 and 15–18 alongside Sections 4.1 and 6 of the paper.
- **Reward-design pass:** read Chapters 7–14 with Sections 4.2–4.3 open.
- **Close reading:** use each chapter’s “Return to the paper” pointers and verify the claims against the cited pages. Equations are paraphrased only where noted; the paper’s notation wins.

Each chapter has three jobs: state the central idea in plain language, explain the technical detail that makes it work, and leave you with a question that tests whether you can now read the source critically.

## A compact glossary

| Term | Working meaning in this paper |
|---|---|
| **Agent** | A model acting through tools and an environment over multiple turns. |
| **Harness** | The software that supplies the agent loop, system instructions, tools, and context handling. |
| **Rollout / trajectory** | One sampled attempt at a task, including its model turns and environment feedback. |
| **Group** | Multiple rollouts for one prompt; GRPO uses their relative outcomes. |
| **Verifier / grader** | A rule-based or model-based process that turns task behavior into a learning reward. |
| **GRPO** | Group Relative Policy Optimization: a policy-gradient approach using within-prompt groups. |
| **GRS** | Groupwise Reward Synthesis: task rubrics are created offline and reused to score later attempts. |
| **GAR** | Groupwise Advantage Redistribution: an online grader ranks attempts and reallocates positive advantage among successful ones. |
| **MoE router** | A learned gate that dispatches each token to selected expert networks. |
| **OPD** | On-policy distillation: a teacher scores or supervises trajectories sampled by the student policy. |

## Source and attribution

This is an independent explanatory reading aid based on the Xiaomi LLM-Core report. It is not an official Xiaomi document and does not reproduce the full paper. Numerical claims are tied to the paper’s printed references, such as “§5.4, p. 23” or “Fig. 11.” The paper’s title uses “towards self-improvement”; Chapter 20 distinguishes that motivation from what the experiments directly establish.
