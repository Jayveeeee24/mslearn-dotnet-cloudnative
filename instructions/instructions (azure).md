# Azure

# login in cli via code
az login --use-device-code

# view selected subscription in a table
az account show -o table

# List all locations and code
az account list-locations -o table

# To get all the available regions applicable
Azure Portal > Policy > Assignments > Allowed resource deployment regions 

# To make a variable-like for use later
export LOCATION=southeastasia
export RESOURCE_GROUP=rg-eshop
export CLUSTER_NAME=aks-eshop
export ACR_NAME=acseshop$SRANDOM
export CONFIG_NAME=eshop-app-features$SRANDOM

# To create resource group
az group create --name $RESOURCE_GROUP --location $LOCATION

# To create app config instance
az appconfig create --resource-group $RESOURCE_GROUP --name $CONFIG_NAME --location $LOCATION --sku Free

# To retrieve connection string for app config
az appconfig credential list --resource-group $RESOURCE_GROUP --name $CONFIG_NAME --query [0].connectionString --output tsv

# To create Azure Container Registry (ACR)
az acr create --resource-group $RESOURCE_GROUP --name $ACR_NAME --sku Basic

# To login into acr
az acr login --name $ACR_NAME

# To create alias for local image to push in acr
docker tag store $ACR_NAME.azurecr.io/storeimage:v1
docker tag products $ACR_NAME.azurecr.io/productservice:v1

## push the actual image into acr
docker push $ACR_NAME.azurecr.io/storeimage:v1
docker push $ACR_NAME.azurecr.io/productservice:v1

-------------------------------------------------------------------------

# get and save subcription id
export SUBS=$(az account show --query 'id' --output tsv)

# create rbac to allow access in github
az ad sp create-for-rbac --name "eShop" --role contributor --scopes /subscriptions/$SUBS/resourceGroups/$RESOURCE_GROUP --json-auth

# actual acr name
export ACR=acseshop1129911899
# show current versions of product service stored in acr
az acr repository show-tags -n $ACR --repository productservice --orderby time_desc --output table
