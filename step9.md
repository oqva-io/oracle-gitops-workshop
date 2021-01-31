### Sealed Secrets

#### A Kubernetes controller and tool for one-way encrypted Secrets

Sealed Secrets is composed of two parts:

* A cluster-side controller / operator(oke-day2 included)
* A client-side utility: kubeseal

The kubeseal utility uses asymmetric crypto to encrypt secrets that only the controller can decrypt.

1. Install `kubeseal`
```
wget https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.14.1/kubeseal-linux-amd64 -O kubeseal
chmod +x kubeseal
```

2. At startup, the sealed-secrets controller generates a 4096-bit RSA key pair and persists the private and public keys as Kubernetes secrets in the flux-system namespace.
The public key can be safely stored in Git, and can be used to encrypt secrets without direct access to the Kubernetes cluster.

You can retrieve the public key with:
```
./kubeseal --fetch-cert \
--controller-name=sealed-secrets \
--controller-namespace=flux-system \
> pub-sealed-secrets.pem
```

3. Observe the public key
```
cat pub-sealed-secrets.pem
```

4. Generate a Kubernetes secret manifest
```
kubectl -n default create secret generic basic-auth \
--from-literal=user=admin \
--from-literal=password=change-me \
--dry-run \
-o yaml > basic-auth.yaml
```

5. Observe secret manifest
```
cat basic-auth.yaml
```

6. Encrypt the secret with kubeseal
```
./kubeseal --format=yaml --cert=pub-sealed-secrets.pem \
< basic-auth.yaml > basic-auth-sealed.yaml
```

7. Observe and copy encrypted secret 
```
cat basic-auth-sealed.yaml
```

8. Commit the manifests `basic-auth-sealed.yaml` to a Git repository
    1. Open `oracle-gitops-workshop` repository in your GitHub webpage
    2. Go to `clusters/default/flux-system/` directory
    3. Сlick on `Add file` -> `Create new file`
    4. Fill filename `basic-auth-sealed.yaml` to `Name your file...` field
    4. Copy & Paste `basic-auth-sealed.yaml` content to text area and click on `Commit changes`


9. Observe the secret manifest decrypted inside kubernetes
    1. Go to `http://instanceIp:30000`
    2. Click on `Secrets`
    3. Click on `basic-auth`
    4. Click `password` and `user` eye to see real values