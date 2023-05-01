# ReplicatSet

Its a primary method of managing pod replicas and their lifecycle to
provide self-healing capabilities.

Their job is to ensure the desired number of pods are running.

The recommended way to create ReplicaSets is through Deployments.

## Cheatsheet

```shell
kubectl apply -f [replicaset.yml]
kubectl get rs
kubectl describe rs [rsName]
kubectl delete -f [replicaset.yml]
kubectl delete rs [rsName]
```
