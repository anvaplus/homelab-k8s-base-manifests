# homelab-k8s-base-manifests

## Overview

This repository is the chart source for workload deployments in the homelab GitOps model.

It contains:

- Deployable application charts.
- Reusable Helm library templates.
- Versioned chart directories for controlled upgrades.

## GitOps Architecture (4 Repositories)

- [homelab-k8s-argo-config](https://github.com/anvaplus/homelab-k8s-argo-config): Argo CD platform and projects.
- [homelab-k8s-base-manifests](https://github.com/anvaplus/homelab-k8s-base-manifests): Chart and template source (this repository).
- [homelab-k8s-environments](https://github.com/anvaplus/homelab-k8s-environments): Version registry values per environment.
- [homelab-k8s-environments-apps](https://github.com/anvaplus/homelab-k8s-environments-apps): Runtime values and Application definitions.

## Repository Layout

```text
charts/
  microservices/
    <workload>/
      <version>/              # Deployable chart

templates/
  microservices-template/
    <template-name>/
      <version>/              # Helm library chart
```

## Contract With The Other Repositories

For each deployed app, Argo CD combines:

1. Chart defaults from this repository.
2. Values from homelab-k8s-environments-apps.
3. Values from homelab-k8s-environments.

Important: In current Application manifests, the environments values file is listed after environments-apps values, so it has higher precedence if keys overlap. Because of that, homelab-k8s-environments should stay version-focused (for example only version), and runtime keys should stay in homelab-k8s-environments-apps.

## Versioning Strategy

- Chart folders are versioned (for example v0.1.0).
- Chart.yaml version follows semantic versioning.
- New chart version should be introduced by copying previous version folder and evolving templates there.

## Validation

Example with Hugo chart:

```bash
helm dependency build charts/microservices/hugo/v0.1.0
helm template test charts/microservices/hugo/v0.1.0 -f charts/microservices/hugo/v0.1.0/values-template.yaml
```

## Additional Documentation

- [charts/microservices/hugo/README.md](charts/microservices/hugo/README.md)
- [templates/microservices-template/template-hugo/README.md](templates/microservices-template/template-hugo/README.md)
