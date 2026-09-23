# Chapter 5 — The three dials of RL scale

## Scale is not one number

The paper organizes RL scaling around three dimensions:

1. **Training computation:** larger batches, more rollouts, longer sequences, and higher throughput.
2. **Environment and harness diversity:** more kinds of tasks and more ways for the model to interact with them.
3. **Grader computation:** more work spent distinguishing which attempts are actually better.

Think of these as three dials on one learning loop. More rollout compute without useful tasks produces more low-value experience. More task diversity with weak verifiers can teach the wrong lesson. A strong grader that cannot keep up can become the throughput bottleneck. The paper’s main contribution is the attempt to scale the dials together. [Abstract; §1, pp. 1, 3; §4, p. 8]

## The operational scale

The headline training batch contains 1,568 prompts and 16 rollouts per prompt—about 25,000 trajectories. Each step processes 2.7–3.7 billion training tokens, with roughly 110K–150K tokens per sequence and contexts up to 1M. The reported RL post-training costs are $2.6M for Pro and $0.9M for Flash. For Pro, the paper attributes about 43.8% of cost to rollout, 43.5% to training, and 12.7% to grading. [§4.1, pp. 8–9, Fig. 3]

These numbers are not decorative scale markers. They help explain why engineering details later in the paper—partial rollout, asynchronous grading, distributed payload storage, task-aware scheduling, router stability—are necessary. At this size, an inefficient verifier or a single overloaded driver can waste substantial compute.

## “More compute” still needs an interpretation

The paper plots DeepSWE average@3 against cumulative RL cost. Pro rises from 58.4 to 72.6 and Flash from 48.7 to 65.7 over training. That is encouraging evidence that capability improved during the run. It is not a clean compute-only scaling law: the tasks, grading, sampling, and policy all participate in the same evolving training program. The safer reading is that **this co-designed RL program improved as it consumed compute**, not that dollars alone caused the gain. [§4.1, p. 8, Fig. 3]

## Check your understanding

For each dial, name one failure that can happen if the other two are not scaled with it. Which dial do you think is the hardest to measure fairly?

**Return to the paper:** §4.1 and Fig. 3; compare with §4.2–4.3. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
