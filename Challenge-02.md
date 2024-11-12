
[< Previous: Challenge 01](./Challenge-01.md) | [Next: Challenge 03 >](./Challenge-03.md)

## Creating a Kubernetes Service of Type LoadBalancer

> ⚠️ **Warning:** Ensure the [Challenge-01](/Challenge-01.md) is complete before continuing this exercise

1. Navigate to your working directory:
    ```sh
    cd <path>
    ```

2. **Create a service configuration file:**
    Create a new file named `service-lb.yaml` with the following content:

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: contoso-service
    spec:
      selector:
         app: contoso-website
      ports:
         - protocol: TCP
            port: 80
            targetPort: 80
      type: LoadBalancer
    ```

3. **Apply the service configuration:**
    Use the `kubectl apply` command to create the service in your Kubernetes cluster.
    ```sh
    kubectl apply -f service-lb.yaml
    ```

4. **Verify the service creation:**
    Check the status of the service to ensure it has been created successfully.
    ```sh
    kubectl get services
    ```

5. **Get the external IP:**
    Once the service is created, it may take a few minutes for the external IP to be assigned. You can check the external IP using:
    ```sh
    kubectl get service contoso-service
    ```

6. **Access your application:**
    Use the external IP address to access your application in a web browser.

