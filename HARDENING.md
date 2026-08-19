<!-- markdownlint-disable -->

# Hardening Report: losisin--helm-docs-github-action/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **losisin--helm-docs-github-action/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tagged release is overwritten or compromised.

check-dist.yaml: actions/checkout@v6, actions/setup-node@v6, actions/upload-artifact@v7
ci.yaml: actions/checkout@v6, actions/setup-node@v6, codecov/codecov-action@v6, actions/checkout@v6 (test-action job)
codeql-analysis.yaml: actions/checkout@v6, github/codeql-action/init@v4, github/codeql-action/autobuild@v4, github/codeql-action/analyze@v4

Locations:

- `.github/workflows/check-dist.yaml:22`
- `.github/workflows/check-dist.yaml:27`
- `.github/workflows/check-dist.yaml:47`
- `.github/workflows/ci.yaml:18`
- `.github/workflows/ci.yaml:23`
- `.github/workflows/ci.yaml:40`
- `.github/workflows/ci.yaml:52`
- `.github/workflows/codeql-analysis.yaml:22`
- `.github/workflows/codeql-analysis.yaml:27`
- `.github/workflows/codeql-analysis.yaml:38`
- `.github/workflows/codeql-analysis.yaml:43`

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is interpolated directly inside a run: shell command. In ci.yaml, the step 'Print Output' runs: `echo "${{ steps.test-action.outputs.helm-docs-path }}"`. The expression `steps.test-action.outputs.helm-docs-path` is substituted into the shell command string before the shell executes it, allowing a malicious value in that output to inject arbitrary shell commands. The value should be passed via an env: variable and referenced as a quoted shell variable instead.

Locations:

- `.github/workflows/ci.yaml:58`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all three workflow files:

1. check-dist.yaml: Pinned actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38, actions/upload-artifact@v7 → @043fb46d1a93c77aae656e7c1c64a875d1fc6a0a

2. ci.yaml: Pinned actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38, codecov/codecov-action@v6 → @fb8b3582c8e4def4969c97caa2f19720cb33a72f. Fixed script-injection in 'Print Output' step by moving `${{ steps.test-action.outputs.helm-docs-path }}` into an env: block as HELM_DOCS_PATH and referencing it as `echo "$HELM_DOCS_PATH"` in the shell command.

3. codeql-analysis.yaml: Pinned actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803, github/codeql-action/init@v4 → @7188fc363630916deb702c7fdcf4e481b751f97a, github/codeql-action/autobuild@v4 → @7188fc363630916deb702c7fdcf4e481b751f97a, github/codeql-action/analyze@v4 → @7188fc363630916deb702c7fdcf4e481b751f97a

All original version tags preserved as inline comments for readability.

