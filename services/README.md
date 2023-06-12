# Services

For some parts of your application (for example, frontends) you may want to expose a Service onto an external IP address, that's outside of your cluster.

Kubernetes ServiceTypes allow you to specify what kind of Service you want.

Type values and their behaviors are:

-   ClusterIP: Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default that is used if you don't explicitly specify a type for a Service. You can expose the service to the public with an Ingress or the Gateway API.
-   NodePort: Exposes the Service on each Node's IP at a static port (the NodePort). To make the node port available, Kubernetes sets up a cluster IP address, the same as if you had requested a Service of type: ClusterIP.
-   LoadBalancer: Exposes the Service externally using a cloud provider's load balancer.
-   ExternalName: Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up.

If you want to access your pods through the services, you can use a service of type NodePort.
So let's say the gateway pod is running on port 30001 (on a Service of type NodePort). The minikube ip is: 192.169.59.2. Then you can access the API using http://192.169.59.2:30001/api.

In case you want to access some other Service you can use the port-forward command:

```shell
kubectl port-forward my-service 8080:30001
```
In this way you can access the gateway at: http://localhost:8080/api.

Now let's say you are running k8s on a Linux VM in WSL.

How can you access the services from outside the WSL? It's easy all you have to do is run this command:

```shell
minikube service my-service -n namespace --url
```

Keeping the terminal open you can now access the API at the url that is provided from the command.
