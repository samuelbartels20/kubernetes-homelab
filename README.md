# Kubernetes-homelab

## Why a homelab?
My motivation for having a homelab is that it is a great way to learn and educate myself and pick up new skills that I might have use for in at my work.
Besides that I'm also running some services that are used daily by myself, family & friends.

## ✨ Features

A Kubernetes cluster deployed with [Talos Linux](https://github.com/siderolabs/talos) and an opinionated implementation of [Flux](https://github.com/fluxcd/flux2) using [GitHub](https://github.com/) as the Git provider, [sops](https://github.com/getsops/sops) to manage secrets and [cloudflared](https://github.com/cloudflare/cloudflared) to access applications external to your local network.

- **Required:** Some knowledge of [Containers](https://opencontainers.org/), [YAML](https://noyaml.com/), [Git](https://git-scm.com/), and a **Cloudflare account** with a **domain**.
- **Included components:** [flux](https://github.com/fluxcd/flux2), [cilium](https://github.com/cilium/cilium), [cert-manager](https://github.com/cert-manager/cert-manager), [spegel](https://github.com/spegel-org/spegel), [reloader](https://github.com/stakater/Reloader), [ingress-nginx](https://github.com/kubernetes/ingress-nginx/), [external-dns](https://github.com/kubernetes-sigs/external-dns) and [cloudflared](https://github.com/cloudflare/cloudflared).


## Core Components

[actions-runner-controller](https://github.com/actions/actions-runner-controller): Self-hosted Github runners.
[cilium](https://github.com/cilium/cilium): Internal Kubernetes container networking interface.
[external-secrets](https://github.com/external-secrets/external-secrets): Managed Kubernetes secrets using 1Password Connect.
[rook](https://github.com/rook/rook): Distributed block storage for peristent storage.
[volsyn](https://github.com/backube/volsync): Backup and recovery of persistent volume claims.


## GitOps
Flux watches the clusters in my [kubernetes](https://github.com/samuelbartels20/kubernetes-homelab/tree/main/kubernetes) folder (see Directories below) and makes the changes to my clusters based on the state of my Git repository.
The way Flux works for me here is it will recursively search the kubernetes/${cluster}/apps folder until it finds the most top level kustomization.yaml per directory and then apply all the resources listed in it. That aforementioned kustomization.yaml will generally only have a namespace resource and one or many Flux kustomizations (ks.yaml). Under the control of those Flux kustomizations there will be a HelmRelease or other resources related to the application which will be applied.
[Renovate](https://github.com/renovatebot/renovate) watches my entire repository looking for dependency updates, when they are found a PR is automatically created. When some PRs are merged Flux applies the changes to my cluster.


**Other features include:**

- Dev env managed w/ [mise](https://mise.jdx.dev/)
- Workflow automation w/ [GitHub Actions](https://github.com/features/actions)
- Dependency automation w/ [Renovate](https://www.mend.io/renovate)
- Flux `HelmRelease` and `Kustomization` diffs w/ [flux-local](https://github.com/allenporter/flux-local)
- Terraform for infrastructure provisioning
