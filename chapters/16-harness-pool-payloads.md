# Chapter 16 — The rollout factory and its data plumbing

## Keep the control plane light

A training driver should decide what to run and where it belongs; it should not have to hold every token, routing record, screenshot, and probability vector in memory. MiMo-V2.6 separates the **control plane** (scheduling metadata) from the **data plane** (large trajectory payloads). Payloads are written once to distributed storage; the driver schedules using rewards, lengths, and payload keys. Training workers fetch only the pieces needed for their partition. [§6.2, pp. 27–29, Fig. 14]

This matters because one batch may contain tens of thousands of sequences, each carrying token IDs, log probabilities, MoE routing data, top-p candidate sets, and multimodal inputs. A screenshot-heavy path can accumulate gigabytes. Gathering all payloads on one driver would make that machine the batch-size ceiling. Distributed storage plus targeted packing avoids moving the full batch through a single bottleneck. [§6.2, pp. 28–29]

## Why persistent actor pools?

The system executes agent harnesses and Agent Loops with Ray actors. Creating one actor for every harness instance and rollout would burden Ray’s global control store with actor/file-descriptor overhead. Instead, fixed-size host actor pools run many persistent concurrent tenants. Environment calls and tokenization that might block run on background threads, so one slow operation does not stall every tenant sharing the event loop. Different harness codebases use separate pools, while task mixture can change independently of the fixed actor allocation. [§6.2, pp. 27–28]

## Pack where the data will be consumed

At batch yield, scheduling still operates on metadata. A packer associated with a training tensor-parallel group fetches only the unpadded sequence rows and context-parallel windows needed for its ranks. This avoids aggregating all tensors on the driver and avoids dense, padded intermediates. For images, metadata guides load balancing; pixels are fetched when the vision encoder needs them, then embeddings are redistributed to the ranks holding the matching tokens. [§6.2, pp. 28–29]

The pattern is a systems principle worth remembering: **move small descriptions centrally; move large payloads only to their consumers.**

## Check your understanding

Which parts of a trajectory should a scheduler need to see to choose the next sample? Which parts should remain in distributed storage until a grader or training worker actually needs them?

**Return to the paper:** §6.2 and Fig. 14. [Read the source](https://www.alphaxiv.org/abs/2609.mimo-scaling-reinforcement-learning).
