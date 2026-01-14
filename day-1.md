## Problems with docker

1. Single host nature -> Docker runs on a single host with multiple containers, and if any container is killed then the application running inside it won't be reachable.
2. No auto healing -> If the container is killed then it won't start again until someone manually restarts.
3. No Auto scaling -> Docker can not increase or decrease the number of containers automcatically based on the traffic.
4. No enterprise level support -> No firewall, No load balancer, No API gateway available by default for the production ready application

## How K8s is solving these problems?

By default K8s is a cluster(a group of nodes), it is installed in master node arch on prod.

1. If there is any pod in the node creates the problem, k8s will identify and move the other pods into the nodes running in the cluster.
2. K8s controls or fixes the damage -> Whenever the API server receives the signal that the container is going down, it'll spin up the new container.
3. Replica sets(Replication controller) - Set the replicas based on the traffic manually, K8s also support HPA(Horizontal Pods Autoscaller).
4. By default K8s does not support advanced LB capabilities, but it has services and kube-proxy which gives you basic load balancing like round robin, hence K8s introduced CRD(Custom Resource Definition). It is in developing phase.