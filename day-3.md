## Kubernetes Pods

### What is Pod?
- Definition of how to run a container, it is one or a group of containers.
- Containers inside same pod can have shared network, shared storage, they can communicate using `localhost`
- To run a docker container, either we use command line or Dockerfile. In K8s, yaml file is used to run the pod.
- Kubenetes wants to standardization and it is declarative.
- Containers inside the pod don't get the IP address, only pod is assigned a cluster ip address.
- Container can be accessed through pod's IP address assigned by kube-proxy.

---

### Kubectl
- Command line for kubernetes.
- Can interact with cluster using `kubectl`.

---

### Demo - Running a cluster & Pod on local, [sample yaml](pod.yaml)

**Installation needed:**
- Install kubectl
- Install minikube(kind in preferred in later stages), `brew install minikube`.
- Install vfkit `brew install vfkit`
 

Start the container(Apple silicon chips have issues with vfkit, hence going with docker here, docker desktop is enough)
```
minikube start --memory=4096 --driver=docker
```

Fetch the node running inside the cluster
```
kubectl get nodes
```

Create a sample pod, [more here](https://kubernetes.io/docs/concepts/workloads/pods/#using-pods)

```
kubectl create -f pod.yaml
```

Fetch the created pod
```
kubectl get pods

# gives more info about the pods

kubectl get pods -o wide 
```

Login into cluster
```
minikube ssh
```

Get the pod status
```
kubectl describe pod nginx
```

Check the pod logs
```
kubectl logs nginx
```