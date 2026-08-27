# Training

<a id="trn-inst"></a>

## Instances

Two Scaleway instance types serve the training work. Rehearses: FuguTTX
IAC-TRAIN, FuguTTX TRN-INST.

| Instance   | GPU          | VRAM  | EUR/hour | Role                       |
| ---------- | ------------ | ----- | -------- | -------------------------- |
| H100-1-80G | 1× H100 PCIe | 80 GB | 2.73     | Default; hosts the teacher |
| L40S-1-48G | 1× L40S      | 48 GB | 1.47     | Budget runs, no teacher    |

Each price in this table is unverified, per the FuguTTX rule. Scaleway grants no
H100 quota by default. The quota request is a rehearsal.

- **TRN-INST-1** — The pipeline must read the live price before it creates a
  resource.
- **TRN-INST-2** — [LEARNING](LEARNING.md#lrn-deliver) must record the response
  time of the quota request.

<a id="trn-mut"></a>

## The mutation pipeline

The pipeline makes a repair pair from a valid configuration, in four steps. Two
sources supply mutations. A mechanical set deletes a token, swaps two clauses,
misspells a keyword, or unbalances a brace. The teacher supplies the realistic
set, which imitates an operator mistake. Rehearses: FuguTTX TRN-AUG, FuguTTX
TRN-TRACES.

- **TRN-MUT-1** — A mutation must break the configuration.
- **TRN-MUT-2** — The driver must write the broken text into a guest, and it
  must run the [checker](engine.md#eng-checkers). The pair must enter the corpus
  only when the checker rejects the broken text. A mutation that still parses is
  a semantic break. The pipeline must route it to
  [the behavior gate](evaluation.md#evl-gates).
- **TRN-MUT-3** — The driver must run the checker on the original text, and the
  output must show acceptance. This step proves the reference repair.
- **TRN-MUT-4** — The driver must record the broken text, the real checker
  output, and the reference diff.

<a id="trn-roll"></a>

## The rollout rules

Decision [C6](DECISIONS.md#c6) governs each rollout: every observation comes
from a guest. A teacher-written parser message must not enter the corpus. A
fabricated observation teaches the model to expect a system that does not exist.

- **TRN-ROLL-1** — The driver must restore the guest from the base snapshot
  between two pairs. It uses `fuguvm snapshot restore`.
- **TRN-ROLL-2** — The driver must copy each checker output from the guest
  verbatim. It must not edit, shorten, or rewrite the output.
- **TRN-ROLL-3** — The prompt text, [the edit schema](engine.md#eng-schema), and
  the error templates are shared artifacts. The repository defines each one
  once. The driver and [the harness](engine.md#eng-split) must read them from
  the same source. So the training format and the run-time format cannot drift.
- **TRN-ROLL-4** — The corpus must hold recovery examples. A recovery example
  holds a bad proposal, the refusal of [ENG-ANCHOR](engine.md#eng-anchor), and
  the corrected proposal.

<a id="trn-priv"></a>

## The privilege rehearsal

The behavior gate installs a configuration in `/etc` of the guest, and it
restarts the daemon. This rehearses the doas boundary of the FuguTTX harness at
no risk. Rehearses: the FuguTTX harness specification, FuguTTX D7.

- **TRN-PRIV-1** — The guest must hold one `doas` rule that permits the checker
  and `rcctl` only.
- **TRN-PRIV-2** — The guest is disposable, and the driver must restore it after
  each run.

<a id="trn-teach"></a>

## The teacher

The teacher proposes a break and a repair, and a guest disposes (decision
[C5](DECISIONS.md#c5)). Rehearses: FuguTTX TRN-AUG.

- **TRN-TEACH-1** — vLLM must serve Qwen3-32B on the train instance.
- **TRN-TEACH-2** — The endpoint must bind to localhost.
- **TRN-TEACH-3** — The driver must reach the endpoint over an SSH tunnel.

<a id="trn-exec"></a>

## Execution

A run is `make train-sft` against a provisioned instance. Rehearses: FuguTTX
TRN-EXEC, FuguTTX IAC-DURA.

- **TRN-EXEC-1** — Training must run in the published Axolotl CUDA Docker image.
- **TRN-EXEC-2** — Every training configuration must live in the repository.
- **TRN-EXEC-3** — Checkpoints must synchronize to Object Storage after each
  epoch, so a destroy loses minutes at this scale.

<a id="trn-budget"></a>

## The compute budget

The estimates are order-of-magnitude, at the unverified H100 price of EUR 2.73
per hour. Scaleway documents a minimum of 60 minutes per created resource, so no
run is cheaper than one hour.

| Item                                                        | GPU-hours | EUR per run |
| ----------------------------------------------------------- | --------- | ----------- |
| SFT pass (1.7B, QLoRA)                                      | 2–4       | 6–11        |
| Mutation campaign (Qwen3-32B, vLLM)                         | 5–15      | 14–41       |
| Rollout campaign (teacher held open)                        | 4–12      | 11–33       |
| Guest [tiers T1 and T2](evaluation.md#evl-tiers) (dev host) | —         | 2–6         |

An active month costs approximately EUR 60–200.
[The cap](infrastructure.md#iac-apply) is EUR 300 per month, and only a human
raises it. The FuguTTX specification sets one campaign month at EUR 300–800.

The rollout campaign is the line to watch. The teacher bills while the guests
work, so a slow guest raises the GPU bill.

- **TRN-BUDGET-1** — LEARNING must report the achieved rollouts per GPU-hour.
