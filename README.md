# Planes

![Phorge logo](https://avatars.githubusercontent.com/u/187407936?s=200&v=4)

All the Kubernetes Planes running @ Phorge !

## Setup

requirements:

- [k0sctl](https://github.com/k0sproject/k0sctl/)
- [Cilium](https://cilium.io/)
- [FluxCD](https://fluxcd.io/)
- [Mozilla SOPS](https://getsops.io/)

1. Deploy cluster

```bash
cd cluster/<cluster-name>
k0sctl apply -c k0s-cluster.yml
```

2. Get kubeconfig

```bash
cd cluster/<cluster-name>
k0sctl kubeconfig -c k0s-cluster.yml
```

3. Install Cilium

```bash
cilium install --version 1.19.2 \
  --helm-set ipam.operator.clusterPoolIPv4PodCIDRList="10.244.0.0/16" \
  --helm-set envoy.enabled=false \
  --helm-set l2announcements.enabled=true \
  --helm-set bpf.masquerade=true \
  --helm-set hubble.enabled=false \
  --helm-set bpf.mapDynamicSizeRatio=0.001 \
  --helm-set bpf.distributedLRU.enabled=false \
  --helm-set bpf.ctTcpMax=131072 \
  --helm-set bpf.ctAnyMax=65536 \
  --helm-set bpf.natMax=131072
```

4. Create sops key for the cluster

```bash
age-keygen -o age.agekey
kubectl create namespace flux-system
cat age.agekey |
kubectl create secret generic sops-age \
--namespace=flux-system \
--from-file=age.agekey=/dev/stdin
rm age.agekey
```

5. Bootstrap flux

```bash
flux bootstrap github --owner=phorge-fr --repository=Planes --branch=main --path=cluster/<cluster-name> --token-auth=true
```