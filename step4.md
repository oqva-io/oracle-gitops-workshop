### Refer to values inline

1. Set watching for pods
```
watch kubectl get pod
```
   
2. Update `message` parameter
    1. Open `oracle-gitops-workshop` repository in your GitHub webpage
    2. Go to `clusters/default/flux-system/hello-kubernetes.yaml` file
    3. Сlick on pencil to edit the file 
    4. Under the `values` section add new `message` parameter and click on `Commit changes`
    5. Observe the `Latest commit` hash value
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
    message: Hello from GitHub!
```
       
3. What changed?
    1. Go back to CloudShell and see pods are updated
    2. Observe flux reconcile git repository to the latest commit
    ```
    kubectl get gitrepositories.source.toolkit.fluxcd.io -A
    ```
    2. Observe `kustomize` updated to the latest git commit
    ```
    kubectl get kustomizations.kustomize.toolkit.fluxcd.io -A
    ```
    3. Observe helm release status
    ```
    kubectl describe helmreleases.helm.toolkit.fluxcd.io hello-kubernetes -n flux-system
    ```
    4. Observe the update via Web at `http://instanceIp:30002`