# Flux multi-tenancy demo for OpenShift

Here's a Flux multi-tenancy demo for OpenShift. This work has been derived from the original Flux multi-tenancy example, which can be found [here](https://github.com/fluxcd/flux2-multi-tenancy).  A nice thing of the demo in this repo is that we will use only **Web UI of OpenShift** to install Flux and bootstrap the demo. Yes - as a Cluster-Admin user, you can up and running a GitOps system by just clicking. You click to install Flux via OperatorHub, then you click to import one of the follwoing snippets into your cluster, and your multi-tenant GitOps system will be ready to use in minutes.

Persona: **Alice** is an OpenShift Cluster-Admin. She'd like to use Flux in an OpenShift-ish way via its Web UI by

  1. Installing Flux via OperatorHub
  1. Bootstraping the multi-tenant setup with copy & paste via OpenShift YAML import

and achieve the similar result with the CLI setup.

Persona: **Chanwit** is a member of the Dev team. Please change user name, or add other users as team members [here](https://github.com/openshift-fluxv2-poc/platform-team/blob/main/tenants/base/dev-team/rbac.yaml#L33) before proceed with your fork.

## Production Cluster: Source & Kustomization

You can copy the below YAML snippet, and import it directly into OpenShift to kick off the setup without using CLI.
If you fork, please change the repo URL at this [line](https://github.com/openshift-fluxv2-poc/platform-team/blob/main/README.md?plain=1#L31) to match the forked one.

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
  # If you fork, please change this repo URL to match the forked one.
  url: 'https://github.com/openshift-fluxv2-poc/platform-team' 
  ref:
    branch: main
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
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
  # If you fork, please change this repo URL to match the forked one.
  url: 'https://github.com/openshift-fluxv2-poc/platform-team' 
  ref:
    branch: main
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
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
