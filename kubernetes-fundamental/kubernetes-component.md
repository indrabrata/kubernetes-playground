# Kubernetes Component

## Node

- Is a VM, Host where kubernetes is lying

## Pod

- Pod is a smallest component of kubernetes
- Abstraction over the container
- Usually we run 1 application per pod
- Each pod get their IP address and make the pod can communicate with other pod but must be in same network
- If pod crashed, it will got new ip address
- To connectin pod into deployment, we need to specify the selector in deployment to match the label in pod

## Service

- A permanent IP address that attachedd to each pod with a DNS name
- Lifecycle of service and pod is connected
- And that's why we can just use IP of service not the pod
- The name of service will used for mapping the ip address
- Act also as a load balancing, will forward request to the pod less busy of active pod

### External Service

- Open communication from external service

### Internal Service

- Privately, Not connected to external service

## Ingress

- Ingress is a gateway for forwaring request from external into appropriate service
- Route traffic into cluster
- Placed outside the cluster

## ConfigMap

- ConfigMap usually contains the environment for the application like url database
- Dont put the creds config map
- ConfigMap attached into pod

## Secret

- Same like ConfigMap but used to save confidential data
- Store in base64 encoded format

## Volume

- Attached a physical storage of vm into pod
- Or, it can be remote storage on the outside k8s cluster

## Deployment

- For stateless application
- With deployment we dont need to specify the pod, it's like blueprint
- Abstraction of the pod
- Deployment make us can define how many replica of pod
- Replica connected to same service
- We can't use deployment for replica database because database has it own state (stateful)
- Deployment manages a repliceset, replicaset manages the pods, pods are abstraction of container
- For connecting deployment to service, we need to specify the selector in service to match the label in deployment

## StatefulSet

- Means application such as database for avoiding data incosistency
- DB are often hosted outside kubernetes cluster

## Namespace

- We can organize the resource in kubernetes using namespace
- Virtual cluster inside the kubernetes cluster
- Kubernetes have 4 default namespace
  - Default
    - Create resource at the begining
  - Kube-system (for kubernetes component)
    - Master node component
    - System process
    - Do not create or modify anything in this namespace
  - Kube-public (for public access)
    - public accessible namespace
    - A config map which contains cluster info
  - Kube-node-lease (for node heartbeat)
    - Improve the performance of node heartbeats
  - Kubernetes-dashboard (for dashboard component) -> spesific in minikube
- Benefit of using namespace
  - Multiple environment (dev, staging, production) / blue green deployment
  - Multiple team (team-a, team-b) -> it reduce conflict
  - Resource quota -> we can limit cpu, memory, storage, etc in namespace using resource quota
  - Access control -> we can give access to user in specific namespace using role based access control (RBAC)
- if we want to access the resource in specific namespace, we can use `-n` or `--namespace` flag
  - Example: `kubectl get pods -n <namespace-name>`
- if we want to access component in other namespace we can use [service-name].[namespace-name]
- We cant use namespace for :
  - Node
  - Persistent Volume
  - Storage Class
  - Cluster Role
  - Cluster Role Binding

## Ingress Controller
