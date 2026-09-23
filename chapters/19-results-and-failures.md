# Chapter 19 — What improved, what broke, and what the ablations say

## Read results in layers

The main training curves show both Pro and Flash improving on DeepSWE, AutomationBench, and MiMo Visual Coding during RL, with fluctuations between checkpoints. DeepSWE average@3 rises from 58.4 to 72.6 for Pro and from 48.7 to 65.7 for Flash over the training run. Other reported benchmarks cover cyber and coding tasks as well. Treat this as evidence that the **full training program** made progress across several task families. [§4.1, p. 8, Fig. 3; §5.3, pp. 22–23, Fig. 9]

The final comparison table is mixed rather than a sweep. MiMo-V2.6 Pro is competitive with named frontier models on some metrics, but trails on others. For example, Pro is at 71.9 on DeepSWE v1.1 versus 74.0 for Claude Opus 5 and 73.0 for GPT-5.6 Sol; Pro reports 82.0 on OSWorld-Verified versus 83.4 and 83.0 for those two baselines, while scoring 53.1 on AutomationBench compared with their 50.3 and 45.8. These comparisons help situate capability; they are not a claim of uniform state-of-the-art performance. [§5.2–5.3, pp. 21–22; Table 3, p. 26]

## What the ablations teach

- **Multi-harness:** held-out harness mean pass@1 on DeepSWE rises from about 50% to 66% over training, giving evidence of transfer beyond the training wrapper. [Fig. 10, p. 23]
- **Router freeze:** with a trainable router, layer-9 expert-load CV rises from 0.78 to 2.0, peak load from 6× to 16× mean, and cold experts from 0.5% to 22%. Freezing keeps CV near 0.7, peak load near 5.5×, and cold experts near 1% while benchmark performance grows. Restoring the original router at step 20 recovers load balance without changing benchmark performance, supporting the diagnosis that router drift drove the collapse. [§5.4, pp. 23–24, Fig. 11]
- **GAR:** pass-rate gains continue while turn count stays stable and token length grows more gradually than without online groupwise grading. [§4.3.2, pp. 18–19, Fig. 8]

These are mechanism-focused experiments and therefore especially valuable. Still, read the scope: the router comparison is at one layer and for a particular Pro run; the GAR result is a code-only Flash comparison with batch size 128; held-out harness evidence pertains to the tested harness set.

## Failures are part of the scale result

The report describes GPU-memory double-bit errors, a Kubernetes failure in the cyber cluster, grader network unavailability, partial-rollout length-estimation problems, expert imbalance within a microbatch causing OOM, and CPU-memory pressure during packing. The training program was not a frictionless run; it required restarts and fixes. The failures reveal where the scaling design is vulnerable: task duration estimates, per-rank expert load, host-memory volume, and grader availability. [§5.5, pp. 23–24, Fig. 12]

## Check your understanding

Which result is the clearest evidence for the groupwise-grading mechanism? Which is the clearest evidence for a scaling-system challenge? For each, state one limit on generalizing beyond the reported experiment.

**Return to the paper:** §5.3–5.5; Figs. 8–12; Table 3. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
