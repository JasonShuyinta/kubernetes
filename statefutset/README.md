# StatefulSet

It is used to manage stateful applications (Databases) as it provides guarantees about the ordering and uniqueness of the Pods.

Statefulsets maintain a sticky identity to each of its Pods (these Pods are
not interchangeable).

- Deleting or scaling a Statefulset will not delete the volumes associated
with the StatefulSet.
- StatefulSets required a Headless Service. 
- It is advisable to create a PersistentVolume or PersistentVolumeClaim to store data

To create a Volume, refer to the volume section in this repository

### Headless Service

Is a service with a service IP but instead of load-balancing it will return the IPs of our associated pods.

This allows to interact directly with the pods instead of a proxy. All you have to do is define
the clusterIP as None (use headlessService.yaml as reference).

## Cheatsheet
```shell
kubectl apply -f [statefulset.yml]
kubectl get statefulsets
kubectl describe statefulset [statefulsetName]
kubectl delete -f [statefulset.yml]
kubectl delete statefulset [statefulsetName]
```
