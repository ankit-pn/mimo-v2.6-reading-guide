# Chapter 4 — Why pre-training is only the beginning

## The model must be ready to explore

The report separates three preparation stages. **Pre-training** builds broad language and multimodal capabilities. **Mid-training** shifts the model toward agentic tasks and very long contexts. A short **supervised fine-tuning (SFT)** stage then precedes the large RL run. The stages answer different questions: what the model knows, what interaction patterns it can attempt, and what behavior it begins RL from. [§3, p. 7; §4, p. 8; §5.1, p. 20]

MiMo-V2.6-Flash receives 48 trillion pre-training tokens: 26T in the text stage and 22T in the omni-modal stage. Pro receives 30T: 27T text and 3T omni-modal. Mid-training then mixes agent trajectories from coding, general, visual, and research work with text, repository code, and image/video/audio data. Context is extended from 256K to 1M in the final mid-training stage. [§3.1–3.2, p. 7]

## Why change optimizers before RL?

The base model uses AdamW for pre-training. For hidden weight matrices in mid-training, the authors switch to **Muown**, a Muon variant with row-norm control. Their stated motivation is that matrix-aware updates may retain data efficiency at large batch sizes, where AdamW’s elementwise adaptation becomes less attractive. Embeddings, the language-model head, and the MoE router remain on AdamW. The report says the switch caused no loss spike in mid-training; it does not claim this is a universal optimizer rule. [§3.2, p. 7]

Mid-training also uses MXFP4 quantization-aware training (QAT), preparing weights for low-precision rollout. This is an early example of a recurring theme: the model is trained partly for the system that will serve it. If inference arithmetic differs from training arithmetic, the policy that generates experience may not be the policy the optimizer evaluates.

## What this setup lets us conclude

The large-model RL results are not “RL starting from nothing.” They inherit enormous pre-training, an agent-oriented mid-training mixture, SFT, long-context adaptation, and low-precision preparation. This makes the main RL story a **stacked recipe**. Later, the released 9B model experiments give a more accessible test bed, but they still start from substantial SFT. [§7.1, pp. 33–35]

## Check your understanding

If RL gains depend on an agent-rich mid-training stage, what comparison would be needed to measure the contribution of that stage? Why is “RL improved the model” still a valid but narrower claim?

**Return to the paper:** §3.1–3.2; §5.1; §7.1. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
