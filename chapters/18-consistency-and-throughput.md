# Chapter 18 — Making training learn from the model that actually rolled out

## Tiny numerical differences can change a discrete decision

The rollout engine is SGLang; the training engine is Megatron-LM. They run on different systems and numerical pathways. With an MoE model, a small difference in a router score can change which expert receives a token. With top-p sampling, a changed probability distribution can change which tokens are included in the candidate set. If training evaluates a different path from the one that generated the trajectory, the update is not quite learning from the behavior that produced its evidence. [§6.4, pp. 32–33]

The paper addresses this mismatch in layers:

- **QDQ after updates:** quantize–dequantize expert weights using the same MXFP4 constraints as rollout, so inference and training see aligned expert weights.
- **Rollout Routing Replay (R3):** record the selected expert IDs during rollout and replay them during training, preserving the discrete route.
- **Candidate-set replay:** record each token’s top-k or top-p candidate set and renormalize training probabilities over that same set. At a typical top-p of 0.97, the paper reports fewer than five tokens on average in the candidate set after an initial fixed-width bitmap transfer.

The paper describes R3 and candidate-set replay as compact and negligible-overhead. [§6.4, p. 32]

## Cache work while the environment is thinking

Each dialogue context has a persistent cache key. While a policy version is unchanged, later turns prefill only the new suffix. During tool execution, cached state can move from HBM to pinned host memory; restoration and offload use side CUDA streams so they do not block generation. Historical images remain represented in cached KV state, so only newly introduced visual inputs need to cross process boundaries. [§6.4, p. 32]

For throughput, the authors use a block-6 DFlash speculative decoder fine-tuned on early RL logs. Average accepted length is reported as 31.3% higher than the inherited MTP configuration. In a mixed-task evaluation, block-6 improves global average throughput by about 6% over block-8; an RL-adapted FP8 draft model yields about 10.3% higher per-node throughput than the baseline on the long-context workload. These are engineering metrics, not direct capability gains. [§6.4, pp. 32–33]

## Check your understanding

Explain why replaying router choices can improve training–inference consistency even when both engines use the same nominal weights. Why is throughput improvement different from a benchmark-score improvement?

**Return to the paper:** §6.4, especially pp. 32–33. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
