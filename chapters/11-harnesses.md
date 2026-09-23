# Chapter 11 — Why the harness belongs in the training mixture

## A model acts through a software interface

An agent harness determines how a model receives instructions, calls tools, manages context, and responds to tool results. Change the harness and the same model may face different tool names, context conventions, retry logic, or interaction constraints. A policy trained in one fixed harness can therefore learn habits that fit that wrapper rather than general problem-solving. [§4.2.5, pp. 14–15]

The authors argue that production harnesses are not always ideal for controlled RL. They may contain extra safeguards and workflows whose requirements are not measured by the task reward; a model can learn to ignore them. Their tightly coupled modules are also difficult to vary one at a time. Xiaomi instead constructs **mini-harnesses** from a shared minimal agent loop with decoupled system prompt, tools, and context-management components. Configurations can be recombined across code, general, visual, and cyber tasks. [§4.2.5, p. 14]

## The transfer test

The paper’s main-model multi-harness experiment trains on four mini-harnesses and evaluates on those plus three held-out harnesses: Codex, Claude Code, and mini-swe-agent. On DeepSWE, the mean pass@1 over held-out harnesses rises from approximately 50% to 66% across training. The paper also reports gains across the individual training harnesses. That is more informative than testing only on the training wrapper: it suggests that at least some learning transfers across interaction implementations. [§5.3, pp. 22–23, Fig. 10]

The 9B study adds a different view. Its multi-harness RL condition improves scores across seven harnesses and three coding evaluations relative to the SFT checkpoint; the table’s mean on MiMo Code Bench (mini) rises from 53.1 to 59.0. [§7.2, pp. 35–36, Table 7]

## What this does—and doesn’t—establish

Held-out harnesses support a transfer claim, but only for the harnesses and tasks tested. They do not prove that the learned policy is invariant to arbitrary tool interfaces or that all gains come from harness diversity: the training also includes additional RL. A decisive ablation would hold task data, compute, and reward fixed while changing only the harness mixture.

## Check your understanding

Why is a held-out harness a test of generalization that a held-out benchmark alone may not be? What has to stay fixed to attribute the gain specifically to multi-harness training?

**Return to the paper:** §4.2.5; §5.3, Fig. 10; §7.2, Table 7. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
