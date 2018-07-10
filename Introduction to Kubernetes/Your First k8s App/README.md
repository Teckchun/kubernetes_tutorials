<img src="https://github.com/kubernetes/kubernetes/raw/master/logo/logo.png" width="100">

----

Kubernetes is an open source system for managing [containerized applications]
across multiple hosts; providing basic mechanisms for deployment, maintenance,
and scaling of applications.

Kubernetes builds upon a decade and a half of experience at Google running
production workloads at scale using a system called [Borg],
combined with best-of-breed ideas and practices from the community.

Kubernetes is hosted by the Cloud Native Computing Foundation ([CNCF]).
If you are a company that wants to help shape the evolution of
technologies that are container-packaged, dynamically-scheduled
and microservices-oriented, consider joining the CNCF.
For details about who's involved and how Kubernetes plays a role,
read the CNCF [announcement].

----
## Deploy TOMCAT Server:
### Define the deployment:
* the most simple deployment in Kubernetes is a single pod 
* a pod is the instance of a docker container
* deployment can have any number of pods required to get the job done, they can range from one instance of a single image and grows to hundreds of images of different types, the most simple deployment is "the single pod" no redundancy , no separation services. Just one pod, deploy to kubenetes.

1. Open the deployment.yaml file
* the goal of deployment.yaml file is to provide Kubernetes with the basic information of what the application needs to do, the resources required to accompilsh it

```
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: tomcat-deployment
spec:
  selector:
    matchLabels:
      app: tomcat
  replicas: 1
  template:
    metadata:
      labels:
        app: tomcat
    spec:
      containers:
      - name: tomcat
        image: tomcat:9.0
        ports:
        - containerPort: 8080  


```
* the import things to note are:
    * image name
    * number of replica 
    * the container port
    * name of the application

## we will use the apply command to take the directives from this file and apply it to our cluster:
```
kubectl apply -f ./deployment.yaml
```
Result:
```
deployment.apps/tomcat-deployment created
```
## the next step is to expose the deployment as a service:
```
kubectl expose deployment tomcat-deployment --type=NodePort

```
## to check the port it was created on :
```
minikube service tomcat-deployment --url
```
Result:
```
http://192.168.99.100:30526
```
## to verify the deployment was success
```
curl http://192.168.99.100:30526
```
Result:
```
 a landing page of Tomcat
```

## to check detail about port:
```
kubectl describe pod <portName>
```
Result:
```
Name:           tomcat-deployment-56ff5c79c5-rb9g2
Namespace:      default
Node:           minikube/10.0.2.15
Start Time:     Mon, 09 Jul 2018 21:01:20 +0900
Labels:         app=tomcat
                pod-template-hash=1299173571
Annotations:    <none>
Status:         Running
IP:             172.17.0.5
Controlled By:  ReplicaSet/tomcat-deployment-56ff5c79c5
Containers:
  tomcat:
    Container ID:   docker://d108f08143ec3f9268e89d982d64a23ed2931c37780be527c573e45cad313b94
    Image:          tomcat:9.0
    Image ID:       docker-pullable://tomcat@sha256:5e5829b0d940cc48cad07dc481077bf892911d5502f89bfe712e7b11c0c75dec
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 09 Jul 2018 21:02:40 +0900
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-f7t6q (ro)
Conditions:
  Type           Status
  Initialized    True 
  Ready          True 
  PodScheduled   True 
Volumes:
  default-token-f7t6q:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-f7t6q
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason                 Age   From               Message
  ----    ------                 ----  ----               -------
  Normal  Scheduled              13m   default-scheduler  Successfully assigned tomcat-deployment-56ff5c79c5-rb9g2 to minikube
  Normal  SuccessfulMountVolume  13m   kubelet, minikube  MountVolume.SetUp succeeded for volume "default-token-f7t6q"
  Normal  Pulling                13m   kubelet, minikube  pulling image "tomcat:9.0"
  Normal  Pulled                 12m   kubelet, minikube  Successfully pulled image "tomcat:9.0"
  Normal  Created                12m   kubelet, minikube  Created container
  Normal  Started                12m   kubelet, minikube  Started container
```
## References:
kubectl reference: 
https://v1-8.docs.kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

kubectl cheatsheet:
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

---
## previusly we defined a "NodePort" service, ow let's define a loadbalancer service
```
kubectl expose deployment tomcat-deployment --type=LoadBalancer --port=8080 --target-port=8080 --name tomcat-load-balancer
```
## finally, let's see what IP address was assigned for the service
```
kubectl describe services tomcat-load-balancer
```
---
# Deployment
* Deployments are a way to declare a desired state for your application.
  * they consist of Pods
  * Optionally , they can also include ReplicaSet(that are automatically managed by the Deployment based on its replica configuration)
* With deployment objects you can:
  * create a new deployment
  * update an existing deployent
  * apply rolling updates to Pods running on your cluster
  * rollback to a previous version
  * pause and resume a deployment
* Kubernetes uses your deployment,usually defining a file to deploy your application. according to how you describe it in your deployment configuration.

## working with deployment
* Kuberctl is your gateway to working with deployments.
* Here’s useful commands and what they do.
  * List deployment:
    * kubectl get deployments
  * view status of deployment roll outs:
    *  kubectl rollout status
  * set the image of a deployment: 
    * kubectl set image
  * view the history of a rollout, including previous revisions: 
    * kubectl rollout history
---
## update an image version
```
kubectl set image deployment/tomcat-deployment tomcat=tomcat:9.0.1
```
## check deployment history
```
kubectl rollout history deployment/tomcat-deployment
```
* then you will see the list of revision
* you can also check the detail about the revision by the following command:
```
kubectl rollout history deployment/tomcat-deployment --revision=2
```

## Label and Selectors
* A method to keep things organized, and to help you (a human) and Kubernetes (a machine)identify resources to act upon
* Labels are key/value pairs that you can attach to objects like pods
* They are for users to help describe meaningful and relevant information about an object
* They do not affect the semantics of the core system
* Selectors are a way of expressing how to select objects based on their labels
* Selectors are a simple language to define what labels match and which ones done
* You can specify if a label equals a given criteria or if it fits inside a set of criteria
  * Equality-based 
  * Set-based

* You can label nearly anything in the Kubernetes world 
  *  Deployments
  *  Services
  *  Nodes
  
* Let’s use labels to label a node that it has SSD storage and then use a selector to tell the deployment that our app should only ever go onto a node with SSD storage
* "nodeSelector" is a property on a deployment that uses labels and selectors to choose which nodes the master decides to run a given pod on



 










































