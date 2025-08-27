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
- Good for loosely coupled application
- Type of service:
  - ClusterIP (default) -> 2 apps running in the same pod
    - Only accessible inside the cluster
    - Service will forward the request to the pod
  - NodePort
    - Open a specific port on each node to access the service from outside the cluster
    - NodePort will forward the request to ClusterIP
    - Port range 30000-32767
  - LoadBalancer
    - Create an external load balancer in the cloud (if supported) and assign a fixed, external IP to the service
    - LoadBalancer will forward the request to NodePort then to ClusterIP
  - ExternalName
    - Map the service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value.
    - No proxying of any kind is set up.
  - Headless Service
    - Communicate with 1 specific pod
    - No Cluster IP
    - We can use headless service for statefulset
    - We can use headless service to sync data between pod (database)

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

- Ingress controller is a component that is used to manage ingress resource
- Ingress controller is to evaluates the ingress rules and make the routing
- Entrypoint for the cluster
- Examples of ingress controller:
  - Nginx Ingress Controller -> from k8s
  - Traefik
  - HAProxy
  - Istio
  - Contour
  - Ambassador

## Helm

- Helm is a package manager for kubernetes
- To package yaml file into helm chart -> push in helm repository -> install the helm chart into kubernetes cluster
- Helm chart is a collection of files that describe a related set of kubernetes resources
- HELM for cleint (helm cli)
- HELM for server (tiller) -> running the `helm install` command in the server -> deperecated in helm v3
- Helm chart have 2 main component:
  - Chart.yaml -> contain the metadata of the chart
  - values.yaml -> contain the default value of the chart
- To install helm chart we can use `helm install <release-name> <chart-name>`
- To upgrade helm chart we can use `helm upgrade <release-name> <chart-name>`
- To rollback helm chart we can use `helm rollback <release-name> <revision-number>`
- To uninstall helm chart we can use `helm uninstall <release-name>`

## Storage

- Storage that doesn't depend on the lifecycle of a pod
- Storage must be available before the pod that uses it is created and available on all nodes
- Storage needs survive even if cluster crashed

## Persistent Volume (PV)

- A piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes
- Storage in kubernetes like an abstraction of physical storage or external plugin to connect to external storage
- Accessible by all namespace
- Volume provides the actual storage backend that is mounted into a pod, allowing containers to read and write data.

## Persistent Volume Claim (PVC)

- A request for storage by a user
- Claim the storage from persistent volume
- PVC is namespace scoped
- container mounted to volume, volume in pod mounted to pvc, pvc claim the storage from pv
- why so many abstraction?
  - decouple the storage from the pod
  - make the pod can use different storage backend without changing the pod configuration
  - make the pod can use different storage size without changing the pod configuration
  - make the pod can use different storage class without changing the pod configuration

## Storage Class

- Create provision and create volume dynamically
- A way to describe the "classes" of storage that a cluster offers
- Different classes might map to quality-of-service levels, or to backup policies, or to arbitrary policies determined by the cluster administrators
- Storage class provide the dynamic provisioning of persistent volume
- Abstract underlying storage provider
- we add this in PVC
- we can specify the storage class in PVC
- If we dont specify the storage class in PVC, it will use the default storage class

## StatefulSet and Volume Claim Template

- Example : database
- Make it able to replicate the database
- We can use statefulset with volume claim template to create a pvc for each pod in statefulset
- Each pod in statefulset will have their own pvc
- If there are 3 pod replica statefulset, there will be 3 PV created (linked to pysical storage)
  - That have master and slave database if we create replicate
- The deletion of statefulset is in reverse order of creation
