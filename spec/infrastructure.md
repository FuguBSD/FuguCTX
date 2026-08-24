# Infrastructure

The pilot applies the shared infrastructure instructions with three
substitutions. This document names the substitutions, the dev host test, and the
image stack.

<a id="iac-apply"></a>

## The applied instructions

The synced [infra/CLAUDE.md](../infra/CLAUDE.md) holds the shared rules: naming,
layout, tags, state, credentials, spend guardrails, and teardown. Three
substitutions adapt them to this pilot ([C11](DECISIONS.md#c11)). Rehearses: the
FuguTTX IAC family.

- **IAC-APPLY-1** — The synced instructions must apply as written, with only the
  three substitutions of this unit.
- **IAC-APPLY-2** — The project code must be `ctx`, and the tag prefix must be
  `ctx:`.
- **IAC-APPLY-3** — The Scaleway Project must be FuguCTX's own, in the same
  Organization.
- **IAC-APPLY-4** — The budget must be EUR 300 per month, with alerts at 50, 75,
  and 100 percent.

<a id="iac-devhost"></a>

## The dev host

This pilot needs the dev host more than a data-only pilot does.
[The rollout driver](training.md#trn-roll) and the guest tiers
[T1 and T2](evaluation.md#evl-tiers) run there. Either result of the KVM test
closes an open question in the FuguTTX specification. Rehearses: FuguTTX
IAC-METAL, FuguTTX IAC-DEV, FuguTTX D9.

- **IAC-DEVHOST-1** — Before the first dev stack apply, the project must run the
  FuguTTX KVM test. The test is one virtual instance, one hour, a check of
  `/dev/kvm`, and one OpenBSD guest under `fuguvm`.
- **IAC-DEVHOST-2** — If both checks pass, the host must be an ephemeral virtual
  instance.
- **IAC-DEVHOST-3** — If a check fails, the host must be an Elastic Metal
  server. The offer is the smallest one that runs the measured parallel target
  of [the artifact suite](evaluation.md#evl-suite).

<a id="iac-image"></a>

## The image stack

One stack builds the OpenBSD guest image. The stack is one of the four stacks of
[the applied instructions](#iac-apply).

- **IAC-IMAGE-1** — The stack must build the OpenBSD guest qcow2 with `fuguvm`
  and `autoinstall(8)`, exactly as FuguTTX IAC-IMAGE specifies.
- **IAC-IMAGE-2** — The image must carry the daemons of
  [the checker table](engine.md#eng-checkers).
- **IAC-IMAGE-3** — The image must carry the one `doas` rule of
  [the privilege rehearsal](training.md#trn-priv).
