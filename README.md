# Summary

This branch (`helm`) contains the default Helm-only release content used by
vandoor when a new team does not have the KOTS installer entitlement enabled.

The `main` branch holds the KOTS-flavored default release content used when a
new team does have KOTS enabled. This `helm` branch is its non-KOTS counterpart.

## How it's used

Vandoor's Makefile target `gen-helm-default-specs` clones this branch into
`./gen/helm_default_specs/` and bundles the files into a Go constant
(`gen.HelmDefaultNewSpec`). The release-draft handler stamps that content onto
the new draft when a KOTS-disabled team creates their first release.

## Contents

- `nginx-<version>.tgz` — a minimal plain-nginx Helm chart, pre-packaged via
  `helm create nginx` with the boilerplate trimmed.
- `helmchart.yaml` — the Replicated `kind: HelmChart` v1beta2 CR pointing to the
  bundled chart. The CR's `spec.chart.name` and `spec.chart.chartVersion` must
  match the chart's `Chart.yaml` exactly (required by the validator at
  `pkg/kots/release/spec/helmchart_validation.go`, added in
  [PR #9728](https://github.com/replicatedhq/vandoor/pull/9728)).

More info on KOTS: https://kots.io
