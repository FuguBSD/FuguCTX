# Corpus

<a id="cor-buckets"></a>

## The buckets

Four Object Storage buckets hold the corpus, the checkpoints, and the artifacts.
Rehearses: FuguTTX IAC-PERSIST, FuguTTX D6.

- **COR-BUCKETS-1** — The four buckets must mirror the FuguTTX bucket set:
  `ctx-corpus`, `ctx-evalcorpus`, `ctx-checkpoints`, and `ctx-artifacts`.
- **COR-BUCKETS-2** — Each bucket must apply the versioning and lifecycle rules
  of the FuguTTX bucket set.

<a id="cor-train"></a>

## The training lane

The lane holds valid configurations and verified breaks. The valid
configurations come from three permissive sources. The first source is the
samples in `/etc/examples/` of the OpenBSD base system. The second source is the
configurations of the FuguVM and FuguOracle deployments. The third source is
configurations that the project writes.

- **COR-TRAIN-1** — A break must enter the lane through
  [the mutation pipeline](training.md#trn-mut).
- **COR-TRAIN-2** — Each record must hold the broken text, the real
  [checker](engine.md#eng-checkers) output, the reference repair, and a
  provenance tag.

<a id="cor-eval"></a>

## The eval lane

The lane holds real broken configurations from public mailing-list archives and
forums. The material carries author copyright, so three rules apply (decision
[C7](DECISIONS.md#c7)). This lane makes the pilot faithful. FuguTTX splits its
lanes for the same copyright reason, and no synthetic corpus can rehearse that
rule.

- **COR-EVAL-1** — The project must not redistribute the eval lane.
- **COR-EVAL-2** — The eval lane must not enter training.
- **COR-EVAL-3** — The bucket policy of `ctx-evalcorpus` must name the eval
  principals only.
