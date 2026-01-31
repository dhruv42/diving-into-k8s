## Kubernetes Service

The functionalities provide by service,
- Load balancing
- App gateway
- Service discovery

### Load Balancing & App Gateway
- Service is a method for exposing an application that is running as a one or multiple pods in the cluster.

- It also provides the `load balancing` capabilities.
- Deployment creates the pods using the RS controller, but service makes them accessible to outside world.

- **For example:-** If there are two pods with 2 diff IPs and each one is accessed via some internal application, when any of the pod goes down it gets a new IP address and the internal app can't access it because of the change in IP, this is where service comes into the picture by creating an abstraction over pods, now the internal apps can access the pods via service.

> Internal Application -> Service -> Pods

### Service Discovery

- Service would face the same issue of change in the IP address while maintaning the desired replica count.
- Service does not achieve this by keeping track of IP addressed, it uses `Lables and Selectors`.
- Every new pod that comes up or goes down has the same lable.

---

Kubernetes allows you to create service of different kinds, [more here.](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)

- **ClusteIP** - This is the default kind, the service is only accessible inside the cluster. You get load-balancing & service discovery through this kind.

- **Nodeport** -  It will allow the application to be accessible via Node's IP address, not necessarily need to have a cluster IP address.

- **LoadBalancer** - This type of service will expose the application to external world. If a service is created on EKS then the app can be accessed via ELB IP address(a public ip). It only works on cloud providers.
