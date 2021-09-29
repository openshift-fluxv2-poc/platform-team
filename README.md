# Platform Team

Alice is a Cluster-Admin.

## Setup multi-tenant via OpenShift YAML import

### Source & Kustomization
```
---
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  namespace: flux-system
  name: platform-team
spec:
  timeout: 20s
  gitImplementation: libgit2
  interval: 1m
  url: 'https://github.com/openshift-fluxv2-poc/platform-team'
  ref:
    branch: main
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  namespace: flux-system
  name: multi-tenant-policy-enforcement
spec:
  timeout: 2m
  path: ./infra
  interval: 5m
  prune: true
  force: false
  sourceRef:
    name: platform-team
    kind: GitRepository
```
