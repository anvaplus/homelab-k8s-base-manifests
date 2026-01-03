# homelab-k8s-base-manifests

## Overview

This repository contains reusable Helm chart templates for deploying applications and services in the Homelab environment. It serves as the single source of truth for application deployment patterns, ensuring consistency and reusability.

*Note: This document represents a plan. The contents of this repository will evolve as the homelab is built and refined.*

## 🔗 Related Repositories

This repository is part of a 4-repository GitOps architecture:

- **[homelab-k8s-argo-config](https://github.com/anvaplus/homelab-k8s-argo-config)** - ArgoCD configurations for platform tools
- **[homelab-k8s-base-manifests](https://github.com/anvaplus/homelab-k8s-base-manifests)** (This repository) - Helm chart templates for applications
- **[homelab-k8s-environments](https://github.com/anvaplus/homelab-k8s-environments)** - Version declarations for applications
- **[homelab-k8s-environments-apps](https://github.com/anvaplus/homelab-k8s-environments-apps)** - Application configuration values

## Purpose

This repository provides:
- **Standardized Helm charts** for different types of applications (e.g., web apps, APIs).
- **Reusable templates** for consistent deployments across all environments.

## Repository Structure

The planned structure for this repository is as follows:
```
├── charts/                           # Production-ready Helm charts
│   ├── common/                     # A common chart for simple deployments
│   └── applications/               # Charts for specific applications
└── templates/                        # Template definitions (source of charts)
    └── common-template/            # Base template for the common chart
```

## Key Components

### Common Chart

A generic chart in `charts/common/` can be used to deploy most applications with a standard set of features:
- Deployment/StatefulSet
- Service
- Ingress
- ConfigMaps and Secrets
- Health checks
- Resource limits

### Application-Specific Charts

For applications with unique requirements, specific charts can be created under `charts/applications/`.

## How It Works

Applications deployed via ArgoCD reference the charts in this repository. The configuration for these applications is provided by the `homelab-k8s-environments-apps` repository, and the version of the application to be deployed is specified in the `homelab-k8s-environments` repository.

### Example Flow

1. **`homelab-k8s-environments-apps`** defines which chart to use and provides the values:
   ```yaml
   # In an ArgoCD Application manifest
   source:
     repoURL: 'https://github.com/anvaplus/homelab-k8s-base-manifests.git'
     targetRevision: main
     path: 'charts/common'
     helm:
       valueFiles:
         - 'values.yaml' # From homelab-k8s-environments-apps
   ```

2. **`homelab-k8s-environments`** specifies the application version:
   ```yaml
   # In a values file
   image:
     tag: "v1.2.3"
   ```

3. ArgoCD combines the chart from this repo with the values from the other repos to deploy the application.

## Chart Versioning

Charts in this repository should follow semantic versioning (MAJOR.MINOR.PATCH).

### Creating New Chart Versions

1. Copy the previous version of a chart directory.
2. Update `Chart.yaml` with the new version number.
3. Make necessary changes to the templates.
4. Test the chart locally before committing.
