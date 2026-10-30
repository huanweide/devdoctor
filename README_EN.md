**中文** · [README.md](./README.md)

<!-- badges -->
[![Node](https://img.shields.io/badge/Node.js-≥14-339933?logo=node.js&logoColor=white)](https://nodejs.org)
[![Zero-Deps](https://img.shields.io/badge/dependencies-zero-0f9d58?logo=npm&logoColor=white)](https://www.npmjs.com/)
[![License](https://img.shields.io/github/license/huanweide/devdoctor)](LICENSE)
<!-- /badges -->

# devdoctor

> A zero-dependency, single-file Node CLI · the four-dimensional project check-up center

It folds the scattered "project health check-up" across several tools into one yardstick: **dependency bloat, license compliance, dependency cycles, and plaintext secrets** — all in one command, fully local, offline, zero-dependency, single-file, cross-platform, ready to drop into a CI gate.

## Who should use it / Who shouldn't

- **Use it if** you're a JS/TS developer with `node_modules` / `.env`; you want one yardstick for bloat / license / cycles / secrets; or you want a four-dimensional gate in CI.
- **Skip it if** you already use snyk / license-checker / depcheck separately and are happy with the split tools (it still works as a complement); or you only want single-point checks and no combined entry point.

## Why devdoctor

- **Composition, not a new metric**: safeguard / idiot-index / recall-cli / pagext / chaineye / licguard are all single-point zero-dependency tools. devdoctor gathers their "check-up" capabilities (bloat from idiot-index's idea, license from licguard, cycles from chaineye) plus a new secrets scan into one entry point — **no need to install four tools separately; one yardstick measures all**.
- **Better than, or absent from, the market**: snyk / depcheck / license-checker are separate and mostly need `npm install`. devdoctor is zero-dependency, single-file, runs offline, and unifies four dimensions.
- **Wide audience**: every JS/TS developer has `node_modules` and `.env`; the four-dimensional check-up applies to everyone.

## Install / Run

Zero dependency, no `npm install`:

```bash
# Run directly
node index.js doctor

# Or link globally as a command
npm link
devdoctor doctor
```

Requires Node ≥ 14.

## Subcommands

| Command | Purpose | Key gate |
|---|---|---|
| `devdoctor bloat [dir]` | Dependency bloat: node_modules size, largest packages, native-replacement candidates | `--max-mb N` `--max-redundant N` |
| `devdoctor license [dir]` | License compliance: SPDX classification (permissive / weak-copyleft / strong-copyleft / unknown) | `--fail-on unknown\|weak\|strong` `--max N` |
| `devdoctor cycles [dir]` | Dependency cycles: build a DAG from JS/TS relative imports, iterative DFS to avoid stack overflow | `--fail-on-cycle` `--max-cycles N` |
| `devdoctor secrets [dir]` | Plaintext secrets: AWS/private keys/Token/JWT etc. in `.env` and source | `--fail-on-secret` `--max N` |
| `devdoctor doctor [dir]` | One-shot full check + composite health score | — |
| `devdoctor help` | Help | — |

`dir` defaults to the current directory.

## Examples

```bash
# Full check on the current project
node index.js doctor

# In CI: no strong-copyleft license, no plaintext secret, no dependency cycle
node index.js license --fail-on strong
node index.js secrets --fail-on-secret
node index.js cycles  --fail-on-cycle

# Cap node_modules size
node index.js bloat --max-mb 200
```

## Design iron laws (quality baseline)

- **Local, zero-dependency, offline, single-file**: only Node built-ins `fs` / `path`, no npm packages.
- **Gate guardrails**: every numeric threshold is validated with `Number.isFinite`; non-finite values exit 2 immediately, never silently passing (avoids `count > NaN` being always false and voiding the gate).
- **Path pre-check**: root must `statSync` to an existing directory; wrong paths never falsely report "pass".
- **Robust parsing**: bad JSON is silently skipped, not crashed; large files (>5MB) are skipped to avoid OOM; LGPL is matched before GPL to avoid mis-classifying weak-copyleft as strong.
- **Cycle detection**: explicit-stack iterative DFS (three-color marking) — no stack overflow on large repos.

## Relation to family tools

devdoctor is the "check-up integration" edition of the existing zero-dependency family. It doesn't replace non-check-up tools like recall-cli (terminal completion) / pagext (web extraction); it converges the check-up capabilities into one entry point.

## Roadmap

Rule set is stable. Possible future additions: a composite health-score weighting config and a JSON report aggregation mode. No breaking changes planned.

## Contributing

Issues and pull requests are welcome. Keep it single-file and zero-dependency.

## ⭐ Star

If devdoctor gave you one yardstick for project health, a Star ⭐ helps others find it: https://github.com/huanweide/devdoctor

## Author

[huanweide](https://github.com/huanweide)

## License

MIT
