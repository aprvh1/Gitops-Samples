# GitOps Sample  

This repository contains various **GitOps Sample Pipelines** using **Harness GitOps** to help you automate deployments, sync applications, and handle failure scenarios efficiently. These samples cover different aspects of GitOps workflows, including syncing applications, automating PR-based deployments, handling failures, integrating security testing, and setting up notifications for pipeline completion.

## 🚀 Samples Covered  

Below are the different GitOps workflows covered in this repository:

### 1️⃣ [Fetching App Details and Syncing App using Harness Pipeline](https://github.com/harness-community/Gitops-Samples/tree/gitops-1/Fetch-App-Sync)  
Learn how to create a **Harness GitOps pipeline** that fetches application details and syncs it with the desired state defined in Git. 
This sample covers:  
✔ Setting up a **GitOps pipeline** in Harness  
✔ Fetching application details from the Git repository  
✔ Syncing the application using Harness GitOps  

---

### 2️⃣ [PR Pipeline: Update Values, Approve, Merge & Sync GitOps App](https://github.com/harness-community/Gitops-Samples/tree/main/PR-Pipeline)  
This sample demonstrates a **PR-based GitOps pipeline**, where application updates go through a **Pull Request (PR) workflow** before being deployed. 
Key highlights:  
✔ Updating `values.yaml` in a GitOps repository  
✔ Raising a **PR to review changes** before deployment  
✔ Obtaining **manual approval** before merging  
✔ Merging the PR and **syncing the GitOps application**  

---

### 3️⃣ [GitOps PR Pipeline with Failure Strategy](https://github.com/harness-community/Gitops-Samples/tree/main/Failure-Strategy-PR-Pipeline)  
This sample builds on the PR pipeline by adding a **Failure Strategy** to ensure the application remains in a **healthy state** in case of sync failures.  
✔ If **GitOps sync fails**, the pipeline will **revert the merged PR**  
✔ This restores the application to a **previous healthy state**  
✔ After reverting, the pipeline **re-syncs** the GitOps application  

---

### 4️⃣ [Adding Notifications to the GitOps PR Pipeline](https://github.com/harness-community/Gitops-Samples/tree/main/Notifications-PR-Pipeline)  
This sample focuses on **adding notifications** to a GitOps PR pipeline. It ensures that users are informed when a pipeline completes successfully.  
✔ Configuring **email notifications**  
✔ Selecting **pipeline events** to trigger notifications  
✔ Adding **recipients and user groups**  

---

### 5️⃣ [PR Pipeline with Security Testing Orchestration (STO) and Rollback](https://github.com/harness-community/Gitops-Samples/tree/main/PR-Pipeline-STO)
This sample extends the PR Pipeline by integrating Security Testing Orchestration (STO) to scan for vulnerabilities before deploying changes.

✔ **Aqua Trivy** for scanning container images for security vulnerabilities

✔ **Gitleaks** for detecting secrets and vulnerabilities in the Git repository

✔ Failure Strategy to rollback changes if high-severity vulnerabilities are found

✔ Ensures that only **secure deployments** make it to production

---

### 6️⃣ [Syncing Multiple GitOps Applications](https://github.com/harness-community/Gitops-Samples/tree/main/Syncing-multiple-apps)
This sample demonstrates how to **sync multiple GitOps applications** in Harness GitOps pipelines.

✔ Manually select multiple applications for syncing in Harness pipelines.

✔ Use a **matrix looping strategy** to dynamically sync multiple applications at runtime.

✔ Improve efficiency by syncing multiple applications in a single pipeline execution.

---

### 7️⃣ [Deploying to a New Environment Using ApplicationSet in PR Pipeline](https://github.com/harness-community/Gitops-Samples/tree/main/Application-Set)

This sample demonstrates how to deploy an application to a new environment using **ApplicationSet in a PR Pipeline**.

✔ Uses ApplicationSet to dynamically generate applications for new environments.

✔ Automatically detects environment configurations in the repo and provisions applications.

✔ PR pipeline workflow includes creating a **config file, approval, merge, and syncing ApplicationSet**.

✔ Git Generator fetches environment details from `config.json` for automation.

✔ Streamlines deployments across multiple environments using a GitOps-driven workflow.

---

### 8️⃣ [Harness Secret Expressions in GitOps Application Manifests](./GitOps-Secret-Expressions)

This guide demonstrates how to use **Harness Secret Expressions** directly in GitOps application manifests for secure, centralized secret management.

**Key Features**:

✔ **Centralized Secret Management**: Create secrets once in Harness (backed by HashiCorp Vault) and reference them across all applications

✔ **No Separate Manifests Needed**: Same manifests work for both GitOps Applications and CD Pipelines

✔ **Portable Helm Charts**: Keep your Helm templates vendor-agnostic - expressions go in `values.yaml`, not in templates

✔ **Enhanced Security**: Secrets are resolved during deployment and automatically masked in UIs

**Includes Three Sample Applications**:

📁 **Sample 1: Simple Kubernetes Secret** ([`/simple-example`](./GitOps-Secret-Expressions/simple-example/))
- Secret expressions **directly in Secret manifest** (`stringData` field)
- Simplest approach for teams using plain Kubernetes YAML
- Shows account/org/project-level secret scopes

📁 **Sample 2: Helm Chart with Values File** ([`/helm-values-example`](./GitOps-Secret-Expressions/helm-values-example/))
- Secret expressions in **`values.yaml` file**
- Templates use standard `{{ .Values.* }}` syntax (stays portable!)
- Best practice for Helm users - no vendor lock-in

📁 **Sample 3: Shared Manifests** ([`/shared-manifests-example`](./GitOps-Secret-Expressions/shared-manifests-example/))
- Proves you **don't need separate manifests** for GitOps vs CD Pipeline
- Same Git repo works for both deployment methods
- Eliminates duplication

**Documentation Includes**:
- Patch script for existing GitOps agents
- Step-by-step setup for each sample
- Clear explanation of where secret expressions work (and don't work)
- Vault configuration details
- Troubleshooting guide

**Secret Expression Examples**:
```yaml
# In Secret manifest or values.yaml:
vault-secret: <+secrets.getValue("account.vaultSecret")>
api-key: <+secrets.getValue("org.apiKey")>
db-password: <+secrets.getValue("dbPassword")>
```

**Perfect for**: Teams looking to eliminate hardcoded secrets in Git while maintaining GitOps best practices with HashiCorp Vault integration.

---

### 9️⃣ [Argo Rollouts Examples](./Argo-Rollouts-Examples)

This sample contains comprehensive examples demonstrating various **Argo Rollouts deployment strategies** and progressive delivery features for Kubernetes applications.

**Key Examples Included**:

✔ **Canary Deployments**: Rollout using the canary update strategy with gradual traffic shifting

✔ **Blue-Green Deployments**: Rollout using the blue-green update strategy for instant traffic switching

✔ **Canary Analysis**: Rollout with automated canary analysis using Prometheus metrics for validation

✔ **Experiment (A/B Testing)**: Perform A/B tests with analysis against different variants using job metric providers

✔ **Preview Stack Testing**: Launch experiments that test preview stacks without production traffic

✔ **Canary with Istio**: Two approaches for traffic splitting:
  - Host-level traffic splitting during updates
  - Subset-level traffic splitting for more granular control

**Features**:
- Complete demo application with multiple deployment strategies
- Integration with service meshes (Istio) for advanced traffic management
- Automated analysis and validation during rollouts
- Support for various metric providers (Prometheus, Wavefront, Jobs)
- Load testing capabilities included

**Perfect for**: Teams implementing progressive delivery, canary deployments, and advanced rollout strategies in Kubernetes with Argo Rollouts.

---

## 🎯 Why Use These GitOps Samples?  
✅ **Automate deployments** using GitOps principles  

✅ **Improve visibility** into GitOps sync failures and rollback strategies

✅ **Enhance control** over application updates with PR-based workflows  

✅ **Get notified** when deployments complete successfully

✅ **Strengthen security** by integrating security testing into the pipeline

✅ **Increase efficiency** by syncing multiple applications dynamically

✅ **Centralize secret management** with Harness secret expressions in manifests

✅ **Implement progressive delivery** with Argo Rollouts examples for canary, blue-green, and A/B testing strategies

These examples provide a step-by-step guide to setting up Harness GitOps pipelines efficiently. Whether you're new to GitOps or looking to enhance your workflows, these samples will help streamline your deployments! 🚀
