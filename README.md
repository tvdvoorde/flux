# flux

```bash

az provider register --namespace Microsoft.Kubernetes
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.KubernetesConfiguration

az provider show -n Microsoft.Kubernetes -o table
az provider show -n Microsoft.ContainerService -o table
az provider show -n Microsoft.KubernetesConfiguration -o table

az extension add -n k8s-configuration
az extension add -n k8s-extension
az extension update -n k8s-configuration
az extension update -n k8s-extension
az extension list -o table

az feature register --namespace "Microsoft.ContainerService" --name "AKS-GitOps"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-GitOps')].{Name:name,State:properties.state}"
az provider register -n Microsoft.ContainerService

az group create -g flux1 -l westeurope

az aks create -n flux1 -g flux1 

az aks get-credentials -n flux1 -g flux1 --admin

kubectl create configmap test --from-literal=testdata=testvalue -n kube-system

az k8s-configuration flux create -g flux1 \
  -c flux1 \
  -n my-config  \
  --ns my-namespace \
  -t managedClusters \
  -s cluster \
  -u https://github.com/tvdvoorde/flux \
  --kind git \
  --branch main \ 
  --config multiTenancy.enfore=false

az k8s-configuration flux show -g flux1 \
  -c flux1 \
  -n my-config  \
  -t managedClusters 

az extension add --upgrade --name k8s-extension

az k8s-extension update --configuration-settings multiTenancy.enforce=false -c flux1 -g flux1 -n flux -t managedClusters 
  
# https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/conceptual-gitops-flux2#opt-out-of-multi-tenancy
# https://github.com/Azure/azure-cli/issues/27357

k apply -f h1.yaml

k create sa flux-applier -n default

kubectl create clusterrolebinding flux-admin-default --clusterrole=cluster-admin --serviceaccount=default:flux-applier

k apply -f h2.yaml



```