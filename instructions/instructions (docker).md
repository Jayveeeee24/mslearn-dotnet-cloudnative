# publish the dotnet project into a container

cd ./dotnet-docker 
 dotnet publish /p:PublishProfile=DefaultContainer

## DOCKER
 legend: 
 -t = tag (name)
 -f = file (what file)
 --rm = automatically remove
 -it = interactive and terminal
 -p = port

# get docker images list
 docker images

# list running containers
 docker ps

# build docker image from dockerfile
 cd ./dotnet-docker 
 docker build -t productsbackend:latest -f Products/Dockerfile .

# run docker image into container
docker run -it --rm -p 32001:8080  products

# start the containers in compose
docker compose up

# stop running containers in compose
docker compose down

# login to docker hub
docker login

# tag docker image for push
docker tag store artemisjayvee/storeimage
docker tag products artemisjayvee/productservice

# To create alias for local image to push in acr
docker tag store $ACR_NAME.azurecr.io/storeimage:v1
docker tag products $ACR_NAME.azurecr.io/productservice:v1

# push docker image to docker hub
docker push artemisjayvee/storeimage
docker push artemisjayvee/productservice

# stop a running container
docker stop 1aa78c737318