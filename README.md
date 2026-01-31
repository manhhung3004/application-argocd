# ArgoCD Applications Configuration

This repository contains the **ArgoCD** manifests required to manage and deploy services using GitOps. It defines the `AppProject` and applications (`ApplicationSet` or `Application`) for various environments.

## Overview

- **Project**: `multi-env` (Defined in `app-project.yaml`)
- **Environments**:
  - `dev`
  - `staging`
  - `production`
- **Services**:
  - `service-a`
  - `service-b`

## Directory Structure

```plaintext
.
├── app-project.yaml          # ArgoCD AppProject definition (permissions, destinations)
├── service-a.yaml            # Single Application definition for Service A
├── service-a-appset.yaml     # ApplicationSet for Service A (multi-environment)
├── service-b.yaml            # Single Application definition for Service B
└── service-b-appset.yaml     # ApplicationSet for Service B (multi-environment)
```

## Prerequisites

- Kubernetes Cluster with **ArgoCD** installed.
- `kubectl` CLI configured to access the cluster.

## Getting Started

### 1. Apply the Project
First, create the `AppProject` which defines the allowed source repositories and destination namespaces.

```bash
kubectl apply -f app-project.yaml
```

### 2. Deploy Applications
You can deploy services using either single `Application` manifests or `ApplicationSet` for multiple environments.

#### Using ApplicationSet (Recommended)
This will automatically create applications for `dev`, `staging`, and `production` based on the generator list.

```bash
kubectl apply -f service-a-appset.yaml
kubectl apply -f service-b-appset.yaml
```

#### Using Single Application
To deploy a specific instance manually:

```bash
kubectl apply -f service-a.yaml
```

## Configuration Details

### ApplicationSet Generators
The `ApplicationSet` files use a `list` generator to loop through environments:

- **dev**: Deploys to `dev` namespace using `values-dev.yaml`
- **staging**: Deploys to `staging` namespace using `values-staging.yaml`
- **production**: Deploys to `production` namespace using `values-prod.yaml`

### Source Repository
- **URL**: `ssh://git@git.younetmedia.com:10022/hungnm/hungnm-test.git`
- **Revision**: `main`

## Troubleshooting

- **Sync Status**: Check the ArgoCD UI to verify that applications are `Synced` and `Healthy`.
- **Permissions**: If deployment fails with permission errors, verify that `app-project.yaml` allows access to the target `namespace` and `clusterResourceWhitelist`.
