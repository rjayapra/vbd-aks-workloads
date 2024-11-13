[< Previous: Challenge 05](./Challenge-05.md) | [ Next: Challenge 06 >](./Challenge-06.md)

## Creating a Horizontal Pod Autoscaler in AKS

To create a Horizontal Pod Autoscaler (HPA) in your AKS cluster using the `config/hpa.yaml` file, follow these steps:

1. **Apply the HPA configuration**:
    Use the following command to create the HPA using the `config/hpa.yaml` file:
    ```sh
    kubectl apply -f config/hpa.yaml
    ```
2. **Verify the HPA creation**:
    Check the status of the HPA to ensure it has been created successfully:
    ```sh
    kubectl get hpa
    ```

3. **Monitor the HPA**:
    You can describe the HPA to get more details and monitor its status:
    ```sh
    kubectl describe hpa <hpa-name>
    ```

Replace `<hpa-name>` with the name of your Horizontal Pod Autoscaler.

If you are running a load test on the store-front app, you can notice that the autoscaler increases the number of replicas automatically