### Refer to values in ConfigMap

1. Create values file
```
echo "replicaCount: 2" > values.yaml
```

2. Create `ConfigMap`
```
kubectl create configmap myconfig --from-file=values.yaml --namespace=flux-system
```

3. Set watching for pods
```
watch kubectl get pod
```

4. Define a `ConfigMap` resource from which to take the values
   1. Open `oracle-gitops-workshop` repository in your GitHub webpage
   2. Go to `clusters/default/flux-system/hello-kubernetes.yaml` file
   3. Сlick on pencil to edit the file
   4. After the `values` section and new `valuesFrom` section
```
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: hello-kubernetes
  namespace: flux-system
spec:
  interval: 1m
  timeout: 1m
  chart:
    spec:
      chart: ./charts/hello-kubernetes
      sourceRef:
        kind: GitRepository
        name: oracle-gitops-workshop
      interval: 1m
  targetNamespace: default
  values:
    message: This is new one
  valuesFrom:
  - kind: ConfigMap
    name: myconfig
```

5. See the amount of pods changed