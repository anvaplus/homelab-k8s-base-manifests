# Hugo Deployable Chart

## Location

charts/microservices/hugo/v0.1.0

## Purpose

Deploy a Hugo workload through a versioned application chart that consumes the shared Hugo library template.

## Dependency

- templates/microservices-template/template-hugo/v0.1.0

## Cross-Repository Value Ownership

- homelab-k8s-base-manifests: chart defaults and templates.
- homelab-k8s-environments-apps: runtime values (replicas, env, resources, secret mappings, image repository).
- homelab-k8s-environments: deployment version (version).

Current Argo CD manifests list values files in this order:

1. values from homelab-k8s-environments-apps
2. values from homelab-k8s-environments

So the second file wins on conflicts. Keep homelab-k8s-environments version-only to avoid accidental runtime overrides.

## Required Values

- appName
- componentName
- namespace
- version
- image.repository

Optional:

- image.baseUrl (defaults to docker.io)
- deployment.spec.replicas
- service
- probe
- envConf
- externalSecretsConfig
- resources

## Rendered Image Format

<image.baseUrl>/<image.repository>:<version>

## Behavior

- Ingress is managed outside this chart flow.
- Service and probes have defaults when omitted.
- envConf and externalSecretsConfig are nil-safe.

## Local Validation

```bash
helm dependency build charts/microservices/hugo/v0.1.0
helm template test charts/microservices/hugo/v0.1.0 -f charts/microservices/hugo/v0.1.0/values-template.yaml
```
