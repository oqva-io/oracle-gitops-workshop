### Pause/Resume features

1. Pause git repository hello-kubernetes reconcile
```
./flux suspend source git oracle-gitops-workshop
```

2. Set watching for pods
```
watch kubectl get pod
```

3. Update `message` parameter
   1. Open `oracle-gitops-workshop` repository in your GitHub webpage
   2. Go to `clusters/default/flux-system/hello-kubernetes.yaml` file
   3. Сlick on pencil to edit the file
   4. Under the `values` section update the `message` parameter and click on `Commit changes`
   5. Observe the `Latest commit` hash value

4. Observe pods are not updated (section 2)

5. Observe latest `Fetched revision` is not changed
```
kubectl get gitrepositories.source.toolkit.fluxcd.io -A
```

6. Resume reconcile
```
./flux resume source git oracle-gitops-workshop
```

7. Observe pods are updated
```
watch kubectl get pod
```

8. Observe the update via Web at `http://instanceIp:30002`
