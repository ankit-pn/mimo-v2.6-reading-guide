# MiMo-V2.6 post-training: a flow diagram

Post-training has **two related but distinct tracks** in the report: the flagship Pro/Flash training path and the smaller open 9B research path. Keeping them separate makes the evidence much easier to interpret.

## 1. Flagship MiMo-V2.6 path

```mermaid
flowchart LR
    A["Pre-trained + mid-trained<br/>MiMo-V2.6"] --> B["Short SFT"]
    B --> C["Mixed-task agentic RL<br/>GRPO across code, general, visual, cyber"]
    C --> D["MOPD2<br/>Integrate capabilities from specialized teachers"]
    D --> E["MiMo-V2.6-Pro / Flash"]

    V["MixRL teachers<br/>Verifiable tasks"] --> D
    O["SFT teachers<br/>Open-domain tasks with hard-to-design rewards"] --> D

    classDef start fill:#eaf2f8,stroke:#35627a,color:#152a38
    classDef train fill:#f2f6e9,stroke:#647c3a,color:#243018
    classDef transfer fill:#f3edf6,stroke:#765486,color:#30223a
    classDef result fill:#fff2df,stroke:#a46b1c,color:#3d2a10
    class A start
    class B,C train
    class D,V,O transfer
    class E result
```

The main model is already pretrained and mid-trained before post-training begins. A short SFT stage is followed by mixed-task RL. After mixed RL, **MOPD2** combines teachers: mixed-RL teachers for verifiable tasks and SFT teachers for open-domain tasks where reliable reward design is difficult. [§4, p. 8; §5.1, p. 20; §5.6, pp. 24–25]

## 2. What happens inside the main RL loop

```mermaid
flowchart TD
    P["Current policy"] --> S["Sample Mixer<br/>Select a balanced task batch"]
    S --> G["Prompt group<br/>16 rollouts per prompt"]
    G --> A["Agent harness + environment<br/>Code · General · Visual · Cyber"]
    A --> T["Multi-turn trajectories<br/>Actions + tool/environment feedback"]
    T --> V["Task verifier<br/>Tests, rubrics, visual metrics, or rule checks"]
    V --> Q["Groupwise quality grading<br/>GRS rubrics / GAR comparisons<br/>Correct confirmed hacks"]
    Q --> R["Rewards and advantages<br/>Length + behavior shaping; loss masks"]
    R --> U["GRPO policy update<br/>Router frozen for stability"]
    U --> P

    classDef policy fill:#eaf2f8,stroke:#35627a,color:#152a38
    classDef explore fill:#f2f6e9,stroke:#647c3a,color:#243018
    classDef judge fill:#f3edf6,stroke:#765486,color:#30223a
    classDef update fill:#fff2df,stroke:#a46b1c,color:#3d2a10
    class P policy
    class S,G,A,T explore
    class V,Q,R judge
    class U update
```

The flagship batch uses **1,568 prompts × 16 rollouts**, or about 25K trajectories and 2.7–3.7B tokens per step. GRPO learns from group-relative advantages. Verifiers establish task outcomes; GRS adds reusable quality rubrics on selected high-pass-rate code tasks, while GAR compares attempts online on other code tasks. Length penalties, segment-level behavior penalties, and confirmed-hack correction refine the signal. [§4.1, pp. 8–9; §4.3, pp. 16–20; §5.1, pp. 20–21]

## 3. Open 9B research path

```mermaid
flowchart LR
    A["MiMo-generated training data<br/>77.4B tokens; 27.2B loss tokens"] --> B["SFT Qwen3.5-9B"]
    B --> C["MiMo-V2.6-Distill-Qwen-9B"]
    C --> D["Separate domain-specific GRPO runs<br/>Code · Cyber · General · Visual"]
    D --> E["Open RL baselines + released tasks,<br/>verifiers, framework, mini-harnesses"]

    classDef data fill:#f3edf6,stroke:#765486,color:#30223a
    classDef model fill:#eaf2f8,stroke:#35627a,color:#152a38
    classDef rl fill:#f2f6e9,stroke:#647c3a,color:#243018
    classDef open fill:#fff2df,stroke:#a46b1c,color:#3d2a10
    class A data
    class B,C model
    class D rl
    class E open
```

This is the open research track, not a reproduction of the trillion-parameter mixed-task run. The team distills MiMo-generated data into a Qwen3.5-9B SFT checkpoint, then runs GRPO separately on released domain environments. The released task sets total about 7K across code, cyber, general, and visual domains, with roughly another 1K music-generation tasks in the training resources. [§7.1–7.2, pp. 33–35, Tables 4–6]

## MOPD2 in plain language

MOPD2 means **Multi-Prefix Multi-Teacher On-Policy Distillation**. It gives the student token-level guidance from different teachers depending on the task:

- On verifiable tasks, the student can generate a full rollout and learn from a mixed-RL teacher.
- On tasks with difficult-to-design rewards, an SFT teacher trained on synthetic demonstrations can provide guidance.
- Prefix-conditioned OPD reuses a history from a teacher trajectory or SFT example, then has the student generate the next turn itself. It does not simply copy a fixed continuation.

This broadens the post-trained model beyond tasks with convenient automatic verifiers, while still relying on teacher-generated supervision. [§5.6, pp. 24–25, Fig. 13]

## The three stages to remember

**SFT gives a starting behavior → RL improves behavior through environment outcomes and group comparisons → MOPD2 transfers capabilities from specialized teachers.** The open 9B work follows a parallel SFT-then-RL path to make parts of this research reproducible at smaller scale.

**Source:** [*MiMo-V2.6: Scaling Reinforcement Learning Towards Self-Improvement*](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning), §§4–7.
