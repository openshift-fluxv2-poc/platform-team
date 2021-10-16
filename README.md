# Platform Team

Here's a Flux multi-tenancy demo for OpenShift. This work has been derived from the original Flux multi-tenancy example, which can be found [here](https://github.com/fluxcd/flux2-multi-tenancy).

Persona: **Alice** is an OpenShift Cluster-Admin. She'd like to use Flux in an OpenShift-ish way via its Web UI by

  1. Installing Flux via OperatorHub
  1. Bootstraping the multi-tenant setup with copy & paste via OpenShift YAML import

and achieve the similar result with the CLI setup.

Persona: **Chanwit** is a member of the Dev team. Please change user name, or add other users as team members [here](https://github.com/openshift-fluxv2-poc/platform-team/blob/main/tenants/base/dev-team/rbac.yaml#L33) before proceed.

## Production Cluster: Source & Kustomization

You can copy the below YAML snippet, and import it directly into OpenShift to kick off the setup without using CLI.
If you fork, please change the repo URL at this [line](https://github.com/openshift-fluxv2-poc/platform-team/blob/main/README.md?plain=1#L30) to match the forked one.

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
  url: 'https://github.com/openshift-fluxv2-poc/platform-team' # If you fork, please change this repo to match the forked one.
  ref:
    branch: main
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  namespace: flux-system
  name: multi-tenant-production
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

## Staging Cluster: Source & Kustomization
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
  name: multi-tenant-staging
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
