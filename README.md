# ourios-semconv

The Ourios OpenTelemetry semantic-conventions registry and the weaver
templates that render it — the single source for every telemetry name
(`ourios.*` metrics and attributes) across the Ourios server and its
dashboard plugins.

## Layout

- `registry/` — the weaver registry (`attributes.yaml`, `metrics.yaml`,
  `events.yaml`, `manifest.yaml`). The manifest depends on upstream
  [semantic-conventions] at a pinned tag.
- `templates/registry/rust/` — renders the `ourios-semconv` Rust
  constants crate (consumed by [ourios]).
- `templates/registry/ts/` — renders the TypeScript constants module
  (consumed by [ourios-grafana-datasource] and [ourios-perses-plugin]).

## Consuming

Each consumer pins a ref of this repo, checks it out in CI (and via a
local helper for development), and runs [weaver]:

```sh
weaver registry generate rust <out-dir> \
    -t <checkout>/templates -r <checkout>/registry --future
# or: weaver registry generate ts <out-dir> ...
```

Consumers commit the generated output and gate it with a no-diff check
against their pinned ref, so builds never need weaver and a registry
change is always an explicit pin bump.

Note: weaver resolves the manifest's upstream dependency over the
network; if GitHub rate-limits the archive fetch (HTTP 429), point the
dependency at a local clone via a temporary manifest.

## Changing a name

Names follow the upstream [naming conventions]; new names are
formulated against the OpenTelemetry docs before they land here. A
change ships as: PR to this repo → tag → pin bumps in the consumers
(each regenerates and commits its output).

[semantic-conventions]: https://github.com/open-telemetry/semantic-conventions
[weaver]: https://github.com/open-telemetry/weaver
[naming conventions]: https://opentelemetry.io/docs/specs/semconv/general/naming/
[ourios]: https://github.com/jensholdgaard/ourios
[ourios-grafana-datasource]: https://github.com/jensholdgaard/ourios-grafana-datasource
[ourios-perses-plugin]: https://github.com/jensholdgaard/ourios-perses-plugin
