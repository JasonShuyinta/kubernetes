# Pods vs Deployment

- Pods don't self-heal, scale, update or rollback.
- Deployments do.

A deployment manages a single pod template, and you can create a deployment for each microservice.

When creating a deployment, a replicaset is also created in the background, and you don't interact directly with the ReplicaSet (you let the Deployment do that).

### Resources and limits
When creating a deployment you can define the resources the container inside of the pod can take. You can also add limits to the quantity of memory the container is allowed to take. When a process in the container tries to consume more than the allowed amount of memory, the system kernel terminates the process with an Out Of Memory (OOM) error.

**CPU** and **memory** are *resource types*. 

CPU represents compute processing and is specified in units of Kubernetes CPUs. Memory is specified in units of bytes.

Limits and requests for CPU resources are measured in cpu units. In Kubernetes, 1 CPU unit is equivalent to 1 physical CPU core, or 1 virtual core, depending on whether the node is a physical host or a virtual machine running inside a physical machine.

It is also possible to tell K8S on which node you want to deploy your pod. You can do this by adding the **nodeSelector** field. All you have to do is gather the label of the k8s node and add it to the field. Remember, it needs to be unique as k8s needs to be sure on which node to deploy your pod.


```yaml
# e.g. node label is : kubernetes.io/hostname: devkube01
nodeSelector:
  kubernetes.io/hostname: devkube01
```

Example:

The following Pod has two containers. Both containers are defined with a request for 0.25 CPU and 64MiB (226 bytes) of memory. Each container has a limit of 0.5 CPU and 128MiB of memory. You can say the Pod has a request of 0.5 CPU and 128 MiB of memory, and a limit of 1 CPU and 256MiB of memory.

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
  - name: app
    image: images.my-company.example/app:v4
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
  - name: log-aggregator
    image: images.my-company.example/log-aggregator:v6
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

You can measure the usage of resources using the Metrics API.
This API allows you to access CPU and memory usage for the nodes and pods in your cluster. Its primary role is to feed resource usage metrics to K8s autoscaler components.
Here is an example of the Metrics API request for a minikube node piped through jq for easier reading:

```shell
kubectl get --raw "/apis/metrics.k8s.io/v1beta1/nodes/minikube" | jq '.'
```

Here is the same API call using curl:
```shell
curl http://localhost:8080/apis/metrics.k8s.io/v1beta1/nodes/minikube
```


## Cheatsheet

```shell
#Imperative way
kubectl create deploy [deploymentName] --image=nginx --replicas=3 --port=80
#Declarative
kubectl apply -f [deployment.yaml]
kubectl get deploy
kubectl describe deploy [deploymentName]

#Get ReplicaSets
kubectl get rs

#Deletion
kubectl delete -f [deployment.yaml]
kubectl delete deploy [deploymentName]
```
