# Platform Team

Alice is an OpenShift Cluster-Admin. She'd like to use Flux in an OpenShift-ish way via its Web UI by

  - Installing Flux via OperatorHub
  - Bootstraping the platform setup with copy & paste via OpenShift YAML import

and achieve the similar result with the CLI setup.

## Setup multi-tenant via OpenShift YAML import

### Production: Source & Kustomization
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
  path: ./clusters/production
  interval: 5m
  prune: true
  force: false
  sourceRef:
    name: platform-team
    kind: GitRepository
```

### Staging: Source & Kustomization
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
  path: ./clusters/staging
  interval: 5m
  prune: true
  force: false
  sourceRef:
    name: platform-team
    kind: GitRepository
```
