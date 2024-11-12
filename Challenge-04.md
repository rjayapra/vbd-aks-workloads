[< Previous: Challenge 03](./Challenge-03.md) 
## Troubleshooting Workloads in AKS

## Getting Pod Labels
To get the labels of all pods in a specific namespace, use the following command:
```sh
kubectl get pods --namespace <namespace> --show-labels
```

## Checking Resources in a Namespace
To check all resources in a specific namespace, use:
```sh
kubectl get all --namespace <namespace>
```

## General Troubleshooting Commands

### Describe a Pod
To get detailed information about a specific pod:
```sh
kubectl describe pod <pod-name> --namespace <namespace>
```

### Check Pod Logs
To view the logs of a specific pod:
```sh
kubectl logs <pod-name> --namespace <namespace>
```

### Check Events
To view recent events in a specific namespace:
```sh
kubectl get events --namespace <namespace>
```

### Check Node Status
To get the status of all nodes in the cluster:
```sh
kubectl get nodes
```

### Check Deployment Status
To get the status of a deployment:
```sh
kubectl rollout status deployment/<deployment-name> --namespace <namespace>
```

### Debugging a Pod
To start a debugging session in a pod:
```sh
kubectl exec -it <pod-name> --namespace <namespace> -- /bin/bash
```

## Additional Resources
- [AKS Troubleshooting Guide](https://docs.microsoft.com/en-us/azure/aks/troubleshooting)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
