### Provision `oke-day2` components

1. Add `oke-day2` repository as git source
```
./flux create source git oke-day2 \
--url=https://github.com/oqva-io/oke-day2 \
--branch=master \
--interval=30s
```

2. Provision `oke-day2` components on k8s cluster
```
./flux create kustomization flux-system \
--source=oke-day2 \
--path="./clusters/default/flux-system" \
--prune=true \
--interval=30s
```

3. Verify all pods in `ready` state
 ```
watch kubectl get pod -A 
```

4. Observe `oke-day2` Helm charts synced
```
kubectl get helmcharts.source.toolkit.fluxcd.io -A
```

5. Observe helm charts installed
```
kubectl get helmreleases.helm.toolkit.fluxcd.io -A
```

6. Open kubernetes dashboard
   1. Go to `http://instanceIp:30000`
   2. Click on `Custom Resource Definitions`
   3. Click on `Helm Release`
   4. Choose namespace `flux-system`
   5. Click on `kubernetes-dashboard` object
    
7. Open flux dashboard in Grafana
    1. Go to `http://instanceIp:30001`
    2. Click on `Home` link
    3. Choose `Flux Cluster Stats` dashboard