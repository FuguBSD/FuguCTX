# Evaluation

Three gates grade a repair, and five measures land on the scorecard. Three tiers
run the gates, from a text check in CI to a behavior check in OpenBSD guests.
The artifact suite checks the shipped artifact.

<a id="evl-gates"></a>

## The three gates

A parser verdict is not a correctness verdict ([C10](DECISIONS.md#c10)). Three
gates therefore grade a repair. The check command and the
effective-configuration dump come from
[the checker table](engine.md#eng-checkers). Rehearses: FuguTTX TRN-TRACES, and
the FuguTTX judge filter.

| Gate | Name        | Test                                                       |
| ---- | ----------- | ---------------------------------------------------------- |
| A    | Parse       | The checker accepts the repaired configuration             |
| B    | Equivalence | The effective-configuration dump equals the reference dump |
| C    | Behavior    | The daemon starts, and a probe gets the expected answer    |

- **EVL-GATES-1** — A repair passes only through the three gates, in order: gate
  A, then gate B, then gate C.

<a id="evl-harm"></a>

## The harm rate

A repair that passes gate A and fails gate B is the dangerous case. The
configuration parses, and it does the wrong thing. The harm rate is the most
transferable measurement of the pilot. FuguTTX grades an agent action with a
scenario check, and a wrong action that passes a weak check carries the same
danger there.

- **EVL-HARM-1** — The scorecard must report the dangerous case as the harm
  rate.
- **EVL-HARM-2** — The harm rate is the headline number of the project.
- **EVL-HARM-3** — A high harm rate must block promotion.

<a id="evl-tiers"></a>

## The tiers

Evaluation promotes a model version, and a scorecard lands in
[the artifacts bucket](corpus.md#cor-buckets). This is the FuguTTX D5 pattern.
Tier T0 needs no OpenBSD binary, so it runs on a Linux CI runner. Tiers T1 and
T2 need the real parsers, so [the dev host](infrastructure.md#iac-devhost) runs
them in guests. Tier T1 runs its gates on [the eval lane](corpus.md#cor-eval).

| Tier | Where                  | What                                              |
| ---- | ---------------------- | ------------------------------------------------- |
| T0   | CI, CPU, every commit  | Text-level exact match against the reference diff |
| T1   | OpenBSD guests, FuguVM | Gates A and B on the eval lane                    |
| T2   | OpenBSD guests, FuguVM | Gate C, and the artifact suite                    |

- **EVL-TIERS-1** — The first baseline run fixes each promotion threshold. A
  threshold in this document before that run is a guess, and the specification
  must not hold a guess.

<a id="evl-scores"></a>

## The scored measures

The scorecard reports five measures. The refusal rate counts the refusals of
[ENG-ANCHOR](engine.md#eng-anchor). A high refusal rate is a good failure: the
tool says nothing, and the operator loses no time.

| Measure          | Definition                                                    |
| ---------------- | ------------------------------------------------------------- |
| Repair rate      | The share of inputs where the repair passes gate A            |
| Equivalence rate | The share of inputs where the repair passes gate B            |
| Harm rate        | The share of inputs that pass gate A and fail gate B          |
| Minimality       | The edit distance between the proposal and the reference diff |
| Refusal rate     | The share of inputs where the harness refuses every edit      |

- **EVL-SCORES-1** — The scorecard must report the five measures of this table.

<a id="evl-suite"></a>

## The artifact suite

The suite installs the shipped artifact into OpenBSD guests that FuguVM boots
from [the project image](infrastructure.md#iac-image). The artifact is `ctx`,
llama.cpp, and the model. The suite checks five things. Rehearses: FuguTTX
IAC-DEV, FuguTTX IAC-IMAGE.

- **EVL-SUITE-1** — The artifact must build and run under
  `pledge("stdio rpath")` and unveil.
- **EVL-SUITE-2** — Two guests must give byte-identical diffs for the same
  input. A difference fails [the determinism contract](engine.md#eng-determ).
- **EVL-SUITE-3** — The tool must write no file, and it must open no socket. A
  violation fails [C9](DECISIONS.md#c9).
- **EVL-SUITE-4** — Guest scores must equal host scores on a sample.
- **EVL-SUITE-5** — The suite must measure the memory and the thread count of
  one scenario. This measurement fixes the parallel-scenario target that FuguTTX
  IAC-DEV leaves open.
