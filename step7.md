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

```
NAMESPACE     NAME                               CHART                        VERSION   SOURCE KIND      SOURCE NAME              READY   STATUS                                 AGE
flux-system   flux-system-grafana                grafana                      6.1.10    HelmRepository   grafana                  True    Fetched revision: 6.1.10               113s
flux-system   flux-system-hello-kubernetes       ./charts/hello-kubernetes    *         GitRepository    oracle-gitops-workshop   True    Fetched and packaged revision: 0.1.2   10m
flux-system   flux-system-kubernetes-dashboard   kubernetes-dashboard         *         HelmRepository   kubernetes-dashboard     True    Fetched revision: 4.0.0                113s
flux-system   flux-system-metrics-server         metrics-server               5.0.2     HelmRepository   bitnami                  True    Fetched revision: 5.0.2                113s
flux-system   flux-system-observability          ./charts/k8s-observability   *         GitRepository    oke-day2                 True    Fetched and packaged revision: 0.1.0   113s
flux-system   flux-system-prometheus             prometheus                   12.0.1    HelmRepository   prometheus               True    Fetched revision: 12.0.1               113s
flux-system   flux-system-sealed-secrets         sealed-secrets               1.10.x    HelmRepository   stable                   True    Fetched revision: 1.10.3               113s
```

5. Observe helm charts installed

```
kubectl get helmreleases.helm.toolkit.fluxcd.io -A
```

```
NAMESPACE     NAME                   READY   STATUS                             AGE
flux-system   grafana                True    Release reconciliation succeeded   2m51s
flux-system   hello-kubernetes       True    Release reconciliation succeeded   11m
flux-system   kubernetes-dashboard   True    Release reconciliation succeeded   2m51s
flux-system   metrics-server         True    Release reconciliation succeeded   2m51s
flux-system   observability          True    Release reconciliation succeeded   2m51s
flux-system   prometheus             True    Release reconciliation succeeded   2m51s
flux-system   sealed-secrets         True    Release reconciliation succeeded   2m51s
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