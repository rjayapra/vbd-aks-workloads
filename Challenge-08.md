[ < Previous: Challenge 07 ](./Challenge-07.md)

## Installing NGINX Ingress Controller and Creating Ingress Resource for AKS-Store Demo Application

## Step 1: Install NGINX Ingress Controller

1. Add the NGINX ingress controller Helm repository:
    ```sh
    helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
    ```

2. Update your Helm repositories:
    ```sh
    helm repo update
    ```

3. Install the NGINX ingress controller using Helm:
    ```sh
    helm install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress-basic --create-namespace
    ```

## Step 2: Create Ingress Resource for AKS-Store Demo Application

1. Create a YAML file named `ingress.yaml` with the following content: Replace <namespace> with your namespace value
    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: store-front
      namespace: <namespace>
    spec:
      ingressClassName: nginx
      rules:
      - http:
          paths:
          - backend:
              service:
                name: store-front
                port:
                  number: 80
            path: /
            pathType: Prefix
    ```

2. Apply the ingress resource:
    ```sh
    kubectl apply -f ingress.yaml
    ```

## Step 3: Verify the Ingress

1. Get the external IP address of the NGINX ingress controller:
    ```sh
    kubectl get ingress -w
    ```
    Now you can access the application with the public IP address directly

Additional Challenge: 

- Update your DNS to point `aks-store.example.com` to the external IP address obtained in the previous step.

- Access your application using the URL `http://aks-store.example.com`.
