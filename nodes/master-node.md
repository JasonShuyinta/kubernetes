# Master Node

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
