# plugin-example-lifecycle

The reference **deploy-substrate lifecycle** plugin
(`deploy:examplelifecycle`) — a plugin that brings its own host-side venue
lifecycle over the wire.

Beyond the deploy walk (`OpExecute`), the plugin serves the substrate-lifecycle
ops: `OpPrepareVenue` returns a self-contained venue descriptor that the host
re-materializes into a real deploy executor (a host-local shell executor here) —
the live executor never crosses the wire — plus `Start`/`Stop`/`Status`/
`PostApply`/`PostTeardown`/`Rebuild` and the generalized `OpPreresolve`.

## What it provides

| Capability | Surface |
|---|---|
| `deploy:examplelifecycle` | the deploy target — `OpExecute` plus the venue-lifecycle and preresolve ops |

The plugin is **out-of-process only** (not listed in `compiled_plugins:`): it is
the witness that a substrate plugin the host was not built with can drive a venue
lifecycle host → plugin. The channel is the one reused to externalize the pod/vm
substrate lifecycles.

## How to use it

Compose the plugin candy as a deploy substrate:

```yaml
- '@github.com/opencharly/plugin-example-lifecycle/candy/plugin-example-lifecycle:<tag>'
```

## Layout

- `candy/plugin-example-lifecycle/` — the plugin module: `plugin.go` (the
  provider + `NewProvider()`/`NewMeta()` + the lifecycle ops),
  `schema/examplelifecycle.cue` (the self-contained `#ExamplelifecycleInput`),
  `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-internals:install-plan` — the deploy lifecycle, the
  venue descriptor, and the reverse channel. This candy carries no `skill:` entity
  of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
