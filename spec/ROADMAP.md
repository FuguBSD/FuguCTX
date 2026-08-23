# Roadmap

Work proceeds in phases. Each phase ends with a measurement and an entry in the
[learning register](register.md#reg-deliver), which is the FuguTTX D10 pattern.
No phase starts cloud spend before P2. Every phase is days, not weeks, because
the model is small. This scale mitigates the
[sequencing risk](risks.md#rsk-seq).

| Phase | Work                                                                                                                                                                                                                     |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| P0    | The repository, this specification as `spec/`, and the org sync pack.                                                                                                                                                    |
| P1    | The [harness](engine.md#eng-split), the [edit schema](engine.md#eng-schema), the [checker table](engine.md#eng-checkers), and the [tier T0](evaluation.md#evl-tiers) score script. Local only: no cloud resource exists. |
| P2    | The [persistent stack](infrastructure.md#iac-apply), the credential split, the state backend, and the [KVM test](infrastructure.md#iac-devhost). First register entries.                                                 |
| P3    | The [image stack](infrastructure.md#iac-image) and the dev host. The [mechanical mutation pipeline](training.md#trn-mut) for `pf.conf` and `sshd_config`, which both supply a dump.                                      |
| P4    | The first [SFT campaign](training.md#trn-exec) on the H100. The baseline [scorecard](evaluation.md#evl-scores) fixes the promotion thresholds.                                                                           |
| P5    | The [teacher](training.md#trn-teach), the realistic mutation set, and the [rollout campaign](training.md#trn-roll).                                                                                                      |
| P6    | [Gate C](evaluation.md#evl-gates), and the five configurations that depend on it.                                                                                                                                        |
| P7    | The [eval lane](corpus.md#cor-eval), and the first [harm-rate](evaluation.md#evl-harm) report against real operator mistakes.                                                                                            |
