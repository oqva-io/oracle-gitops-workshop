### GitHub setup
1. Create or Login to your GitHub account
2. Fork https://github.com/oqva-io/oracle-gitops-workshop to your GitHub account

### Provision `oracle-gitops-meetup` components

1.  Add `oracle-gitops-meetup` repository url as git source 
```
# example: https://github.com/dima/oracle-gitops-workshop
export GITHUB_REPO=<<URL_TO_GIGHUT_REPOSITORY>>
```
```
./flux create source git oracle-gitops-workshop \
--url=${GITHUB_REPO} \
--branch=master \
--interval=30s
```

3. Observe `oracle-gitops-meetup` as flux git source
```
kubectl get gitrepositories.source.toolkit.fluxcd.io -A
```

4. Provision `hello-kubernetes` application
```
./flux create kustomization oracle-gitops-workshop \
--source=oracle-gitops-workshop \
--path="./clusters/default/flux-system" \
--prune=true \
--interval=30s
```

5. Observe `kustomize` updated to the latest git commit
```
kubectl get kustomizations.kustomize.toolkit.fluxcd.io -A
```

6. Observe `hello-kubernetes` Helm charts synced
```
kubectl get helmcharts.source.toolkit.fluxcd.io -A
```

7. Observe helm chart installed
```
kubectl get helmreleases.helm.toolkit.fluxcd.io -A
```

8. Verify all resources are ready
   1. All pods in `ready` state
    ```
    kubectl get pod
    ```
   2. Go to [Instances](https://cloud.oracle.com/compute/instances)
   3. Copy Instance `Public IP` (any instance)
   4. Test our demo application at `http://instanceIp:30002`