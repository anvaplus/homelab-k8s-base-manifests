# Hugo Template Chart

## Location

templates/microservices-template/template-hugo/v0.1.0

## Purpose

Provide reusable Helm library templates for Hugo-based workloads.

## Rendered Resources

- Deployment
- Service
- ConfigMap from envConf
- ExternalSecret from externalSecretsConfig.envSecrets

## Runtime Conventions

- deployment.spec.replicas default is 1.
- Service defaults are applied when service is not provided.
- Probe defaults are applied when probe is not provided.
- envConf is optional and nil-safe.
- externalSecretsConfig is optional and nil-safe.

## Value Contract

This template expects a deployment chart to pass common workload values including:

- appName
- componentName
- namespace
- image.repository
- version

The version field is required because image tag rendering is built as:

<image.baseUrl or docker.io>/<image.repository>:<version>

## Secret Store Default

Default secret store name for ExternalSecret rendering:

- op-cluster-secret-store
