# StatefulSet

It is used to manage stateful applications (Databases) as it provides guarantees about the ordering and uniqueness of the Pods.

Statefulsets maintain a sticky identity to each of its Pods (these Pods are
not interchangeable).

- Deleting or scaling a Statefulset will not delete the volumes associated
with the StatefulSet.
- StatefulSets required a Headless Service. 
- It is advisable to create a PersistentVolume or PersistentVolumeClaim to store data

To create a Volume, refer to the volume section in this repository

### Limitations of the StatefulSet

- The storage for a given Pod must either be provisioned by a PersistentVolume Provisioner based on the requested storage class, or pre-provisioned by an admin.
- Deleting and/or scaling a StatefulSet down will not delete the volumes associated with the StatefulSet. This is done to ensure data safety, which is generally more valuable than an automatic purge of all related StatefulSet resources.
- StatefulSets currently require a Headless Service to be responsible for the network identity of the Pods. You are responsible for creating this Service.
- StatefulSets do not provide any guarantees on the termination of pods when a StatefulSet is deleted. To achieve ordered and graceful termination of the pods in the StatefulSet, it is possible to scale the StatefulSet down to 0 prior to deletion.


```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx # has to match .spec.template.metadata.labels
  serviceName: "nginx"
  replicas: 3 # by default is 1
  minReadySeconds: 10 # by default is 0
  template:
    metadata:
      labels:
        app: nginx # has to match .spec.selector.matchLabels
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: registry.k8s.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "my-storage-class"
      resources:
        requests:
          storage: 1Gi
```

In the above example:

A Headless Service, named nginx, is used to control the network domain.
The StatefulSet, named web, has a Spec that indicates that 3 replicas of the nginx container will be launched in unique Pods.
The volumeClaimTemplates will provide stable storage using PersistentVolumes provisioned by a PersistentVolume Provisioner.

You must set the .spec.selector field of a StatefulSet to match the labels of its .spec.template.metadata.labels. Failing to specify a matching Pod Selector will result in a validation error during StatefulSet creation.

You can set the .spec.volumeClaimTemplates which can provide stable storage using PersistentVolumes provisioned by a PersistentVolume Provisioner.

### Headless Service

Is a service with a service IP but instead of load-balancing it will return the IPs of our associated pods.

This allows to interact directly with the pods instead of a proxy. All you have to do is define
the clusterIP as None (use headlessService.yaml as reference).

When creating a Headless service you can access the pod using their DNS entry: podname.servicename.namespace.svc.cluster.local.27017
For example let us have this HeadlessService

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongo
spec:
  ports:
    - name: mongo
      port: 27017
      targetPort: 27017
  clusterIP: None
  selector:
    app: mongo
```
In this way we can select the "Master" node to do the Writes on the DB and the "Slave" nodes to do the Reads from the DB.

The Master node would be contacted using its DNS which would be:

**mongo-0.mongo.default.svc.cluster.local:27017**

## Cheatsheet
```shell
kubectl apply -f [statefulset.yml]
kubectl get statefulsets
kubectl describe statefulset [statefulsetName]
kubectl delete -f [statefulset.yml]
kubectl delete statefulset [statefulsetName]
```
