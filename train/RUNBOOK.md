# Campaign runbook

This runbook holds a map, not a design. It maps each shared stage name of the
workspace observer set to the verb of this project. It names the stages this
project omits, and it names the file that holds each answer.

Three files own the content. This runbook points at them, and it does not
restate them.

| File                          | Owns                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| `train/config.env`            | The campaign pins and the shared names.                      |
| `infra/persistent/RUNBOOK.md` | The offer probe, the manual platform steps, the human steps. |
| `spec/`                       | Every design fact, the promote rule, the threshold policy.   |

## The state of this project

This project holds no campaign code today. It has no `train/config.env`, no
`infra/persistent/` stack, and no campaign workflow. Every implementable unit in
`spec/STATUS.md` is `open`.

So a dry run is the only test that this project can supply now. An observer that
reaches a stage below must stop, and report the stage as absent.

## The stage map

| Stage      | Verb of this project              | State                             |
| ---------- | --------------------------------- | --------------------------------- |
| `infra`    | the twelve `make infra-*` targets | absent, no stack exists           |
| `corpus`   | not named yet                     | absent, see `spec/corpus.md`      |
| `train`    | the SFT pass                      | absent, see `spec/training.md`    |
| `evaluate` | not named yet                     | absent, see `spec/evaluation.md`  |
| `promote`  | —                                 | omitted, this project has no step |
| `teardown` | the twelve `make infra-*` targets | absent, no stack exists           |

## What this project omits

- **The CPT pass.** Decision C4 states it: the pilot runs no CPT pass, because
  the base model already reads configuration syntax. So the `train` stage
  carries the SFT pass only.
- **The promote step.** This project defines none.

## The answers

- **The method.** Decision C4 of `spec/DECISIONS.md`, and `spec/training.md`.
- **The corpus.** `spec/corpus.md`. The corpus holds repair pairs, and each pair
  carries a verdict from a real parser.
- **The threshold policy.** `spec/evaluation.md`. A baseline fixes the
  thresholds.

## The rules that the observer set adds

- Export this project's `.env` before any command that reaches Scaleway. The
  `env` block of the workspace checkout shadows every project key (Workspace
  D-05).
- State the clone and the git HEAD that each step read.
- A campaign observation goes to the learning library at capture time. The
  ledger in `spec/LEARNING.md` receives one batch for each campaign.
