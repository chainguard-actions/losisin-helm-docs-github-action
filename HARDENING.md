<!-- markdownlint-disable -->

# Hardening Report: losisin--helm-docs-github-action/v2.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **losisin--helm-docs-github-action/v2.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (e.g. @v7, @v4) instead of immutable full 40-character SHA digests. This exposes the workflow to supply-chain attacks if the tag is moved to a different commit.

ci.yaml: actions/checkout@v7 (line 20), actions/setup-node@v7 (line 24), codecov/codecov-action@v7 (line 42), actions/checkout@v7 (line 55).
check-dist.yaml: actions/checkout@v7 (line 26), actions/setup-node@v7 (line 29), actions/upload-artifact@v7 (line 54).
codeql-analysis.yaml: actions/checkout@v7 (line 28), github/codeql-action/init@v4 (line 32), github/codeql-action/autobuild@v4 (line 40), github/codeql-action/analyze@v4 (line 44).

Locations:

- `.github/workflows/ci.yaml:20`
- `.github/workflows/ci.yaml:24`
- `.github/workflows/ci.yaml:42`
- `.github/workflows/ci.yaml:55`
- `.github/workflows/check-dist.yaml:26`
- `.github/workflows/check-dist.yaml:29`
- `.github/workflows/check-dist.yaml:54`
- `.github/workflows/codeql-analysis.yaml:28`
- `.github/workflows/codeql-analysis.yaml:32`
- `.github/workflows/codeql-analysis.yaml:40`
- `.github/workflows/codeql-analysis.yaml:44`

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is interpolated directly inside a run: shell command string. In ci.yaml the step 'Print Output' runs: `echo "${{ steps.test-action.outputs.helm-docs-path }}"`. The expression value is substituted into the shell command before the shell parses it, allowing an attacker who can influence the action output to inject arbitrary shell commands. The value should be passed via an env: variable and then referenced as a quoted shell variable (e.g. `echo "$HELM_DOCS_PATH"`).

Locations:

- `.github/workflows/ci.yaml:65`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all 11 unpinned action references across 3 workflow files by replacing mutable version tags with full 40-character SHA digests (preserving tags as comments): actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1, actions/setup-node@v7 → @820762786026740c76f36085b0efc47a31fe5020, codecov/codecov-action@v7 → @fb8b3582c8e4def4969c97caa2f19720cb33a72f, actions/upload-artifact@v7 → @043fb46d1a93c77aae656e7c1c64a875d1fc6a0a, github/codeql-action/{init,autobuild,analyze}@v4 → @cdf488f595d80d6e07e03d4674febd5ab45fa938. Also fixed the script injection in ci.yaml's 'Print Output' step by moving `${{ steps.test-action.outputs.helm-docs-path }}` into an env: block as HELM_DOCS_PATH and referencing it as `$HELM_DOCS_PATH` in the shell command.

