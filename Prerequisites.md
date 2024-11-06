# Prerequisites for Creating and Working on an AKS Cluster

## 1. Azure Subscription
- Ensure you have an active Azure subscription. If you don't have one, you can create a free account at [Azure Free Account](https://azure.microsoft.com/free/).

## 2. Azure CLI
- Install the Azure CLI. You can download and install it from [Azure CLI Installation](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## 3. AKS CLI
- Install the AKS CLI extension by running the following command:
    ```sh
    az extension add --name aks-preview
    ```

## 4. kubectl
- Install `kubectl`, the Kubernetes command-line tool. You can install it using the Azure CLI:
    ```sh
    az aks install-cli
    ```

## 5. Docker ( needed only if there are containers created locally)
- Install Docker on your local machine. You can download and install it from [Docker Installation](https://docs.docker.com/get-docker/).

## 6. Helm
- Install Helm, the Kubernetes package manager. You can download and install it from [Helm Installation](https://helm.sh/docs/intro/install/).

## 7. Git
- Ensure Git is installed on your local machine. You can download and install it from [Git Installation](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## 8. Text Editor or IDE
- Choose a text editor or IDE for editing your Kubernetes manifests and other configuration files. Popular choices include:
    - Visual Studio Code
    - IntelliJ IDEA
    - Sublime Text

## 9. Network Configuration
- Ensure your network configuration allows communication between your local machine and the Azure resources.

## 10. Resource Group
- Create a resource group in Azure where your AKS cluster will reside:
    ```sh
    az group create --name myResourceGroup --location eastus
    ```

## 11. AKS Cluster
- Create an AKS cluster with a system pool and a user agent pool using the Azure CLI:
    ```sh
    az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys --nodepool-name systempool --nodepool-labels agentpool=system --nodepool-mode System

    az aks nodepool add --resource-group myResourceGroup --cluster-name myAKSCluster --name userpool --node-count 2 --nodepool-labels agentpool=user --nodepool-mode User
    ```
## 12. Managed Identity
- Use a managed identity for AKS to interact with other Azure resources. 
    ```sh
    az aks update --resource-group myResourceGroup --name myAKSCluster --enable-managed-identity
    ```

## 13. Connect to AKS Cluster
- Connect to your AKS cluster using `kubectl`:
    ```sh
    az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
    ```

## 14. Verify Connection
- Verify the connection to your AKS cluster:
    ```sh
    kubectl get nodes
    ```

Now you should be ready to work on the AKS cluster created.
