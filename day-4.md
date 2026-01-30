## Kubernetes Deployment

#### Why deployment is needed when we have pods?

- A pod doesn't have the capability of auto-healing and auto-scaling, hence we need deployment.
- Do not create the pod directly, create it using the `deployment`.
- When deployment resource is created, it created intermediary resources like `replicaset(controller)`.
- Inside the deployment, replicaset is defined.
- Defining replicaset not only creates the two pods but also maintains this count even if pods go down(auto-healing capability).

*controllers make sure that the desired and actual state of the cluster are same*


### Demo - Creating a deployment on local, [sample yaml](deployment.yaml)

Create the deployment
```
kubectl apply -f deployment.yaml
```

Get the deployment
```
kubectl get deploy

output:- 

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           7s
```

Check the running pods created by deployment,
```
kubectl get pods

output:- Since we have defined replicaset number as 3, it created 3 pods

NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-bf744486c-d5pmm   1/1     Running   0          15s
nginx-deployment-bf744486c-ggqqn   1/1     Running   0          15s
nginx-deployment-bf744486c-hqdvf   1/1     Running   0          15s
```

Watch the pod - Shows what's happening with the pod
```
kubectl get pods -w
```

Get the replicaset
```
kubectl get rs

output:- 

NAME                         DESIRED   CURRENT   READY   AGE
nginx-deployment-bf744486c   3         3         3       21s
```