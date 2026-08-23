# The repair engine

The engine is the `ctx` tool: the model under llama.cpp, and the harness around
it. This document specifies the division of labor, the edit schema, the checker
table, and the interfaces.

<a id="eng-split"></a>

## Division of labor

The harness reads the configuration, runs the checker, and holds the parser
error. The model proposes edits, and the harness makes the diff (decision
[C8](DECISIONS.md#c8)). This division keeps every offset out of the model. It
mirrors the FuguTTX rule that the harness owns each schema. Rehearses: the
FuguTTX harness patterns.

- **ENG-SPLIT-1** — The harness must anchor each edit, apply it, and render the
  unified diff.
- **ENG-SPLIT-2** — The model must not emit a line number, a hunk header, or a
  byte offset.

<a id="eng-schema"></a>

## The edit schema

The model emits a structured edit list, under a grammar.

- **ENG-SCHEMA-1** — A llama.cpp GBNF grammar must constrain the model output to
  the edit schema. Each edit holds an anchor string, a replacement string, and a
  reason.

<a id="eng-anchor"></a>

## Anchor discipline

An edit binds to the input through its anchor string alone. An anchor that is
absent, or that matches more than one place, gives no unique position.

- **ENG-ANCHOR-1** — The harness must refuse an edit whose anchor is absent from
  the input. It must also refuse an anchor that matches more than one place. The
  harness reports the refusal with the exit code 3.

<a id="eng-determ"></a>

## Determinism

Determinism holds under the pins of [C2](DECISIONS.md#c2).

- **ENG-DETERM-1** — Each proposal must carry the model hash and the checker
  output that started the repair. Same bytes in give the same diff out, for one
  model hash and one engine version.

<a id="eng-safe"></a>

## The tool must not change a system

`ctx` reads a file, and it writes a diff to standard output. Decision
[C9](DECISIONS.md#c9) sets this boundary.

- **ENG-SAFE-1** — `ctx` must not write a configuration, reload a daemon, or
  call a privileged command.
- **ENG-SAFE-2** — `ctx` must run under `pledge("stdio rpath")`, with unveil
  restricted to the model and the input.

<a id="eng-checkers"></a>

## The checker table

One table maps each configuration to its check command.

| Configuration                   | Check command       |
| ------------------------------- | ------------------- |
| `/etc/pf.conf`                  | `pfctl -nf`         |
| `/etc/ssh/sshd_config`          | `sshd -t -f`        |
| `/etc/httpd.conf`               | `httpd -n -f`       |
| `/etc/smtpd.conf`               | `smtpd -n -f`       |
| `/etc/relayd.conf`              | `relayd -n -f`      |
| `/var/unbound/etc/unbound.conf` | `unbound-checkconf` |
| `/etc/doas.conf`                | `doas -C`           |

`/etc/pf.conf` and `/etc/ssh/sshd_config` also supply an effective-configuration
dump. `pfctl -nvf` prints the expanded ruleset, and `sshd -T` prints the
effective settings. Three gates grade a repair ([C10](DECISIONS.md#c10)). Gate
B, the equivalence gate of [evaluation](evaluation.md#evl-gates), needs such a
dump. The other five configurations therefore depend on gate C, the behavior
gate.

- **ENG-CHECKERS-1** — The repository must hold one table that maps a
  configuration to its check command.
- **ENG-CHECKERS-2** — A guest must verify each command on the target release,
  and the [learning register](register.md#reg-deliver) must record the result.

<a id="eng-iface"></a>

## Interfaces

The tool has two subcommands: `ctx repair` and `ctx explain`.

- **ENG-IFACE-1** — `ctx repair` must read a configuration and a checker output,
  and must emit a unified diff or JSON.
- **ENG-IFACE-2** — `ctx explain` must print the reason of each edit, in one
  line per edit.
- **ENG-IFACE-3** — The tool must read a checker output on standard input, so a
  shell pipeline can supply it.
