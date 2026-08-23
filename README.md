# FuguCTX

A configuration repair model for OpenBSD daemons, built as the pilot of FuguTTX.

FuguCTX reads a broken daemon configuration and a parser error, and it proposes
the smallest fix as a unified diff. The operator reads the diff and decides. The
engine is a Qwen3-1.7B fine-tune under llama.cpp, on the CPU only. The `ctx`
tool writes a diff to standard output, and it must not change a system.

The project is also the pilot of FuguTTX. The build rehearses the FuguTTX
production pipeline at small scale, on the same components, at real prices.
OpenBSD supplies a deterministic judge for every configuration: the parser of
the daemon. The project can therefore run the full FuguTTX trace loop, where a
teacher proposes and a real guest disposes.

The project is specification-first: the code follows the specification.

## Documentation

The specification in [spec/](spec/index.md) is the authoritative reference. Read
[spec/DECISIONS.md](spec/DECISIONS.md) before you make a plan.

## Commands

```sh
make deps        # install the Scaleway CLI
make check       # spec-check + ste-lint + test; run it before each commit
make prettier    # Markdown, JSON and YAML formatting check
make help        # list the targets
```

## Commit scopes

`spec`, `docs`, `engine`, `corpus`, `train`, `eval`, `infra`, `ci`.

## License

ISC. See [LICENSE](LICENSE).
