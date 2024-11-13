[< Previous: Challenge 06](./Challenge-06.md) | [ Next: Challenge 07 >](./Challenge-07.md)

## Creating and Managing a Helm Chart

## Prerequisites
- Helm installed on your local machine
- Kubernetes cluster access

## Step 1: To Create a new Helm Chart
1. Open your terminal.
2. Run the following command to create a new Helm chart:
    ```sh
    helm create mychart
    ```
3. Navigate to the chart directory:
    ```sh
    cd mychart
    ```
4. Version the Helm Chart
   - Open the `Chart.yaml` file in your Helm chart directory.
   - Update the `version` field to the desired version number:
    ```yaml
      version: 0.2.0
    ```

5. Deploy the Helm Chart
   - Run the following command to deploy the Helm chart to your Kubernetes cluster:
        ```sh
        helm install myrelease ./mychart
        ```
        - `myrelease` is the name of your release.
        - `./mychart` is the path to your chart directory.

6. Rollback a Revision
 - List the revisions of your release:
    ```sh
    helm history myrelease
    ```
 - Rollback to a specific revision:
    ```sh
    helm rollback myrelease <revision>
    ```
    - Replace `<revision>` with the desired revision number.

7. Update Values
 - Open the `values.yaml` file in your Helm chart directory.
 - Modify the values as needed. For example:
    ```yaml
    replicaCount: 3
    image:
      repository: nginx
      tag: 1.19.0
    ```
 - Upgrade the release with the updated values:
    ```sh
    helm upgrade myrelease ./mychart
    ```

8. Additional Commands
    - To uninstall a release:
        ```sh
        helm uninstall myrelease
        ```
    - To check the status of a release:
        ```sh
        helm status myrelease
        ```

## Step 2: To setup aks store application

1. Copy the [aks-store-demo](./charts/aks-store-demo/) folder
2. Update values.yaml file for the namespace and registry information
3. Install the chart
    ```
    helm install aksstore .\aks-store-demo\
    ```
4. List chart
    ```
    helm list
    ```
5. Helm lint is a tool for examining a Helm chart and validating its structure and contents.
    ```
    In this case, lint on the 'aks-store-demo' directory to ensure that the Helm chart is well-formed and follows best practices.
    
    helm lint .\aks-store-demo\ 
    ```
6. Check history of versions deployed
    ```
    helm history aksstore
    ```
7. Make changes to any template file and update the Chart.yaml file for the Chart version and app version. Now upgarde the deployment
    ```
    helm upgrade aksstore .\aks-store-demo
    ```
8. Rollback to previous revision (1)
    ```
    helm rollback aksstore 1
    ```
9. Check history
    ```
    helm history aksstore
    ```
    This should reflect the old version deployed
    ```
    REVISION        UPDATED                         STATUS          CHART                           APP VERSION     DESCRIPTION     
    1               Tue Nov 12 20:04:22 2024        superseded      aks-store-demo-chart-1.0.0      1.16.0          Install complete
    2               Tue Nov 12 20:10:52 2024        superseded      aks-store-demo-chart-1.1.0      1.16.1          Upgrade complete
    3               Tue Nov 12 20:20:48 2024        deployed        aks-store-demo-chart-1.0.0      1.16.0          Rollback to 1
    ```
10. To uninstall
    ```
    helm uninstall aksstore
    ```
    
    
    
    
    
   
   

    
    
    
    