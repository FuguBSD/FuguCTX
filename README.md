# FuguCTX

A configuration repair model for OpenBSD daemons, built as the pilot of FuguTTX.

FuguCTX reads a broken daemon configuration and a parser error, and it proposes
the smallest fix as a unified diff. The operator reads the diff and decides.

The engine is a Qwen3-1.7B fine-tune under llama.cpp, on the CPU only. The `ctx`
tool writes a diff to standard output, and it must not change a system.

The build rehearses the FuguTTX production pipeline at small scale, on the same
components, at real prices. OpenBSD supplies a deterministic judge for every
configuration: the parser of the daemon.

## Documentation

The project is specification-first: the specification in [spec/](spec/index.md)
is the authoritative reference.

## Commands

```sh
make deps        # install the Scaleway CLI
make check       # spec-check + ste-lint + test; run it before each commit
make format-md   # Markdown, JSON and YAML formatting check
```

## Commit scopes

`spec`, `docs`, `engine`, `corpus`, `train`, `eval`, `infra`, `ci`.

## License

ISC. See [LICENSE](LICENSE).
