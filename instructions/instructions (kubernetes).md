# KUBERNETES SETUP

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

## KUBERNETES 
 legend: 
 --watch = persistent for pods
 -f = file
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