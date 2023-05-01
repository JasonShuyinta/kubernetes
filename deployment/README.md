# Pods vs Deployment

- Pods don't self-heal, scale, update or rollback.
- Deployments do.

A deployment manages a single pod template, and you can create a deployment for each microservice.

When creating a deployment, a replicaset is also created in the background, and you don't interact directly with the ReplicaSet (you let the Deployment do that).

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
