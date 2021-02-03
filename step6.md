### Refer to values in ConfigMap

It is possible to define a list of ConfigMap and Secret resources from which to take values. The values are merged in the order given, with the later values overwriting earlier. These values always have a lower priority than the values inlined in the HelmRelease via the spec.values parameter.

```
spec:
  valuesFrom:
  - kind: ConfigMap
    name: prod-env-values
    valuesKey: values-prod.yaml
  - kind: Secret
    name: prod-tls-values
    valuesKey: crt
    targetPath: tls.crt
```

1. Create `ConfigMap`
```
echo "replicaCount: 2" > values.yaml
kubectl create configmap myconfig --from-file=values.yaml --namespace=flux-system --output=yaml --dry-run > myconfig.yaml
cat myconfig.yaml
```

2. Commit the ConfigMap `myconfig.yaml` to a Git repository
   1. Open `oracle-gitops-workshop` repository in your GitHub webpage
   2. Go to `clusters/default/flux-system/` directory
   3. Сlick on `Add file` -> `Create new file`
   4. Fill filename `myconfig.yaml` to `Name your file...` field
   4. Copy & Paste `myconfig.yaml` content to text area and click on `Commit changes`

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