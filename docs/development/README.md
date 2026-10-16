# Development

Requires Node.js 22 through 24 and npm 10 or later. The plugin was scaffolded with the official `@grafana/create-plugin` tool and uses its webpack, Jest, Playwright, and plugin packaging setup. Do not edit files in `.config/`; use the Grafana plugin-tools extension mechanisms when configuration changes are needed.

Run local checks:

```bash
npm ci
npm run typecheck
npm run lint
npm run test:ci
npm run build
```

Run the local Grafana container with `npm run server`. The Playwright suite uses `@grafana/plugin-e2e`; test against the minimum Grafana version in `src/plugin.json` and a current development image before release. Local build and unit-test results do not replace hosted compatibility checks, plugin signing, artifact integrity, provenance, or deployment validation.

When changing dashboard queries, verify metric names and labels against Orbit Core diagnostics and the Prometheus exporter. Add dashboard JSON checks for every new metric dependency and update this documentation when installation or security assumptions change.

## Latest local checkpoint (2026-10-10)

The package's type check, ESLint check, Jest suite (**3 tests**), and production webpack build passed
on Node.js 22.23.3 and 24.21.0. These versions exercise both ends of the declared Node 22–24 range.
The build uses the repository's Grafana webpack configuration. This checkpoint did not run the
Playwright suite against a Grafana server, verify every supported Grafana release, sign the plugin,
or validate a published artifact. ESLint reports that `@stylistic/eslint-plugin-ts` is deprecated;
the warning does not fail lint and should be handled in a future dependency-maintenance change.

The live npm advisory database reports no vulnerabilities among the package's production
dependencies (`npm audit --omit=dev`). A full `npm audit` reports 8 high and 19 moderate advisories
in development dependencies. The high findings include the plugin-signing tool's proxy-agent chain
and the webpack ESLint plugin's `micromatch`/`braces` chain. The registry currently offers no
compatible fix for those dependency ranges; `npm audit fix --dry-run` proposes major downgrades for
the signing tool and Jest. Keep the development audit open until compatible upstream releases or a
verified dependency replacement are available; do not use a forced downgrade as a release fix.
