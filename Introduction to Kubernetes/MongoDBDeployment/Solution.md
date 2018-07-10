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
# Deployment solution
## a possible solution
* use kubectl to run default mongo image
```
kubectl run mongo-exercise-1 --image=mongo --port=27017
```
* use kubectl scale to scale the deployment to replica of 4
```
kubectl scale --replicas=4 deployment/mongo-exercise-1
```
## another solution is to write the deployment.yaml file and expose the image