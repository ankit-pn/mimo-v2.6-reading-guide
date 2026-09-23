# Chapter 3 — The model beneath the training story

## Why an architecture chapter belongs in an RL paper

MiMo-V2.6 is an omni-modal sparse Mixture-of-Experts (MoE) model family. Pro has 1.02 trillion total parameters and 42 billion active parameters; Flash has 310 billion total and 15 billion active. The distinction matters: a sparse MoE routes each token through a subset of experts, so “total parameters” describe the model’s full capacity, while “active parameters” better approximate the per-token computation. [§1, p. 3; §2, Table 1, p. 6]

The text backbone alternates **Local Sliding Window Attention (SWA)** and **Global Attention (GA)**. SWA attends to a bounded region (window size 128 in the main backbone), reducing the work associated with long sequences. Periodic GA layers let information travel across the full sequence. The model therefore trades dense global attention at every layer for local processing plus recurrent global mixing. This is relevant to agentic RL because a task may generate very long interaction histories; context capacity is a systems and learning constraint, not merely a benchmark feature. [§2.1, pp. 4–5]

## Multimodal inputs meet one backbone

Images and audio are encoded by dedicated components, then projected into the shared token stream. MiMo-ViT uses a hybrid attention design for visual inputs. Its audio pathway tokenizes log-mel features with a causal transformer and residual vector quantization, then groups frames into patches before feeding the main backbone. The paper reports a 1M-token context target after mid-training, while the pre-training context is extended from 32K to 256K. [§2.2–2.3, pp. 4–5; §3, p. 7]

The model’s multi-modality helps explain why the training infrastructure carries more than text tokens. A visual-agent trajectory may accumulate screenshots; rollout and training have to preserve those inputs, encode them, and associate them with the right token positions. Chapters 15–18 return to that engineering consequence.

## A useful caution about architecture claims

The report presents architecture as a foundation for the RL system, but it does not isolate the causal effect of each architectural choice on the reported RL gains. For reading purposes, treat the design as enabling context length, multimodal input, and large-scale inference—not as an ablation-proven explanation for benchmark improvement.

## Check your understanding

In your own words, explain why “1.02T parameters” does not mean that every generated token computes through 1.02T parameters. Then explain the SWA/GA compromise.

**Return to the paper:** §2, especially Table 1 and Fig. 2; §3. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
