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


## Provisioning
PVs may be provisioned statically or dynamically.
- Static: a cluster admin creates a number of PVs. Ther carry the details of real storage (cloud, nfs, local-storage). They exist in K8s API and are available for consumption.
- Dynamic: when none of the static PVs match the PVCs, the cluster may try to dynamically provision a volume for that PVC. This provisioning is based on StorageClasses -> the PVC must request a storage class and the admin must have created and configured that class for the dynamic provisioning to occur.

Some types of Persistent Volumes:

- cephfs - CephFS volume
- csi - Container Storage Interface (CSI)
- fc - Fibre Channel (FC) storage
- hostPath - HostPath volume (for single node testing only; WILL NOT WORK in a multi-node cluster; consider using local volume instead)
- iscsi - iSCSI (SCSI over IP) storage
- **local** - local storage devices mounted on nodes.
- nfs - Network File System (NFS) storage
- rbd - Rados Block Device (RBD) volume

### Local

A local volume represents a mounted local storage device such as a disk, partition or directory.

Local volumes can only be used as a statically created PersistentVolume. **Dynamic provisioning is not supported**.

Compared to hostPath volumes, local volumes are used in a durable and portable manner without manually scheduling pods to nodes. The system is aware of the volume's node constraints by looking at the node affinity on the PersistentVolume.

However, local volumes are subject to the availability of the underlying node and are not suitable for all applications. If a node becomes unhealthy, then the local volume becomes inaccessible by the pod. The pod using this volume is unable to run. Applications using local volumes must be able to tolerate this reduced availability, as well as potential data loss, depending on the durability characteristics of the underlying disk.

The following example shows a PersistentVolume using a local volume and nodeAffinity:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 100Gi
  volumeMode: Filesystem
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  storageClassName: local-storage
  local:
    path: /mnt/disks/ssd1
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - example-node
```

You must set a PersistentVolume *nodeAffinity* when using local volumes. The Kubernetes scheduler uses the PersistentVolume nodeAffinity to schedule these Pods to the correct node.

PersistentVolume *volumeMode* can be set to "Block" (instead of the default value "Filesystem") to expose the local volume as a raw block device.

When using local volumes, it is recommended to create a StorageClass with *volumeBindingMode* set to **WaitForFirstConsumer**. For more details, see the local StorageClass example (below). Delaying volume binding ensures that the PersistentVolumeClaim binding decision will also be evaluated with any other node constraints the Pod may have, such as node resource requirements, node selectors, Pod affinity, and Pod anti-affinity.

An external static provisioner can be run separately for improved management of the local volume lifecycle. Note that this provisioner does not support dynamic provisioning yet. For an example on how to run an external local provisioner, see the local volume provisioner user guide.

#### Local StorageClass
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```

Local volumes do not currently support dynamic provisioning, however a StorageClass should still be created to delay volume binding until Pod scheduling. This is specified by the WaitForFirstConsumer volume binding mode.

Delaying volume binding allows the scheduler to consider all of a Pod's scheduling constraints when choosing an appropriate PersistentVolume for a PersistentVolumeClaim.
