# Chapter 2 — From language model to learning agent

## The unit of learning is an attempt, not a reply

A conventional language-model example can be pictured as prompt → completion. An agentic task is better pictured as **request → actions → observations → revised actions → outcome**. The model may inspect files, call tools, run tests, observe errors, edit an artifact, and repeat. The resulting trajectory contains many model-generated turns and many pieces of environment feedback. [§1, p. 3; §6.1, pp. 26–27]

That difference changes the learning problem. A benchmark answer can often be checked once. An agent must receive credit for an outcome produced by a chain of decisions. Did it inspect the right evidence? Did it use a tool correctly? Did it make a change that fixed the issue without breaking other behavior? Did the final artifact meet the user’s intent? Reward is no longer just a label attached to a final string; it is a judgment over an interaction.

## A minimal formal picture

For a prompt \(q\), the rollout policy samples a group of responses or trajectories \(o_1, \ldots, o_G\). The environment and grader assign rewards; the training algorithm turns those rewards into advantages \(A_i\). The policy is updated to increase the likelihood of sampled actions with positive advantage and decrease the likelihood of actions with negative advantage. In this paper, each trajectory can be long and multi-turn, so the objective also needs token-level masks and importance-sampling ratios. [§4.1, pp. 8–9, Eq. (1)]

The important conceptual move is to compare attempts **for the same task**. If sixteen attempts all receive an identical reward, there is little relative information in that group. If some succeed and some fail, the difference gives a basic learning signal. If all succeed but differ in precision, simplicity, or verification behavior, a groupwise grader can still tell the optimizer which successful behavior is preferable.

## Why “agentic” changes what counts as quality

Suppose two patches pass every available test. One changes the smallest necessary code path and checks the relevant regression. The other adds broad compatibility branches, suppresses exceptions, and relaxes validation. A binary test reward calls them equal. A maintainer may not. The paper’s groupwise grading aims to put some of that distinction back into training. [§4.3.2, pp. 18–19]

This does not make the grader an oracle. It makes the reward definition richer—and therefore creates new questions about grader consistency, evidence, and bias. Chapters 7–14 develop those questions.

## Check your understanding

Why can a model improve a task score while becoming a worse software collaborator? What extra evidence would you want the reward to recognize?

**Return to the paper:** §1; §4.1; §6.1. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
