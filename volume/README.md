# Volume

There are 3 types of Volumes: 

- PersistentVolume
- PersistentVolumeClaim
- StorageClass

## PersistentVolume
It is used to store data, and it could be stored on the Cloud, on NFS or in local (refer to docs for the complete list [Kubernetes/PersistentVolumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) ).

## PersistentVolumeClaim
It is used when you need a pod to access a PersistentVolume. A Pod cannot access directly a PersistentVolume, it has to pass through the PVC. When creating a PVC you specify the storage requirments your pod needs, and that way if there is a PV that satisfies these requirements it will then create a connection between the pod and the PV.

When creating and applying a **PersistentVolume* under the "state" columns you will see available. Once you create a **PersistentVolumeClaim** which is attached to that PersistentVolume you will see that the state is changed to "bound".

## StorageClass
When you need a type of storage you can define it in this yaml file and it will create the PV automatically if it does not already exist.
