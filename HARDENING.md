<!-- markdownlint-disable -->

# Hardening Report: losisin--helm-docs-github-action/v1.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **losisin--helm-docs-github-action/v1.8.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

check-dist.yaml: actions/checkout@v6, actions/setup-node@v6, actions/upload-artifact@v7
ci.yaml: actions/checkout@v6, actions/setup-node@v6, codecov/codecov-action@v5, actions/checkout@v6
codeql-analysis.yaml: actions/checkout@v6, github/codeql-action/init@v4, github/codeql-action/autobuild@v4, github/codeql-action/analyze@v4

Locations:

- `.github/workflows/check-dist.yaml:22`
- `.github/workflows/check-dist.yaml:26`
- `.github/workflows/check-dist.yaml:52`
- `.github/workflows/ci.yaml:19`
- `.github/workflows/ci.yaml:24`
- `.github/workflows/ci.yaml:40`
- `.github/workflows/ci.yaml:49`
- `.github/workflows/codeql-analysis.yaml:28`
- `.github/workflows/codeql-analysis.yaml:33`
- `.github/workflows/codeql-analysis.yaml:39`
- `.github/workflows/codeql-analysis.yaml:44`

### script-injection (severity: high)

Sub-rule (a) violation: A GitHub Actions expression is directly interpolated inside a run: shell command string. In ci.yaml, the step 'Print Output' contains: `run: echo "${{ steps.test-action.outputs.helm-docs-path }}"`

The `steps.*.outputs.*` context is workflow-controllable and flows through YAML template substitution before the shell processes it, allowing potential injection of shell metacharacters. The value should be passed via an env: variable and then referenced as a quoted shell variable instead.

Locations:

- `.github/workflows/ci.yaml:55`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all three workflow files:

1. check-dist.yaml: Pinned actions/checkout@v6, actions/setup-node@v6, and actions/upload-artifact@v7 to their full SHA hashes.

2. ci.yaml: Pinned actions/checkout@v6, actions/setup-node@v6, and codecov/codecov-action@v5 to their full SHA hashes. Also fixed script injection in the 'Print Output' step by moving `${{ steps.test-action.outputs.helm-docs-path }}` into an env: block as HELM_DOCS_PATH and referencing it as `$HELM_DOCS_PATH` in the shell command.

3. codeql-analysis.yaml: Pinned actions/checkout@v6, github/codeql-action/init@v4, github/codeql-action/autobuild@v4, and github/codeql-action/analyze@v4 to their full SHA hashes.

