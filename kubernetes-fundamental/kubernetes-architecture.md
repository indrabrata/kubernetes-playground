# Kubernetes Architecture

## Node

- Each node will have multiple pods running on it
- This process can be done using 3 process that installed in every node
- Worker node do the actual work

### Worker Node

- Container runtime (to run the container)
- Kubelet (interact with container runtime and node)
  - Kubelet has responsible to running the pod inside the node
- Kube Proxy (forward request make sure the performance in low overhead)

### Master Node

- API Server
  - Cluster gateway
  - ACTs as a gatekeeper for the authentication
- Scheduler (Scheduler to crate a pod in one of worker node)
  - Scheduler will lookup in the most resource node available to put the pod
- Controller manager
  - Detecs cluster state change
  - If some of pod are die, controller manager will make request to scheduler and scheduler make request to kubelet
- etcd
  - Key value store (cluster change saved on etcd)
  - Cluster brain
  - Store cluster information to make decision about process later
  - Store all the configuration and state of the cluster
