### Commit invalid value to break the helm release

1. Set watching for pods (`Cloud Shell`)
 ```
watch kubectl get pod 
```

3. Commit broken image repository
    1. Open `oracle-gitops-workshop` repository in your GitHub webpage
    2. Go to `clusters/default/flux-system/hello-kubernetes.yaml` file
    3. Сlick on pencil to edit the file
    4. Under the `values` section and broken image repository and click on `Commit changes`
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
    image:
      repository: not-exists
  valuesFrom:
  - kind: ConfigMap
    name: myconfig
```

4. Observe events in kubernetes dashboard (after ~1m)

`Helm upgrade failed: timed out waiting for the condition`

5. Observe flux dashboard for `Failing Reconcilers` is equal to 1 (after ~2 min)

6. Observe pods state (step 1), one pod will fail with `ImagePullBackOff` status

7. Revert change of broken image repository
    1. Open `oracle-gitops-workshop` repository in your GitHub webpage
    2. Go to `clusters/default/flux-system/hello-kubernetes.yaml` file
    3. Сlick on pencil to edit the file
    4. Under the `values` remove image section and click on `Commit changes`
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

8. Observe events in kubernetes dashboard (after ~1m)

`Helm upgrade succeeded`

9. Observe flux dashboard for `Failing Reconcilers` is equal to 0

10. Observe pods state (step 1) - all pods in `Running` state

### Commit invalid value to break the helm release with remediation

1. Set watching for pods (`Cloud Shell`)
 ```
watch kubectl get pod 
```

2. Commit broken image repository
    1. Open `oracle-gitops-workshop` repository in your GitHub webpage
    2. Go to `clusters/default/flux-system/hello-kubernetes.yaml` file
    3. Сlick on pencil to edit the file
    4. Under the `values` section and broken image repository and click on `Commit changes`
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
    image:
      repository: not-exists
  upgrade:
      remediation:
          remediateLastFailure: true
  valuesFrom:
  - kind: ConfigMap
    name: myconfig
```

3. Observe events in kubernetes dashboard (after ~1m)

`Helm upgrade failed: timed out waiting for the condition`

4. Observe flux dashboard for `Failing Reconcilers` is equal to 1

5. Observe pods state (step 1) all pods in ready state