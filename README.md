# Hangar

![Phorge logo](https://avatars.githubusercontent.com/u/187407936?s=200&v=4)

GitOps repository for all Phorge Kubernetes clusters, managed with [FluxCD](https://fluxcd.io/) and [SOPS](https://getsops.io/).

## Clusters

| Cluster | Role | Description |
|---------|------|-------------|
| [Control](clusters/control/setup/README.md) | Control Plane | Main entry point of the infrastructure. Handles user-facing resource provisioning (VMs, etc.), serves as the AI model gateway, and hosts the infrastructure frontend. |
| [SVC](clusters/svc/setup/README.md) | Public Services | Exposes end-user services to the internet (Forgejo, Open WebUI, etc.). |
| [Core](clusters/core/setup/README.md) | Infrastructure Core | Hosts critical internal services: monitoring, logging, authentication & authorization. |

## Repository Structure

```
base/          # Shared base manifests (controllers, configs, apps)
clusters/      # Per-cluster Flux entrypoints and setup guides
overlays/      # Per-cluster Kustomize overlays
```

## Tooling

| Tool | Purpose |
|------|---------|
| [k0sctl](https://github.com/k0sproject/k0sctl/) | Kubernetes cluster provisioning |
| [Cilium](https://cilium.io/) | CNI — networking & L2 load balancing |
| [FluxCD](https://fluxcd.io/) | GitOps continuous delivery |
| [Mozilla SOPS](https://getsops.io/) + [age](https://github.com/FiloSottile/age) | Secret encryption |