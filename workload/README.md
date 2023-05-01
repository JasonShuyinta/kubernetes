# Workloads

A workload is an application running on k8s. It has:

- Pod (set of running containers)
- ReplicaSet
- Deployment
- StatefulSet (special workload)
- DaemonSet (special workload)

## DaemonSet

Ensures that all nodes run an instance of a Pod.
Scheduled by the scheduler controller and run by the daemon controller.

As nodes are added to the cluster, Pods are added to them.

Typical uses:

- Ruuning a cluster storage daemon
- Running a logs collection daemon on every node
- Running a node monitoring daemon on every node

### CheatSheet DaemonSet

```shell
kubectl apply -f [daemonset.yaml]
kubectl get ds
kubectl describe ds [rsName]
kubectl delete -f [daemonset.yaml]
kubectl delete ds [rsName]
```

## StatefulSet

Scaling a Database is complex as an instance can be used to write to DB while the other ones are read-only.

A StatefulSet is for Pods that must persist or maintain state.
Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods.

Each has a presistent identifier, and if a pod dies it is replaced with another one using the identifier.

When creating a StatefulSet, they are created in sequence from 0 to n and deleted from n to 0.

Use cases are:

- Stable, unique network identifiers
- Stable, databases using persistent storage.

How to make a request to these instances?

To Write use **hostname:mysql-0.mysql**

To Read use **hostname:mysql** (k8s will load balance the call through all of the instances)

N.B.
**Containers are stateless by design**

And StatefulSets offers a solution for stateful scenarios.
Would be better to use a Cloud provider DB.
Deleting a StatefulSet will not delete the PVC (needs to be done manually).

### CheatSheet StatefulSet

```shell
kubectl apply -f [statefulset.yaml]
kubectl get sts
kubectl describe sts [rsName]
kubectl delete -f [statefulset.yaml]
kubectl delete sts [rsName]
```
