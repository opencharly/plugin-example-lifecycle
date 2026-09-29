# AGENTS.md — plugin-example-lifecycle

Standalone plugin repo for the `examplelifecycle` capability
(`deploy:examplelifecycle`) — the reference out-of-tree deploy-substrate
lifecycle plugin (F6). The plugin is a Go module at
`candy/plugin-example-lifecycle/` (module path
`github.com/opencharly/plugin-example-lifecycle/candy/plugin-example-lifecycle`);
the root `charly.yml` only declares `discover: candy` so the repo is a project
and its candy is scanned.

Canonical files:

- `candy/plugin-example-lifecycle/charly.yml` — the `plugin-example-lifecycle:`
  candy entity (`plugin:` block, `plan:` check).
- `candy/plugin-example-lifecycle/plugin.go` — the provider (`NewProvider()` +
  `NewMeta()`) and the `OpPrepareVenue` / lifecycle / `OpPreresolve` dispatch.
- `candy/plugin-example-lifecycle/schema/examplelifecycle.cue` — the
  self-contained `#ExamplelifecycleInput`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model (incl. the `deploy` class), the per-plugin
  CUE-schema contract, placement. Load before touching the provider or schema.
- `/charly-internals:install-plan` — the deploy lifecycle, the venue descriptor,
  and the reverse channel the lifecycle ops ride.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-example-lifecycle/` — compile the plugin
  module.
- `go test ./...` in `candy/plugin-example-lifecycle/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.

## Modify this repo

- Edit the `plugin-example-lifecycle:` candy entity, the Go source, and
  `schema/examplelifecycle.cue` **together**.
- The plugin is **out-of-process only** (deliberately not in
  `compiled_plugins:`); do not add it to the compiled set — it exists to witness
  the host → plugin lifecycle channel.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
