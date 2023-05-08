# Services

There are 3 type of services:

- ClusterIP (default) -> it is used for internal communcation in the k8s cluster. Outside clients cannot access the pods through this Service. It is used for example for databases, when you want your pods to communicate with your db.
- NodePort
- LoadBalancer