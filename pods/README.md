# Pods

Is the smallest unit of work on K8s and it contains the application's containers.

It represents a unit of deployment, and a pods can run multiple containers (usually though a single container runs on a single pod).

In a multi-container scenario, the containers share the IP address and mounted volumes of the Pod and communicate using Localhost or IPC.

Pods are ephemeral and you don't update a pod but you replace it with an updated version.

Scaling is done by adding more pods and not more containers in a pod.

A node can run 1 or more Pods.

## Cheatsheet

```shell
#Create a pod
kubectl create -f [pod.yaml]

#Run a pod
kubectl run [podname] --image=nginx -- /bin/sh

#Get pods
kubectl get pods
kubectl get pods -o wide
kubectl describe pod [podname]

#Extract pod definition to a file
kubectl get pod [podname] -o yaml > file.yaml

#Interactive mode
kubectl exec -it [podname] -- sh

#Delete
kubectl delete -f [pod.yaml]
kubectl delete pod [podname]
```

In order to get logs of a specific container you can run the following command:

```shell
# Get logs of pod with 1 container
kubectl logs [podname]

# Get logs of pod with multiple containers
kubectl logs [podname] -c [containername]

# Get logs of killed pod
kubectl logs [podname] --previous
```
