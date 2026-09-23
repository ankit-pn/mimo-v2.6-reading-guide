# Chapter 20 — The open release, the RSI framing, and the next questions

## The open track is a separate experiment

The main Pro and Flash RL runs are expensive and are not themselves the open reproduction baseline. Xiaomi releases [**MiMo-V2.6-Distill-Qwen-9B**](https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Distill-Qwen-9B), built by supervised fine-tuning Qwen3.5-9B on 77.4B tokens of MiMo-generated data, 27.2B of which contribute to the SFT loss. The mixture includes code, cyber, general, and visual data. The release also includes approximately 7,000 tasks across those four domains—about 3K code, 1K cyber, 1K general, and 2K visual—plus around 1K music-generation tasks, verifiers, an RL framework, and mini-harnesses. [§7.1–7.2, pp. 33–35, Tables 4–5]

On the 9B checkpoint, domain-specific GRPO improves all 11 evaluations reported in Table 6. Examples include SWE-bench Verified 61.1 → 66.2, Terminal Bench 2.1 37.1 → 52.8, MiMo Cyber Bench (mini) 31.3 → 47.0, and MiMo Visual Coding (mini) 64.0 → 72.4. These runs make the recipe more accessible and give evidence of RL gains on open resources. They are distinct, domain-specific experiments—not a complete reproduction of the trillion-parameter mixed-task training run. [§7.2, pp. 34–35, Table 6]

## MOPD2: combine teachers when rewards are hard to write

After mixed RL, the authors use Multi-Prefix Multi-Teacher On-Policy Distillation (MOPD2). For verifiable domains, a mixed-RL teacher supervises student rollouts. For open-domain tasks with difficult-to-design rewards, SFT teachers provide token-level supervision. Prefix-conditioned OPD reuses histories from teacher trajectories or SFT demonstrations, then lets the student generate a new turn from that context. This extends capability into areas such as game development, scientific research, and embodied intelligence where reliable RL verification is harder. It is still teacher-guided training, not autonomous self-improvement. [§5.6, pp. 24–25, Fig. 13]

## A careful conclusion about “self-improvement”

The report’s evidence supports a practical claim: large-scale, carefully instrumented agentic RL can improve a model across a range of interactive tasks, and smaller open models can also gain from RL on released environments. It does not demonstrate an autonomous loop in which a model discovers new learning objectives, creates and validates its own environments, redesigns its training algorithm, and repeats the process without human-designed infrastructure.

That distinction is not a dismissal. The paper makes concrete progress toward a necessary substrate for such research: exploration, more informative feedback, robust environments, and scalable infrastructure. It also leaves productive research questions:

1. Which component contributes the most when compute and task mixtures are held fixed?
2. How accurately do groupwise graders rank solutions, and do rankings transfer across tasks?
3. How much of the gain survives on independently authored, fully external evaluations?
4. How should a system distinguish legitimate tool use from reward exploitation as the policy changes?
5. Can the open 9B results reproduce beyond the benchmark families used to design the training tasks?

## The paper in one sentence

**MiMo-V2.6 argues that agentic RL scales only when exploration, reward quality, and execution infrastructure are scaled together—and provides a large-system report plus open components to support that program.**

## Final reading check

Close the paper and reconstruct the chain: model preparation → environment → rollout → verification/grading → advantage → policy update → consistency and scheduling. For every link, name one failure mode and the paper’s proposed defense. If you can do that, you have moved from recognizing terminology to understanding the design.

**Return to the paper:** §5.6; §7; §8. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
