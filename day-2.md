## Kubernetes Architechture


```
                    ┌──────────────────────────┐
                    │      Control Plane       │
                    │   (Master Components)    │
                    │                          │
                    │  ┌──────────────┐        │
External Requests ─▶│  │  API Server  │ ◀──────┐
                    │  └──────────────┘        │
                    │        │                 │
                    │  ┌──────────────┐        │
                    │  │  Scheduler   │        │
                    │  └──────────────┘        │
                    │        │                 │
                    │  ┌──────────────┐        │
                    │  │ Controllers  │        │
                    │  └──────────────┘        │
                    │        │                 │
                    │  ┌──────────────┐        │
                    │  │     etcd     │        │
                    │  └──────────────┘        │
                    │        │                 │
                    │  ┌──────────────┐        │
                    │  │     CCM      │        │
                    │  └──────────────┘        │
                    └──────────┬───────────────┘
                               │
                               ▼
                ┌────────────────────────────────┐
                │            Data Plane          │
                │          (Worker Nodes)        │
                │                                │
                │  ┌──────────┐  ┌──────────┐    │
                │  │ Kubelet  │  │ KubeProxy│    │
                │  └──────────┘  └──────────┘    │
                │          │          |          │
                │   ┌──────────────────────┐     │
                │   │ Container Runtime    │     │
                │   │ (containerd/CRI-O)   │     │
                │   └──────────────────────┘     │
                │          │                     │
                │        Pods                    │
                └────────────────────────────────┘
```
<hr>

### Control Plane(master node)
- API server
- etcd
- scheduler
- controller manager
- ccm(cloud controller manager)

### Data plane(worker node)
- kubelet
- kube proxy
- container runtime 

## Data Plane - K8s vs Docker 

### **Docker** 

Container Runtime Interface 

- To run the container in docker it needs to have container runtime, without it the container never runs.
- The container runtime component is called Dockershim.

Networking
- There's something called as docker zero or bridge networking in docker.

### **K8s**

### Kubelet

- Pod is the smallest deployable unit similiar to container in docker.
- _Kubelet_ is responsible for creating and running the pod. If the pod is not in the running state then it takes necessary action using k8s control plane.

### Kubeproxy - All Networking

- Kubeproxy provides networking capabilities. 
- Every pod that gets created has an IP address which is provided by kubeproxy and it comes with the default load balancing capabilities. Kubeproxy is responsible for the load balancing as well.


### Container Runtime Interface 
- To run the pod, container runtime is also needed. In K8s CRI(container runtime interface) could be Dockershim, Containerd, Cri-o, but in docker only dockershim can be used.


## Control Plane

### API server
- API server is the most important component of the control plane. 
- API server decides what to do.
- It is responsible for taking the requests from the external systems and also exposing the k8s cluster to the external systems.

### Scheduler
- It is responsible for scheduling the pods or other resources on k8s.
- Scheduler takes actions on the decides tasks by the API server.

### Etcd - The key value store
- Etcd is the component which is responsible for taking backups.
- It stores the cluster information in the key value format.

### Controller manager
- When autoscalling has to be done, replicaset is used and it is one of the controllers that makes sure that the correct number of replicas are running.
- There are multiple controllers like replicaset.
- And the component which makes sure that these controllers are running is called controller manager

### CCM - Cloud Controller Manager
- K8s can be run on EKS by AWS, AKS by Azure, GKE by GCP.
- If a user needs the components like load balancer, storage capabilities on EKS or AKS or GKE then k8s has to understand the underlying cloud provider.
- K8s has to translate the request from the user and convert it into API standard provided by the cloud provider.
- CCM does the translation in K8s. It is an open source utility and a wrapper.
- Not needed for on-premise setup.


