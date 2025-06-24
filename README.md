# openshift-cluster-gitops

## Instructions

### Login

Either login using the following cli command or use the browser
```shell
oc login --server=https://api.clustername.domain:6443 -u kubeadmin -p <password>
```

### Install OpenShift Gitops 

```shell 
./install-openshift-gitops.sh
```

### Add Secret

Add Credentials for the cloned Github respository containing these manifests to the `cluster-gitops` namespace

```shell
oc apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: openshift-cluster-gitops-credentials
  namespace: cluster-gitops
  labels:
    argocd.argoproj.io/secret-type: repository
type: Opaque
stringData:
  url: https://github.com/<org>/openshift-cluster-gitops.git
  username: <username>
  password: <github-pat>
  name: openshift-cluster-gitops
  project: cluster
EOF
```

```shell
oc apply -f openshift-cluster-gitops-application.yaml
```