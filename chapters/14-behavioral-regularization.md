# Chapter 14 — Teaching the model not to win the wrong way

## Success can grow longer without getting better

The authors observe that generated token counts can rise during RL. Longer reasoning or more tool use may help, but length can also become a way to chase reward: extra turns, broad fallback branches, repeated attempts, or unnecessary code. MiMo-V2.6 uses a **group-relative length penalty** to discourage successful responses that are substantially longer than other successful responses to the same prompt. [§4.3.3, pp. 19–20]

The reference length is a chosen percentile of successful rollouts in that prompt’s group. A penalty is applied only when the group pass rate clears a threshold, and only to successful rollouts longer than the reference by more than a tolerance. The deduction ramps up and saturates at a configured maximum. The intuition: compare a solution with what this particular task seems to require, not with one global token budget. Difficult prompts keep room to explore because low-pass-rate groups are not penalized by the same rule. [§4.3.3, pp. 19–20, Eq. (4)]

## Put credit on the right turns

An outcome reward is often broadcast across all model-generated tokens in a trajectory. Yet some turns can be off-path, malformed, repetitive, or affected by an infrastructure failure. The paper’s **Penalty Module** detects problematic segments, contexts, or whole sequences and applies actions at the appropriate level: mask tokens from loss, shape their advantage, or merely monitor them. An early-stop rule can halt a bad rollout, zero its outcome reward, treat triggering and earlier turns differently, and mask sibling contexts. [§6.1, pp. 26–27]

For format errors and tool-call mistakes, segment-level behavioral penalties mask flagged tokens in positively rewarded trajectories and penalize flagged tokens more in negative trajectories. The scheme redistributes advantage mass across flagged and unflagged tokens to avoid accidentally creating excessive overall negative pressure. [§4.3.3, pp. 20–21, Eq. (5)]

## The underlying principle

Reward design is not only deciding which final answer wins. It is deciding **which parts of which trajectory should receive credit**. Length penalties constrain efficiency; segment penalties target local behavior; groupwise graders distinguish whole-solution quality. The different scales of intervention complement each other.

## Check your understanding

Why is a per-prompt reference length more sensible than a single global maximum? Name one way a length penalty could accidentally suppress useful behavior, and explain how the pass-rate gate is meant to reduce that risk.

**Return to the paper:** §4.3.3, Eqs. (4)–(5); §6.1. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
