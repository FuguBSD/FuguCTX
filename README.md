# FuguCTX

A configuration repair model for OpenBSD daemons. FuguCTX reads a broken daemon
configuration and a parser error, and it proposes the smallest fix as a unified
diff. The operator reads the diff and decides.

The engine is a Qwen3-1.7B fine-tune under llama.cpp, on the CPU only. The `ctx`
tool writes a diff to standard output, and it must not change a system. The
project is the pilot of [FuguTTX](https://github.com/FuguBSD/FuguTTX), and the
build rehearses its production pipeline at small scale.

## Commands

```sh
make setup       # install the development tools into .venv
make deps        # install gitleaks and the Scaleway CLI
make check       # run every gate; run it before each commit
make test        # run the test suite
make format-fix  # fix the Python, Markdown, JSON and YAML formatting
```
