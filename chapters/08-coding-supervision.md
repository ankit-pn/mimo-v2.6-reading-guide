# Chapter 8 — A test suite is part of the experiment

## Tests scale well; trustworthy tests take work

Coding is attractive for RL because code can be executed and checked. But “the tests passed” does not automatically mean “the requested task was solved.” A test suite can omit a requirement, demand an implementation detail never specified, be flaky, or be passed through a shortcut. In RL, those mistakes are not merely benchmark noise: they shape the gradient repeatedly. [§4.2.1, pp. 9–11]

The paper describes several sources of coding tasks: issue-linked GitHub pull requests; everyday internal development requests; detailed specification-driven coding; tasks synthesized from existing functionality in codebases; and long-horizon engineering tasks. It also supplements these with filtered public sources and licensed vendors. For GitHub cases without a sufficiently complete issue, a model reconstructs a task statement from the patch and repository context while being told not to reveal implementation-specific clues. [§4.2.1, pp. 9–10]

## Two checks: correctness and stability

**Accuracy** asks whether the tests capture the specification. The team has an auditing agent inspect four candidate rollouts alongside the task, reference patch, tests, patches, outputs, and conversation logs. A passing attempt judged incorrect suggests a false positive; a failing attempt judged correct suggests a false negative. Both trigger review. [§4.2.1, p. 11]

**Robustness** asks whether repeated execution gives the same reward. For tasks with reference patches, fail-to-pass tests should fail before the patch and pass afterward; pass-to-pass tests should pass both before and after. The report requires these outcomes to remain stable across eight reruns. This screens for flaky tests and environmental variation. [§4.2.1, p. 11]

The broader point: a benchmark’s verifier is not a neutral measuring instrument. It is part of the training intervention. Improving test coverage can change what behavior RL selects, even if the model and optimizer remain identical.

## An example of a test/specification mismatch

Imagine a task asking for CSV export. The tests additionally require a particular internal class name that the request never mentions. A correct alternative implementation fails: a false negative. Conversely, tests that check only that a file exists may pass an empty file: a false positive. Rollout-based audits help find both kinds of mismatch because they inspect the artifact and the trajectory, not just the final binary outcome.

## Check your understanding

Why does rerunning tests eight times address a different risk from asking an auditor to compare a rollout with the task specification?

**Return to the paper:** §4.2.1 and Fig. 4. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
