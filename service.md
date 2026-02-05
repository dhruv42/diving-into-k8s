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

```
> kubectl get svc

> kubectl get svc -v=9 # setting verbose level 9 to get more details
```


---

Kubernetes allows you to create service of different kinds, [more here.](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)

- **ClusteIP** - This is the default kind, the service is only accessible inside the cluster. You get load-balancing & service discovery through this kind.

- **Nodeport** -  It will allow the application to be accessible via Node's IP address, not necessarily need to have a cluster IP address.

- **LoadBalancer** - This type of service will expose the application to external world. If a service is created on EKS then the app can be accessed via ELB IP address(a public ip). It only works on cloud providers.

---

### Demo:

Create the docker image using which we are going to run the python app.
 
Create the pods using deployment
```
> kubectl apply -f python-app-deployment.yaml
```

Check if the pods are healthy and running, `-o wide` gives more info about the pods like IP assigned to them
```
> kubectl get pods -o wide
```

Create the service(NodePort type) - Make sure the value in `spec.selector` is same as `template.metadata.label` from deployment. Service manages the pods using the labels and selector and keeping them same creates the link between them.
```
> kubectl apply -f service-nodeport.yaml
```

Get the service info
```
> kubectl get svc

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes           ClusterIP   10.96.0.1      <none>        443/TCP        7d2h
python-app-service   NodePort    10.108.0.136   <none>        90:30007/TCP   16s
```

#### How to access pods from withing the cluster?

Here the Cluster port 90 is mapped to Node port 30007. Pods can be accessed using Node's IP on the cluster port(i.e 10.108.0.136:90) from within the cluster.

```
> minikube ssh
> curl http://10.108.0.136:90
```


#### How to access pods from outside the cluster?

Pods can be accessed from outside via NodePort(minikube)

```
> minikube ip

192.168.49.2
```
App can be accessed on this public ip `192.168.49.2:30007` and on node's port.

> This method might not work for everyone because when minikube is running in the Docker/vfkit/container-based driver, the Node IP is not directly reachable from the host machine.

The other way to access would be to use minikube service, this created the tunnel between host machine and minikube

```
> minikube service python-app-service

│ NAMESPACE │        NAME        │ TARGET PORT │            URL            │
├───────────┼────────────────────┼─────────────┼───────────────────────────┤
│ default   │ python-app-service │ 90          │ http://192.168.49.2:30007 │
└───────────┴────────────────────┴─────────────┴───────────────────────────┘
🏃  Starting tunnel for service python-app-service./┌───────────┬────────────────────┬─────────────┬────────────────────────┐
│ NAMESPACE │        NAME        │ TARGET PORT │          URL           │
├───────────┼────────────────────┼─────────────┼────────────────────────┤
│ default   │ python-app-service │             │ http://127.0.0.1:53624 │
└───────────┴────────────────────┴─────────────┴────────────────────────┘
🏃  Starting tunnel for service python-app-service.
🎉  Opening service default/python-app-service in default browser...
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.
```