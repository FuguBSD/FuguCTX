# Licensing and release

This document names the license of each component, and it sets the rules of a
release.

<a id="lic-lic"></a>

## The licenses

Three license classes cover the project. [C1](DECISIONS.md#c1) sets the base
model, and [the training lane](corpus.md#cor-train) sets the corpus sources.

- **LIC-LIC-1** — The harness and all tooling must carry the ISC license.
- **LIC-LIC-2** — The base model carries the Apache 2.0 license.
- **LIC-LIC-3** — The training lane holds BSD-licensed samples from the OpenBSD
  base system, project configurations, and synthetic breaks.

<a id="lic-release"></a>

## Release integrity

[The eval lane](corpus.md#cor-eval) carries author copyright, per
[C7](DECISIONS.md#c7). The rules below keep every release clean.

- **LIC-RELEASE-1** — The released model must carry attribution. Anyone can
  redistribute the model, commercially included.
- **LIC-RELEASE-2** — The project must not redistribute the eval lane.
- **LIC-RELEASE-3** — A released component must not carry a non-commercial
  restriction.
