# Paused rollout investigation checklist

A paused rollout is not necessarily a failed rollout. Separate an intentional
pause step, an analysis result and a workload readiness problem.

## Inspect before promoting

With the Argo Rollouts kubectl plugin installed, use an authorized lab context.
Replace the sample rollout and namespace with your actual values:

```sh
kubectl config current-context
kubectl argo rollouts get rollout rollouts-demo -n default
kubectl -n default get analysisruns
kubectl -n default get events --sort-by=.metadata.creationTimestamp
```

These commands read state. See [getting started](docs/getting-started.md) for the
sample resource, and [architecture](docs/architecture.md) for analysis objects.

## Classify the pause

| Evidence | Next investigation |
| --- | --- |
| Manual pause step | Confirm the approved promotion criteria and owner |
| AnalysisRun failed | Inspect metric result and configured failure condition |
| AnalysisRun errored | Inspect provider connectivity, authentication and query validity |
| AnalysisRun inconclusive | Review thresholds and measurement quality |
| New pods are not ready | Check events, image pull, probes and resource availability |
| Replica ratio looks right but traffic differs | Check the traffic router and stable/canary Service selectors |

An analysis error is not the same as a measured application failure. Record the
actual phase and measurement before proposing remediation.

## Before any action

Capture rollout revision, step, analysis result, traffic routing and current
stable/canary health. Promotion, abort and retry change state; they require a
reviewed decision rather than being part of this diagnostic checklist.
An abort is not a guarantee that all external side effects or data migrations
have been undone.

Use synthetic traffic for lab tests and keep production rollout controls
separate from diagnostic access.

## Development note

This troubleshooting guide was added with AI assistance. Upstream code,
licenses and contributor attribution remain unchanged.
