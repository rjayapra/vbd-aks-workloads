[< Previous: Challenge 04](./Challenge-04.md) | [ Next: Challenge 06 >](./Challenge-06.md)

## Creating Azure Container Registry and Pushing an Image

### 1. Create an Azure Container Registry
```bash
# Set variables
ACR_NAME=aksstoreacr
ACR_RESOURCE_GROUP=aksstore-rg
ACR_LOCATION=eastus

# Create resource group
az group create --name $ACR_RESOURCE_GROUP --location $ACR_LOCATION

# Create Azure Container Registry
az acr create --resource-group $ACR_RESOURCE_GROUP --name $ACR_NAME --sku Basic --location $ACR_LOCATION
```

### 2. Log in to Azure Container Registry
```bash
az acr login --name $ACR_NAME
```

### 3. Import the Image from GitHub Container Registry to Azure Container Registry
```bash
az acr import -g $ACR_RESOURCE_GROUP --name $ACR_NAME --source ghcr.io/azure-samples/aks-store-demo/product-service:latest --image $ACR_NAME.azurecr.io/aks-store-demo/product-service:latest
az acr import -g $ACR_RESOURCE_GROUP --name $ACR_NAME --source ghcr.io/azure-samples/aks-store-demo/order-service:latest --image $ACR_NAME.azurecr.io/aks-store-demo/order-service:latest
az acr import -g $ACR_RESOURCE_GROUP --name $ACR_NAME --source ghcr.io/azure-samples/aks-store-demo/store-front:latest --image $ACR_NAME.azurecr.io/aks-store-demo/store-front:latest
```

### 4. Verify the Image in Azure Container Registry
```bash
az acr repository list --name $ACR_NAME --output table
```
You have now successfully created an Azure Container Registry and pushed the image to the repository

### 5. Assign permission for AKS cluster to access the image

To allow your AKS cluster to pull images from your Azure Container Registry, you need to assign the `ACRPull` role to the managed identity of your AKS cluster.

```bash
# Get the resource ID of your ACR
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Get the managed identity of your AKS cluster
AKS_MANAGED_IDENTITY=$(az aks show --resource-group <your-aks-resource-group> --name <your-aks-cluster-name> --query "identityProfile.kubeletidentity.clientId" --output tsv)

# Assign the ACRPull role to the managed identity
az role assignment create --assignee $AKS_MANAGED_IDENTITY --role acrpull --scope $ACR_ID
```

Replace `<your-aks-resource-group>` and `<your-aks-cluster-name>` with the appropriate values for your AKS cluster.

You have now successfully assigned the `ACRPull` role to the managed identity of your AKS cluster, allowing it to pull images from your Azure Container Registry.


### 5. Run the deployments

Now replace the image name in [aks-store-quickstart.yaml](./config/aks-store-quickstart.yaml) to use your container registry
```
image: ghcr.io/azure-samples/aks-store-demo/order-service:latest
to 
image: <registry-name>/aks-store-demo/order-service:latest


image: ghcr.io/azure-samples/aks-store-demo/product-service:latest
to
image: <registry-name>/aks-store-demo/product-service:latest


image: ghcr.io/azure-samples/aks-store-demo/store-front:latest
to
image: <registry-name>/aks-store-demo/store-front:latest
```

Redeploy the app:
```
kubectl apply -f aks-store-quickstart.yaml
```

Validate the application status.
