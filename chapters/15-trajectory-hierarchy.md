# Chapter 15 — What counts as a trajectory?

## A conversation is not always one line

An agent may spawn subagents, compact its history, or keep multiple roles and dialogue branches alive. A flat prompt–completion record cannot represent all of that cleanly. MiMo-V2.6 uses a four-level hierarchy: **Sample → Sequence → Context → Segment**. [§6.1, pp. 26–27]

- A **Sample** is a prompt selected by the Sample Mixer.
- A **Sequence** is one Agent Loop execution; in group-relative training, one prompt can spawn several sequences.
- A **Context** is a dialogue branch. It is used for prefix matching, KV-cache reuse, and training-data export.
- A **Segment** is one turn, such as a system/user message, model generation, or tool result. Only model-generated turns contribute to the policy loss.

This hierarchy provides a map from events to credit. A bad tool result is not itself a model action. A model turn that invokes a nonexistent tool may be. An infrastructure failure might invalidate one sequence, while a repeated malformed response might be masked at the segment level.

## Separate detection from consequence

The Penalty Module makes two choices. A **Rule** detects something (handwritten condition or model judge). A **Strategy** describes what to do at a level of the hierarchy: mask, shape advantage, or monitor. This separation makes it possible to reuse a detector with different consequences, and to set consequences at the narrowest level that matches the fault. [§6.1, p. 27]

The system escalates when necessary. If a context contains no surviving model turns, drop the context. If a sequence has no surviving context, give it zero advantage. If the sample has no surviving sequence, reject the sample. The architecture is a practical answer to a credit-assignment question: when an agentic attempt contains many kinds of events, which ones should the optimizer learn from?

## Why this matters for reproducing the work

The hierarchy is not just a storage schema. It shapes which tokens are eligible for loss, where penalties apply, how partial contexts are cached, and how multi-turn requests are rebuilt. A reproduction that stores only final text and a scalar reward would lose many of the mechanisms the paper says are central to stable agentic RL.

## Check your understanding

An agent generates a malformed tool call, then receives an environment error, then retries successfully. Which items are model-generated segments? Which one might be masked or penalized, and why should the environment error not automatically be trained as if it were a model action?

**Return to the paper:** §6.1; compare with §4.3.3. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
