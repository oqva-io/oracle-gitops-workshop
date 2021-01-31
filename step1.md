### Provision OKE cluster

1. Create Kubernetes cluster
    1. Go to [Clusters](https://cloud.oracle.com/containers/clusters)
    2. Click `Create Cluster`
    3. Choose `Quick create` and click `Launch Workflow`
    4. Choose `Public` Visibility Type and click `Next`
    5. Review and click `Create Cluster`
2. Set your public IP to nodes security list
    1. Copy your public IP (google: `show my ip`)
    2. Go to [VCNs](https://cloud.oracle.com/networking/vcns)
    3. Choose cluster `VCN`
    4. Go to `Security Lists`
    5. Choose `oke-seclist-quick-cluster1-*********`  Security List
    6. Click `Edit` at rule with `Destination Port Range` = `30000-32767`.
    7. Put your public IP in `SOURCE CIDR` field (example: `78.78.78.78/32`) and save
3. Access to cluster
    1. Go to [Clusters](https://cloud.oracle.com/containers/clusters)
    2. Choose your cluster
    3. Click `Access Cluster` -> `Launch Cloud Shell` (Step 1)
    4. Copy `oci` command to `Cloud Shell` (Step 2)
   
   *Presentation ~3-5 min until nodes are ready* 