### Provision `Flux`

1. Install `Flux` cli
```
curl -s https://toolkit.fluxcd.io/install.sh | bash -s $PWD
```
2. Install `Flux` controllers on k8s cluster
```
./flux install
```
3. Verify `flux` pods in `ready` state
```
kubectl get pod -n flux-system
```
4. Observe flux custom resources
````
kubectl get crds -A
````