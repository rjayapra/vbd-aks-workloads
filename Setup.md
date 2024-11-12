
[< Previous: Prerequisites](./Prerequisites.md) |  [Next: Challenge 01 >](./Challenge-01.md)

## Setup your environment for AKS access
> ⚠️ **Warning:** Ensure the [Prerequisites](/Prerequisites.md) are met before continuing this exercise

### Steps

1. Get Kubernetes cluster credentials:
   - Open the Azure portal and navigate to your AKS cluster.
   - In the overview page , Click on the "Connect" button at the top of the page.
   - Follow the instructions provided to run the `az aks get-credentials` command in your Cloud Shell or local terminal to get the cluster credentials.
   - This adds the cluster configuration to your `~/.kube/config` file, which allows you to connect to the cluster using `kubectl`.

    > ℹ️ If you have created cluster with Azure RBAC enabled, you need to have the required role permissions to access cluster. Follow the steps below to add role.
      - In the left-hand menu, select "Access control (IAM)".
      - Click on "Add" and then "Add role assignment".
      - In the "Role" dropdown, select "Azure Kubernetes Service Cluster User".
      - In the "Assign access to" dropdown, select "User, group, or service principal".
      - In the "Select" field, search for and select your user account.
      - Click "Save" to assign the role.
      - Once the role is assigned, go back to the AKS cluster overview page.

2. Verify connectivity to cluster:
   - Run the following command to verify the connection to the cluster:

      ```bash
        kubectl get nodes
      ```
   - This command lists all the nodes in the cluster. If you see a list of nodes, you have successfully connected to the cluster.

3. Create your namespace:

    Replace <your-initial> before applying this command
    
    ```
        kubectl create namespace <your-initial>dev
    
        eg: kubectl create namespace rjdev
    ```

4. Setup your namespace

    ```bash
    kubectl config set-context --current --namespace=<your-initial>dev
    ```
    For eg; kubectl config set-context --current --namespace=rjdev

    This command sets `rjdev` as the default namespace for your current context, making it easier to work within that namespace without specifying it in each command.

