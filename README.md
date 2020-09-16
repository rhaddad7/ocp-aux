The following is based on:

https://www.openshift.com/blog/getting-started-helm-openshift
https://docs.o-ran-sc.org/projects/o-ran-sc-it-dep/en/latest/installation-guides.html#prerequisites
https://docs.openshift.com/container-platform/4.5/networking/using-sctp.html

## Prerequisites

### Workstation/Client setup
Before you begin you will need to have helm2 installed on your machine.  Here are instructions for different platforms:

### For OSX
```
brew install helm@2
brew install helm
cd /usr/local/bin
ln -s /usr/local/opt/helm@2/bin/tiller tiller
ln -s /usr/local/opt/helm@2/bin/helm helm
ln -s /usr/local/opt/helm@3/bin/helm helm3
```
### For Linux jumphost
Download the latest Helm v2 package from https://github.com/helm/helm/releases. Example: https://get.helm.sh/helm-v2.16.10-linux-amd64.tar.gz
Run the following commands:
```
tar -zxvf helm-v2.16.10-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
```
### Optional - Using Ceph for persistent storage
If you would like to use OpenShift Container Storage (Ceph) for persistent storage, please follow the instructions in https://github.com/xphyr/ocp-ric/blob/master/persistent_storage.md

## Install Tiller

```
oc login <>
oc new-project tiller
export TILLER_NAMESPACE=tiller
helm init --client-only
oc process -f tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -p HELM_VERSION=v2.16.10 | oc create -f -
# wait a minute ish for helm to rollout
helm version
# the output should show both client and server versions

# This allows tiller full access to the cluster, which appears to be needed by the ric deployment
oc adm policy add-cluster-role-to-user cluster-admin "system:serviceaccount:${TILLER_NAMESPACE}:tiller"
```

Helm is now ready.  We will deploy AUX in a different namespace

### Deploy the AUX

The AUX installer requires that nodes be labeled to deploy the components if you are using local-storage. If you are using a different storage class, be sure to update that in the ../RECIPE_EXAMPLE/AUX/example_recipe.yaml before running the install.

```
patches/apply-aux-patch.sh
dep/bin/deploy-ric-aux dep/RECIPE_EXAMPLE/AUX/example_recipe.yaml
```
## Cleanup

dep/bin/undeploy-ric-aux

# Scratchspace
