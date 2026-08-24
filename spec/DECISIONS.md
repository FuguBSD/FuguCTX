# Decisions

These twelve decisions control all plans. A plan must not go against a decision.
To change a decision, change this document first.

<a id="c1"></a>

## C1 — Base model: Qwen3-1.7B (Apache 2.0)

Mirrors FuguTTX D1: the same model family, the same tokenizer, and the same
license. The size sits between a token labeller and the 4B flagship. A repair
task needs more capacity than a tagger, and the run still costs single-digit
euros.

<a id="c2"></a>

## C2 — Inference: llama.cpp, CPU only, greedy decoding

Mirrors FuguTTX D2. The artifact is one GGUF file at Q8_0. Determinism holds
under three pins: the llama.cpp version, the thread count, and the model hash.
Every proposal carries the model hash. Details: [engine](engine.md).

<a id="c3"></a>

## C3 — Training: Axolotl QLoRA on a Scaleway H100, fully as code

Mirrors FuguTTX D3. The H100 is larger than a 1.7B run needs, and that is the
point. The pilot exercises the H100 quota, the live price, and the train stack
at low stakes. The L40S stays as the budget escape. Details:
[training](training.md), [infrastructure](infrastructure.md).

<a id="c4"></a>

## C4 — Method: SFT on verified repair pairs

The pilot runs no CPT pass. The base model already reads configuration syntax.
The corpus holds repair pairs, and each pair carries a verdict from a real
parser. This keeps the pilot short, and the register records the choice.
Details: [training](training.md).

<a id="c5"></a>

## C5 — Teacher: Qwen3-32B under vLLM, on the train instance

The same teacher, served the same way, as FuguTTX specifies. The endpoint binds
to localhost. The driver reaches the endpoint over an SSH tunnel. The teacher
proposes a break and a repair, and a guest disposes. Details:
[training](training.md).

<a id="c6"></a>

## C6 — The rollout rule: every observation comes from a guest

A teacher-written parser message must never enter the corpus. The driver runs
each checker in a real OpenBSD guest, and it records the exact output. A
fabricated observation teaches the model to expect a system that does not exist.
This rule is the reason the pilot exists. Details: [training](training.md).

<a id="c7"></a>

## C7 — Corpus lanes: two lanes, and the lane rule is absolute

The training lane holds permissively licensed configurations and synthetic
breaks. The eval lane holds real operator mistakes from public archives. The
eval lane carries author copyright. The project must not redistribute it, and it
must never enter training. Details: [corpus](corpus.md).

<a id="c8"></a>

## C8 — The model proposes edits, and the harness makes the diff

The model emits a structured edit list, under a grammar. The harness anchors
each edit, applies it, and renders the unified diff. The model must not emit a
line number, a hunk header, or a byte offset. Details: [engine](engine.md).

<a id="c9"></a>

## C9 — The tool must not change a system

`ctx` reads a file and writes a diff to standard output. It must not write a
configuration, reload a daemon, or call a privileged command. It runs under
`pledge("stdio rpath")`, with unveil restricted to the model and the input.
Details: [engine](engine.md).

<a id="c10"></a>

## C10 — Three gates grade a repair

A parser verdict is not a correctness verdict. A repair passes only through
three gates, in order. The parser accepts it, the effective configuration equals
the reference, and the daemon starts and answers. Details:
[evaluation](evaluation.md).

<a id="c11"></a>

## C11 — Infrastructure: the shared infrastructure instructions, applied

Same stacks, same layout, same state rules, same credential split, and the same
watchdog, from the `infra` pack of FuguBSD/Tooling. The tag prefix is `ctx:`.
The project gets its own Scaleway Project in the same Organization. The budget
is EUR 300 per month. Details: [infrastructure](infrastructure.md).

<a id="c12"></a>

## C12 — The learning register is a deliverable

Every campaign ends with a register entry that maps its outcome to FuguTTX spec
units. A learning that contradicts the FuguTTX spec must become a FuguTTX spec
change, not a note. Details: [register](register.md).
