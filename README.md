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
# Kubenetes Installation:

## Install kubectl

1. Install kubectl binary via curl

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
```

2. Make it executable
```
chmod +x ./kubectl

```
3. Move the library in to your PATH
```
 sudo mv ./kubectl /usr/local/bin/kubectl
```
## Install minikube

1. Install 
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```
### Basic minikube commands
* Start minikube
```
$minikube start
```

the result wil be as following:
<img src="https://github.com/Teckchun/-Kubernetes-Demo/blob/master/Assets/Images/kube-ctl-start-result.png?raw=true">

### Let's use kubectl to actually deploy the hello-minikube sample application

* we use the kubectl run command to deploy a sample image to our Kubernetes cluster. this command is wordth noting.
```
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080
```
* this command provide a simple and easy way to run a given docker image on your Kubernetes cluster

### Now that's it's deployed. let's expose minikube sample application using the service command. this command will take the application which is running locally in our Kubernetes cluster and allow external IP address to access it

```
kubectl expose deployment hello-minikube --type=NodePort

```

### to see the status use 
```
kubectl get pod
a
```
<img src="https://github.com/Teckchun/-Kubernetes-Demo/blob/master/Assets/Images/kube-ctl-get-pod.png?raw=true">

### to delete deployment use:
```
kubectl delete deployment hello-minikube 
```

### to stop local Minikube cluster and free up any resources when you're completed

```
minikube stop
```



