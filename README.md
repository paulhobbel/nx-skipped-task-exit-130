# Nx exits 130 when a task is skipped because its dependency failed

Minimal reproduction: two `nx:run-commands` targets, no plugins.

- `proj:upstream` runs `exit 1`.
- `proj:downstream` depends on `upstream`.

## Steps

```sh
npm ci
NX_DAEMON=false NX_SKIP_NX_CACHE=true npx nx run proj:downstream; echo "exit $?"
```

**Expected:** exit 1. `upstream` failed and `downstream` was skipped because of it.

**Actual:** exit 130 (SIGINT), as if the run had been interrupted. Running `npx nx run proj:upstream` alone exits 1.

Reproduced on `nx` 23.2.1, 23.3.0 and `24.0.0-canary.20261019-c4bb923`.

The `repro` workflow runs the same command in GitHub Actions and fails while Nx exits 130.
