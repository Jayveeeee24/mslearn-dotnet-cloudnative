## KUBERNETES SETUP

# update linux and get certs prereq for kubernetes
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl

# download kubernetes package repo
sudo mkdir /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# add kubernetes repo to apt config
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

# instal kubectl tool
sudo apt-get update
sudo apt-get install -y kubectl

# install the k3d implementation of Kubernetes and create a cluster
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
k3d cluster create devcluster --config k3d.yml

-----------------------------------------------------------------------------------------

## KUBERNETES 
 legend: 
 --watch = persistent for pods
 -f = file
 
 cluster = The total group of all your servers combined
 node = An individual server/machine inside that group, holds the pod inside the cluster
 pod = instance, service, container
 deployment = images, blueprint

# run and apply yml file
kubectl apply -f backend-deploy.yml
kubectl apply -f frontend-deploy.yml

# get images/deployment
kubectl get deployment

# get the actual services/containers
kubectl get pods

# scale a microservice
kubectl scale --replicas=5 deployment/productsbackend

# delete a pod deployed
kubectl delete pod storefrontend-f9b6c6c77-q9xwg

-----------------------------------------------------------------------------
# Azure Kubernetes Service (AKS)

# create the azure kubernetes service (aks)
az aks create \
  --resource-group $RESOURCE_GROUP \
  --name $CLUSTER_NAME \
  --node-count 1 \
  --generate-ssh-keys \
  --node-vm-size standard_b2as_v2 \
  --network-plugin azure \
  --attach-acr $ACR_NAME

# get credentials to aks
az aks get-credentials --name $CLUSTER_NAME --resource-group $RESOURCE_GROUP

# command to check if the new aks can pull images from acr
az aks check-acr --acr $ACR_NAME.azurecr.io --name $CLUSTER_NAME --resource-group $RESOURCE_GROUP

# check status of aks
kubectl get nodes -A

# deploy ingress nginx into the aks
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.3/deploy/static/provider/cloud/deploy.yaml

# check if ingress is ready
kubectl get services --namespace ingress-nginx

# deploy eShop app (FE + BE)
kubectl apply -f deployment.yml

# get deployments (blueprint/image)
kubectl get deployments

# get deployed services (instance/containers)
kubectl get pods

# get ip of the deployed service
echo "http://$(kubectl get services --namespace ingress-nginx ingress-nginx-controller --output jsonpath='{.status.loadBalancer.ingress[0].ip}')"

# monitor service with watch 
kubectl get pods --selector=app=productservice --watch

# remove deployment / rollback
kubectl rollout undo deployment/productservice

# Lists all running container instances inside nginx
kubectl get pods -n ingress-nginx

# Retrieves the complete configuration blueprint and routing rules for the eshop-ingress  
kubectl get ingress eshop-ingress -o yaml

------------------------------------------------------------------------------
### LINKERD

# install linkerd and run pre-checks
curl -sL run.linkerd.io/install | sh
export PATH=$PATH:/home/vscode/.linkerd2/bin
linkerd check --pre

# install linkerd into kubernetes
linkerd install --crds | kubectl apply -f -
linkerd install --set proxyInit.runAsRoot=true | kubectl apply -f -

# validate linkerd deployment
linkerd check

# get list of linkerd components
kubectl -n linkerd get deploy

# Apply the Linkerd service profile for the product service
kubectl apply -f - <<EOF
apiVersion: linkerd.io/v1alpha2
kind: ServiceProfile
metadata:
  name: productsbackend
  namespace: default
spec:
  routes:
  - condition:
      method: GET
      pathRegex: /api/Product
    name: GET /v1/products 
    isRetryable: true
  retryBudget:
    retryRatio: 0.2
    minRetriesPerSecond: 10
    ttl: 120s  
EOF

# install linkerd extension
linkerd viz install | kubectl apply -f -

# view linkerd dashboard
linkerd viz dashboard
