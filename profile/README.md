# esops-dev

**Open-source tooling for the people who keep production running.**

The `esops-dev` organisation builds tools for site reliability engineers, platform engineers, and operations teams — the people who live with the consequences of every deployment.

The org started with operational utilities for self-managed Elasticsearch and OpenSearch clusters (hence the name: **es** + **ops**). It is broadening into adjacent observability and platform tooling that shares the same audience and the same engineering principles.

## Projects

### [esops-go](https://github.com/esops-dev/esops-go) — *operate*

A `kubectl`-style operations CLI for self-managed Elasticsearch and OpenSearch clusters. Replaces the usual collection of `curl | jq` recipes with a single static Go binary covering health and allocation diagnostics, index lifecycle management, snapshot operations, reindex planning and verification, and credential-free diagnostic bundles. Named contexts with `prod` protection tiers so destructive operations require explicit acknowledgement.

### [esops-doctor](https://github.com/esops-dev/esops-doctor) — *diagnose*

A read-only diagnostic linter for the same clusters — think `kube-bench` and `kube-score`, but for ES/OS. Declarative rules written in CEL (so adding a check is a YAML change, not a Go change), built-in profiles for `prod` / `staging` / `dev` / `ci` / `cis-bench`, and structured output formats including SARIF and JUnit for direct integration into security and CI pipelines. Read-only is enforced through static import-graph analysis, not just code review.

`esops-go` and `esops-doctor` are deliberate counterparts: `esops-go` is imperative and may mutate, `esops-doctor` is declarative and never mutates. They share the same config file, so configuring one configures the other.

### promforecast — *predict* *(in development)*

A self-hosted forecasting service for Prometheus-compatible metrics. Reads a YAML config, runs `statsforecast` models against a long-term TSDB (VictoriaMetrics by default), and exposes predictions as first-class Prometheus metrics — graphable, alertable, indistinguishable from the rest of your monitoring stack. Ships as a Helm chart.

## Engineering principles

Common across all projects in the org:

- **Single static binaries or container-native deployments.** No runtime sprawl, no JVM-and-Python-and-Node coexistence.
- **Pipeline-friendly by default.** Stable exit codes, structured output (JSON / NDJSON / SARIF / JUnit / YAML), CI-aware behaviour. `stdout` is data, `stderr` is logs.
- **Conservative by design.** Destructive operations are opt-in; production contexts require explicit confirmation; diagnostic tooling is read-only and enforced as such in CI.
- **Secrets handled properly.** Indirection over inline plaintext (`${env:...}`, `${file:...}`, `${keyring:...}`), redaction in logs, no auth headers in diagnostic bundles.
- **Zero telemetry.** No phone-home, no opt-in metrics, no embedded SDKs. The only network traffic is to the systems you point the tools at.
- **Honest about scope.** Each project documents its non-goals. No AI features for their own sake, no scope creep into adjacent product categories.
- **Reproducible releases.** Signed artefacts (cosign), SBOMs (CycloneDX & SPDX), SLSA provenance, deterministic builds where the toolchain allows.

## Conventions

- **License**: Apache-2.0 across the organisation.
- **Versioning**: SemVer. Pre-`1.0` projects may have unstable interfaces — see each project's README for status.
- **Security**: every repo has a `SECURITY.md`. Vulnerabilities should be reported privately, not via public issues.
- **Contributing**: every repo has a `CONTRIBUTING.md` covering local setup, test conventions, and PR expectations. Open an issue before significant changes.

## Maintainer

[Werner Dijkerman](https://www.werner-dijkerman.nl); freelance cloud infrastructure and platform engineer, based in the Netherlands.
