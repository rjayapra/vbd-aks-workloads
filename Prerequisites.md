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

## 5. Docker (needed only if new container images need to be created)
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

## 9. Azure Service Principal
- Create an Azure Service Principal for AKS to interact with other Azure resources. You can create it using the Azure CLI:
    ```sh
    az ad sp create-for-rbac --skip-assignment
    ```

## 10. Network Configuration
- Ensure your network configuration allows communication between your local machine and the Azure resources.

## 11. Resource Group
- Create a resource group in Azure where your AKS cluster will reside:
    ```sh
    az group create --name myResourceGroup --location eastus
    ```

## 12. AKS Cluster
- Create an AKS cluster using the Azure CLI:
    ```sh
    az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
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

By following these prerequisites, you will be ready to create and work on an AKS cluster.
