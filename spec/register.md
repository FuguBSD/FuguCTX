# The learning register

The learning register records what each campaign teaches about the FuguTTX
specification. This document defines the register as a deliverable, the planned
rehearsals, and the scope of a claim.

<a id="reg-deliver"></a>

## The register is a deliverable

The register is the deliverable of G2 ([C12](DECISIONS.md#c12)).

- **REG-DELIVER-1** — Every campaign must end with a register entry. The entry
  maps the outcome of the campaign to FuguTTX specification units.
- **REG-DELIVER-2** — A finding must land in the FuguTTX `docs/research/`
  directory.
- **REG-DELIVER-3** — A finding that contradicts the FuguTTX specification must
  become a FuguTTX specification change, not a note.

<a id="reg-map"></a>

## The planned rehearsals

Each row of the table is a planned rehearsal, with one row per pilot component.
Each campaign appends its findings to the register.

| Pilot component                               | FuguTTX units rehearsed                                   |
| --------------------------------------------- | --------------------------------------------------------- |
| Teacher proposes, guest disposes              | FuguTTX TRN-TRACES, FuguTTX D4                            |
| Snapshot restore between traces, at volume    | FuguTTX TRN-TRACES, FuguTTX IAC-DEV                       |
| Real observations only, and the drift rule    | FuguTTX TRN-TRACES                                        |
| Recovery examples in the corpus               | FuguTTX TRN-TRACES                                        |
| Rollout rate against teacher GPU cost         | FuguTTX TRN-TRACES, FuguTTX TRN-BUDGET                    |
| The doas boundary in a disposable guest       | the FuguTTX harness specification, FuguTTX D7             |
| Parallel scenario memory and thread footprint | FuguTTX IAC-DEV, and the FuguTTX evaluation specification |
| A weak gate and the harm rate it hides        | the FuguTTX evaluation specification, FuguTTX D5          |
| The copyright lane and its bucket policy      | FuguTTX IAC-PERSIST, FuguTTX D6                           |
| Qwen3-32B under vLLM, and the SSH tunnel      | FuguTTX TRN-AUG                                           |
| Axolotl in Docker on the GPU OS image         | FuguTTX TRN-EXEC, FuguTTX D3                              |
| SFT pass end to end                           | FuguTTX TRN-SFT, FuguTTX D4                               |
| H100 quota request and grant time             | FuguTTX IAC-TRAIN, FuguTTX IAC-PREREQ                     |
| Live price read before apply                  | FuguTTX IAC-PREREQ, FuguTTX IAC-SPEND                     |
| State backend, native lock, encryption        | FuguTTX IAC-STATE                                         |
| Three-application credential split            | FuguTTX IAC-CRED, FuguTTX D9                              |
| Train key over SSH, expiry backstop           | FuguTTX IAC-TRAINCRED                                     |
| Watchdog, heartbeat, claim protocol           | FuguTTX IAC-SPEND                                         |
| Train stack up/down, teardown completeness    | FuguTTX IAC-TRAIN, FuguTTX IAC-TEARDOWN                   |
| Guest image build with fuguvm and autoinstall | FuguTTX IAC-IMAGE                                         |
| llama.cpp on OpenBSD, CPU only, determinism   | FuguTTX D2, and the FuguTTX inference specification       |

<a id="reg-scope"></a>

## The scope of a claim

The register must scope every claim, because these FuguTTX risks stay open after
the pilot:

- Training dynamics at 4B: multi-hour epochs, checkpoint sizes that stress the
  bucket rules, catastrophic forgetting, and replay mixes.
- A multi-step agent loop. The pilot runs one turn: a configuration in, a diff
  out. FuguTTX runs a loop with a terminal `report` step.
- Free-form tool selection. The pilot calls one checker, and the harness picks
  it from the file name.
- RAG, variants, and personas.
- Knowledge-dense continued pretraining. [C4](DECISIONS.md#c4) drops the CPT
  pass.

A 1.7B result does not predict a 4B result. The register must say what a finding
is evidence for, and what it is not.
