# Overview

## Product

**G1 — the product.** FuguCTX proposes a minimal repair for a broken OpenBSD
daemon configuration. The output is a unified diff. The operator reads the diff
and decides.

## The pilot

FuguCTX reads a broken daemon configuration and a parser error, and it proposes
the smallest fix as a diff. The build rehearses the FuguTTX production pipeline
at small scale, on the same components, at real prices. Cheap learnings are a
deliverable. A marker of the form "Rehearses: FuguTTX TRN-TRACES" ties a
component to the FuguTTX unit that it rehearses.

The pilot targets one gap above all others. OpenBSD supplies a deterministic
judge for every configuration: the parser of the daemon. The project can
therefore run the full FuguTTX trace loop, where a teacher proposes and a real
guest disposes.

**G2 — the pilot.** FuguCTX rehearses the FuguTTX pipeline before FuguTTX pays
full price. Where a FuguTTX component fits, FuguCTX must use that component.
This rule holds even where a lighter tool serves G1 alone.

**The rank rule.** G1 defines the interfaces. G2 defines the implementation.
When the two conflict, a decision in [DECISIONS.md](DECISIONS.md) records the
choice.

## Deliverables

The deliverables are the model, the `ctx` engine, the rollout driver, and
[the learning](LEARNING.md).

## Accepted costs

The design buys pilot value with real costs. The table names each one.

| Accepted cost                                  | Reason                                                    |
| ---------------------------------------------- | --------------------------------------------------------- |
| The model file is near 1.2 GB                  | The engine is a Qwen3-1.7B fine-tune in GGUF form         |
| Cold start takes seconds                       | llama.cpp loads a quantized model at start                |
| Training needs a cloud GPU                     | The training pipeline mirrors FuguTTX D3, on purpose      |
| Data generation needs a live OpenBSD guest     | Every observation must be real, per [C6](DECISIONS.md#c6) |
| The teacher bills GPU time during each rollout | The driver reaches the teacher on the train instance      |

The last row is a design cost, and it is also a lesson. The rollout rate bounds
the teacher cost. FuguTTX carries the same coupling in its trace campaign, so
LEARNING must measure it here first.

## The name

CTX carries three true meanings, and each one fits the project.

- **Ciguatoxin.** CTX and TTX act on the same voltage-gated sodium channel. TTX
  blocks the channel, and CTX opens it. FuguCTX opens a system that a broken
  configuration blocks.
- **Context.** `ctx` is the common short name for context in systems code. The
  model reads the configuration, the parser error, and the lines around the
  fault.
- **Context diff.** The context lines of a diff anchor every edit. A diff is the
  product.

A fugu chef removes the dangerous part, and serves the rest. FuguCTX removes the
dangerous part of a repair, which is the write. The tool proposes, and the
operator decides. The name is still a scope statement.
