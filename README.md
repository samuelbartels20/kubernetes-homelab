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

## Homelab + GitOps

Welcome to my repo where I maintain everything related to my homelab which adheres to Infrastructure as Code (IaC) and GitOps practices where possible. This allows me to have a single source of written truth for my homelab, declaring how and where I want it setup. I have a Kubernetes cluster that runs most of the services in my homelab but I also have a few services running as Docker containers on my NAS.
This allows me to:
- Version control my changes, allowing easy rollback of breaking patches/tinkering/etc
- Allow for easy reinstall/disaster recovery of a cluster.
- Version control and declare hardware provisioning, ensuring repeatable and robust hardware configuration.
- With [Talos](https://github.com/siderolabs/talos) and a few scripts that I have written, I can define and provision a cluster easily with as few manual steps as possible.
- [Renovate](-https://www.mend.io/renovate) and a few scripts makes it easy to handle Talos and Kubernetes updates. Renovate will create pull requests when new updates are available. When the pull requests are merged to the main branch I only need to run a commands to upgrade the cluster.

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

