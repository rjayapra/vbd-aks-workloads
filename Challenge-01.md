## Hands-On Practice: Deploying a Containerized Application to Azure Kubernetes Service (AKS)


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

3. Create a deployment manifest:

    A deployment manifest file is required to deploy your application to Kubernetes cluster. 
    The manifest file allows you to define what type of resource you want to deploy and all the details associated with the workload.
    Kubernetes groups containers into logical structures called pods, which have no intelligence. 
    Deployments add the missing intelligence to create your application. Let's create a deployment file.

    a. Create a deployment.yaml file

    ```
      # deployment.yaml
      apiVersion: apps/v1 # The API resource where this workload resides
      kind: Deployment # The kind of workload we're creating
      metadata:
        name: contoso-website # This will be the name of the deployment
      spec:
        template: # This is the template of the pod inside the deployment
          metadata: # Metadata for the pod
            labels:
              app: contoso-website
    ```

    Pods don't use the same names as the deployments. The pod's name is a mix of the deployment's name with a random ID added to the end.
    Notice the use of the labels key. You add the labels key to allow deployments to find and group pods.
    A pod wraps one or more containers. All pods have a specification section that allows you to define the containers inside that pod.
    Update the deployment.yaml file to match the following YAML.

    ```yaml
    # deployment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: contoso-website
    spec:
      template: # This is the template of the pod inside the deployment
        metadata:
          labels:
            app: contoso-website
        spec:
          containers:
            - image: mcr.microsoft.com/mslearn/samples/contoso-website
              name: contoso-website
              resources:
                requests: # Minimum amount of resources requested
                  cpu: 100m
                  memory: 128Mi
                limits: # Maximum amount of resources requested
                  cpu: 250m
                  memory: 256Mi
    ```

    The containers key is an array of container specifications because a pod can have one or more containers. The specification defines an image, a name, resources, ports, and other important information about the container.
    All running pods follow the name contoso-website-<UUID>, where UUID is a generated ID to identify all resources uniquely.

    It's a good practice to define a minimum and a maximum amount of resources that the app is allowed to use from the cluster. You use the resources key to specify this information.
    Update the deployment.yaml file to match the following YAML.

    Notice how the resource section allows you to specify the minimum resource amount as a request and the maximum resource amount as a limit.
    The last step is to define the ports this container exposes externally through the ports key. The ports key is an array of objects, which means that a container in a pod can expose multiple ports with multiple names.
    Update the deployment.yaml file to match the following YAML.

    ```yml
    # deployment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: contoso-website
    spec:
      template: # This is the template of the pod inside the deployment
        metadata:
          labels:
            app: contoso-website
        spec:
          nodeSelector:
            kubernetes.io/os: linux
          containers:
            - image: mcr.microsoft.com/mslearn/samples/contoso-website
              name: contoso-website
              resources:
                requests:
                  cpu: 100m
                  memory: 128Mi
                limits:
                  cpu: 250m
                  memory: 256Mi
              ports:
                - containerPort: 80 # This container exposes port 80
                  name: http # We named that port "http" so we can refer to it later
    ```
    Notice how you name the port by using the name key. Naming ports allows you to change the exposed port without changing files that reference that port.
    Finally, add a selector section to define the workloads the deployment manages. The selector key is placed inside the deployment specification section of the manifest file. Use the matchLabels key to list the labels for all the pods managed by the deployment.
    Update the deployment.yaml file to match the following YAML.

    ```yaml
    # deployment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: contoso-website
    spec:
      selector: # Define the wrapping strategy
        matchLabels: # Match all pods with the defined labels
          app: contoso-website # Labels follow the `name: value` template
      template: # This is the template of the pod inside the deployment
        metadata:
          labels:
            app: contoso-website
        spec:
          nodeSelector:
            kubernetes.io/os: linux
          containers:
            - image: mcr.microsoft.com/mslearn/samples/contoso-website
              name: contoso-website
              resources:
                requests:
                  cpu: 100m
                  memory: 128Mi
                limits:
                  cpu: 250m
                  memory: 256Mi
              ports:
                - containerPort: 80
                  name: http
    ``` 
    

    > ℹ️ In an AKS cluster which has multiple node pools (Linux and Windows), the deployment manifest file previously listed also defines a nodeSelector to tell your AKS cluster to run the sample application's pod on a node that can run Linux containers. Linux nodes can't run Windows containers and vice versa.

    Save the manifest file.

    b. Run the following command to deploy the application to the AKS cluster:

    ```bash
    kubectl apply -f deployment.yaml
    ```

    c. Run the following command to verify the deployment:

    ```bash
    kubectl get deployments
    ```

    d. Run the following command to get the pods in the deployment:

    ```bash
    kubectl get pods
    ```

    e. Run the following command to get details about a pod:

    ```bash
    kubectl describe pod <pod-name>
    ```
   
