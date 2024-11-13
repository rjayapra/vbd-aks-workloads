# Creating and Managing a Helm Chart

## Prerequisites
- Helm installed on your local machine
- Kubernetes cluster access

## Step 1: Create a Helm Chart
1. Open your terminal.
2. Run the following command to create a new Helm chart:
    ```sh
    helm create mychart
    ```
3. Navigate to the chart directory:
    ```sh
    cd mychart
    ```

## Step 2: Version the Helm Chart
1. Open the `Chart.yaml` file in your Helm chart directory.
2. Update the `version` field to the desired version number:
    ```yaml
    version: 0.2.0
    ```

## Step 3: Deploy the Helm Chart
1. Run the following command to deploy the Helm chart to your Kubernetes cluster:
    ```sh
    helm install myrelease ./mychart
    ```
    - `myrelease` is the name of your release.
    - `./mychart` is the path to your chart directory.

## Step 4: Rollback a Revision
1. List the revisions of your release:
    ```sh
    helm history myrelease
    ```
2. Rollback to a specific revision:
    ```sh
    helm rollback myrelease <revision>
    ```
    - Replace `<revision>` with the desired revision number.

## Step 5: Update Values
1. Open the `values.yaml` file in your Helm chart directory.
2. Modify the values as needed. For example:
    ```yaml
    replicaCount: 3
    image:
      repository: nginx
      tag: 1.19.0
    ```
3. Upgrade the release with the updated values:
    ```sh
    helm upgrade myrelease ./mychart
    ```

## Additional Commands
- To uninstall a release:
    ```sh
    helm uninstall myrelease
    ```
- To check the status of a release:
    ```sh
    helm status myrelease
    ```
