# Kubernetes Command

## Create Deployment

```bash
kubectl create deployment <deployment-name> --image=<image-name>:<tag>
```

## Get Pods

```bash
kubectl get pods
```

## Get Services

```bash
kubectl get services
```

## Get Deployments

```bash
kubectl get deployments
```

## Get Nodes

```bash
kubectl get nodes
```

## Get ReplicaSets

- ReplicaSets are used to ensure that a specified number of pod replicas are running at any given time. They can be created automatically by a Deployment.

```bash
kubectl get replicasets
```

## Edit Deployment

```bash
kubectl edit deployment <deployment-name>
```

## Get Pod Logs

```bash
kubectl logs <pod-name>
```

## Describe Pod

```bash
kubectl describe pod <pod-name>
```

## Delete Pod

```bash
kubectl delete pod <pod-name>
```

## Execute Command in Pod

```bash
kubectl exec -it <pod-name> -- <command>
```

## Apply Configuration from File

```bash
kubectl apply -f <file-name>.yaml
```

## Create Namespace

```bash
kubectl create namespace <namespace-name>
```

## Get Namespaces

```bash
kubectl get namespaces
```
