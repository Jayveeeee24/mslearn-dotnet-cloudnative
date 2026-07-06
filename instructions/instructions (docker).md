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

# build docker image from dockerfile
 cd ./dotnet-docker 
 docker build -t productsbackend:latest -f Products/Dockerfile .

# run docker image into container
docker run -it --rm -p 32001:8080  products

# start the app using docker compose
docker compose up

# login to docker hub
docker login

# tag docker image for push
docker tag store artemisjayvee/storeimage
docker tag products artemisjayvee/productservice

# push docker image to docker hub
docker push artemisjayvee/storeimage
docker push artemisjayvee/productservice