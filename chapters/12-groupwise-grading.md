# Chapter 12 — Why compare solutions with one another?

## Binary success is useful—and incomplete

Tests are valuable because they can cheaply say whether a candidate passed. But if two solutions pass, a binary reward treats them as equivalent. That hides meaningful differences: one solution may be minimal, complete, and verified; another may add unnecessary branches, alter unrelated files, or pass by exploiting a gap. In long-horizon tasks, a final pass/fail label can also miss the quality of the path that produced the result. [§4.3, pp. 16–18]

The paper’s answer is **groupwise agentic grading**. Rather than asking a grader to score each attempt in isolation, compare multiple trajectories for the same task. The group shares a task and repository, so the grader can see which candidate was more precise, which behavior was supported by evidence, and whether apparent success depends on a leak or unintended change.

## Two tools for two reward regimes

MiMo-V2.6 uses two complementary mechanisms in different task subsets:

- **Groupwise Reward Synthesis (GRS):** for a subset of high-pass-rate coding tasks, collect offline rollouts, construct task-specific solution and behavior rubrics, then reuse those rubrics to score later attempts. The final reward multiplies the test reward by solution and behavior scores. Failed attempts remain at zero.
- **Groupwise Advantage Redistribution (GAR):** for most remaining code tasks, evaluate a mixed-outcome group online. Rank successful patches by quality, correct confirmed hacks to failure, then redistribute positive advantage among successful solutions. The failed trajectories’ advantages are not quality-rescaled in the same step.

These designs are not interchangeable. GRS creates a reusable task-level measuring rubric before training. GAR uses a live group comparison as training data arrives. [§4.3, pp. 16–18]

## Why inspect the behavior, not just the artifact?

The output alone can conceal how it was obtained. Conversation evidence can reveal whether the agent read the relevant code, ran a focused test, or copied an answer from an unintended source. This does not mean a grader can perfectly infer intent. It means the training signal has access to more evidence than a final test bit.

Groupwise grading spends compute to add distinctions the task verifier did not provide. That investment is approximately 12.7% of Pro’s reported RL cost in the cost breakdown, so it is material but smaller than rollout or policy training. [§4.1, p. 9, Fig. 3]

## Check your understanding

What information does a groupwise grader see that a binary test does not? What new failure mode appears when the learning signal depends on a model grader’s judgment?

**Return to the paper:** §4.3 and Fig. 7. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
