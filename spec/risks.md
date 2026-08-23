# Risks

This document names each risk of the project and its mitigation. The units are
citation-only: a plan can cite a risk, and no code implements one.

<a id="rsk-gate"></a>

## A weak gate

A parser verdict grades syntax only. [The harm rate](evaluation.md#evl-harm)
exists to expose the gap, and [gate B](evaluation.md#evl-gates) needs an
effective-configuration dump. Five configurations of
[the checker table](engine.md#eng-checkers) have no dump, so the behavior gate
carries the risk.

<a id="rsk-realism"></a>

## Mutation realism

A mechanical break teaches a mechanical repair.
[The teacher set](training.md#trn-mut) and [the eval lane](corpus.md#cor-eval)
test that gap. [The register](register.md#reg-deliver) reports the score
difference between the two sets.

<a id="rsk-cap"></a>

## Capacity

A 1.7B model can propose a plausible and wrong edit. The refusal path of
[ENG-ANCHOR](engine.md#eng-anchor) keeps a bad proposal out of the diff.

<a id="rsk-thru"></a>

## Guest throughput

A slow rollout raises the teacher bill. [The driver](training.md#trn-roll) must
run guests in parallel. A measurement on
[the dev host](infrastructure.md#iac-devhost) fixes the count.

<a id="rsk-scope"></a>

## Scope leak

G2 pulls the project toward FuguTTX features. The test: FuguCTX builds nothing
that neither G1 nor a [register row](register.md#reg-map) can name.

<a id="rsk-seq"></a>

## Sequencing

The pilot delays FuguTTX by its own duration. The mitigation is scale: the model
is small, so each pass completes in hours.
