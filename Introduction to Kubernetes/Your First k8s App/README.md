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
  replica: 1
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

## we will use the apply command to take the directives from this file:
```
    
```