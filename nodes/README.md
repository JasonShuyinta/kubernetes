# Nodes

## Master node

The master node is the one in charge of controlling the K8s cluster. All other nodes are called worker nodes.

A master node has different components inside of it:

- kube-controller-manager
- cloud-controller-manager
- kube-sheduler
- kube-apiserver
- etcd

## kube-apiserver

1. REST interface
2. It saves the state to the datastore (etcd)
3. All clients interact with it, and not directly with the datastore.

## etcd

1. Is the clusters datastore
2. Key-value store
3. It is not for application use
4. Single source of truth

## kube-control-manager

It runs the controllers such as:

- node controller
- replication controller
- endpoints controller
- service account & token controllers

## cloud-control-manager

It iteracts with the cloud providers controllers:

- node
- route
- service
- volume

## kube-scheduler

Watcher newly created pods that have no node assigned and selects a node for them to run on.

To choose on which node to run the pod in takes into account:

- individual and collective resource requirments
- Hardware/software/policy constraints
- Affinity and anti-affinity specifications
- Data locality

## Worker Node

It is a physical or virtual machine. Together many worker nodes form a cluster. Pods run inside a node.

When a worker node is added to a cluster, some k8s services are added to the node:

- container run time
- kubelet
- kube-proxy

## kubelet

It manager the pods lifecycle and ensures that the containers described in the Pod specs are running and healthy.

## kube-proxy

Its a network proxy that manages the network rules on nodes.

## container runtime

k8s supports different container runtime for example Moby, Containerd, Cri-0, Rkt, Kata, Virtlet.

## Cheatsheet

```shell
kubectl get nodes
kubectl describe node
```
