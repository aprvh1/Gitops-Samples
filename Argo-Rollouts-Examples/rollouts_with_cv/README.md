## Overview

This example demonstrates how to combine **Argo Rollouts** canary deployments with **Harness Continuous Verification (CV)** to automatically validate each canary stage before promoting to the next. CV analyzes metrics and logs from your monitoring tools, ensuring each traffic phase is healthy and providing automated safety checks throughout the progressive rollout.

### What Continuous Verification Adds to Rollouts

- **Per-stage validation** — CV analyzes metrics and logs at each canary percentage (25%, 50%, 75%, 100%) before promotion
- **Automated rollback** — If CV detects anomalies, the pipeline triggers a rollback before the issue reaches more users
- **ML-based analysis** — Harness uses machine learning to establish baselines from stable pods and detect deviations in canary pods
- **Multi-stage sensitivity** — Each Verify step can use a different sensitivity level (HIGH, MEDIUM, LOW) depending on the stage

## Pipeline Stages

The included [pipeline.yaml](./pipeline.yaml) defines a multi-environment promotion flow:


| Stage                     | Description                                                              |
| ------------------------- | ------------------------------------------------------------------------ |
| **Dev Deploy**            | Update release repo, merge PR, sync via ArgoCD with full auto-promote    |
| **QA Deploy**             | Same PR-based flow targeting the QA environment                          |
| **Prod Approval Gate**    | Manual approval before production deployment                             |
| **Prod-NA Deploy**        | Canary rollout with CV validation at each stage (25% → 50% → 75% → 100%) |
| **HighReg Approval Gate** | Manual approval before high-regulation environment                       |
| **Prod-HighReg Deploy**   | Sync and deploy to high-regulation environment with full auto-promote    |


### Prod-NA Canary + CV Flow

The production stage is where Argo Rollouts and CV work together:

1. **ArgoCD Sync** — Syncs the GitOps application to apply manifest changes
2. **ArgoRollout Canary 25** — Shifts 25% traffic to the new version, waits for `Suspended` status
3. **Verify_1** (HIGH sensitivity, 5 min) — CV compares canary vs. stable pod metrics
4. **ArgoRollout Canary 50** — Promotes to 50% traffic
5. **Verify_2** (MEDIUM sensitivity, 5 min) — Second round of CV analysis
6. **ArgoRollout Canary 75** — Promotes to 75% traffic
7. **Verify_3** (HIGH sensitivity, 5 min) — Final CV analysis before full promotion
8. **ArgoRollout Canary 100** — Full promotion to the new version

If any Verify step detects an anomaly, the failure strategy triggers a manual intervention with the option to abort the rollout and rollback.

## Prerequisites

- Argo Rollouts installed in your cluster
- Harness GitOps Agent configured and connected
- A GitOps Application with a Rollout resource using a canary strategy with `pause: {}` steps
- A **Monitored Service** in Harness with health sources (e.g., Prometheus, Datadog) configured for your application

## Full Documentation

For a complete step-by-step guide covering GitOps Application setup, Monitored Service configuration, pipeline creation, and CV analysis details, see the official documentation:

**[Argo Rollouts with Continuous Verification](https://developer.harness.io/docs/continuous-delivery/gitops/argo-rollouts/argo-rollouts-with-cv)**

## Resources

1. [Argo Rollouts Overview](https://developer.harness.io/docs/continuous-delivery/gitops/argo-rollouts/argo-rollouts-overview)
2. [Continuous Verification Overview](https://developer.harness.io/docs/continuous-delivery/verify/verify-deployments-with-the-verify-step)
3. [Create a Monitored Service](https://developer.harness.io/docs/service-reliability-management/monitored-service/create-monitored-service)
4. [Harness GitOps Basics](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics)

