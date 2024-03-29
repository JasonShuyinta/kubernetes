# Ingress

An API object that manages external access to the services in a cluster, typically HTTP.

Ingress may provide load balancing, SSL termination and name-based virtual hosting.

## What is it used for?

An ingress exposes HTTP(S) routes from outside the cluster to the services inside of the cluster (this means services can be of type **ClusterIP** which means they are exposed only to the cluster components). Traffic routing is controlled by rules defined in the Ingress resource.

An *Ingress controller* is responsible for fulfilling the Ingress, usually with a load balancer, though it may also configure your edge router or additional frontends to help handle the traffic.

There are some prerequisites:

You must have an Ingress controller to satisfy an Ingress. Only creating an Ingress resource has no effect. You may need to deploy an Ingress controller such as ingress-nginx.

A minimal Ingress resource:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /testpath
        pathType: Prefix
        backend:
          service:
            name: test
            port:
              number: 80
```


The name of an Ingress object must be a valid DNS subdomain name.

If the ingressClassName is omitted, a default Ingress class should be defined.

An Ingress object can also expose multiple backends (or better, services) simply by adding them as a list like in the following example:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-name
  annotations:
    nginx.org/proxy-connect-timeout: "65s"
    nginx.org/proxy-read-timeout: "65s"
    nginx.org/proxy-send-timeout: "65s"
spec:
  ingressClassName: nginx
  rules:
  - host: my-host.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-gateway
            port:
              number: 8000
      - path: /admin-console
        pathType: Prefix
        backend:
          service:
            name: admin-console-service
            port:
              number: 8001
```

As you can see from above, when a request is trying to reach **my-host.com/admin-console** instead of going to the *my-gateway* service, it will be redirected to the *admin-console-service* instead.

**NOTE**: on GKE, the exposed service, in this case *my-gateway* and *admin-console-service* are required to be of type NodePort or LoadBalancer. The type ClusterIP is not allowed for Ingress-exposed services.

### Ingress rules

Each HTTP rule contains the following information:
-   An optional host. In this example, no host is specified, so the rule applies to all inbound HTTP traffic through the IP address specified. If a host is provided (for example, foo.bar.com), the rules apply to that host.
-   A list of paths (for example, /testpath), each of which has an associated backend defined with a service.name and a service.port.name or service.port.number. Both the host and path must match the content of an incoming request before the load balancer directs traffic to the referenced Service.
-   A backend is a combination of Service and port names. HTTP (and HTTPS) requests to the Ingress that match the host and path of the rule are sent to the listed backend.

### Path types

Each path in an Ingress is required to have a corresponding path type. Paths that do not include an explicit pathType will fail validation. There are three supported path types:

-   ImplementationSpecific: With this path type, matching is up to the IngressClass. Implementations can treat this as a separate pathType or treat it identically to Prefix or Exact path types.
-   Exact: Matches the URL path exactly and with case sensitivity.
-   Prefix: Matches based on a URL path prefix split by /. Matching is case sensitive and done on a path element by element basis. A path element refers to the list of labels in the path split by the / separator. A request is a match for path p if every p is an element-wise prefix of p of the request path.

|Kind   | Path(s)   | Request path(s)   | Mathches? |
|-------|-----------|-------------------|-----------|
|Prefix | /         |(all paths)        |Yes        |
|Exact	|/foo	    |/foo	            |Yes
|Exact	|/foo	    |/bar	            |No
|Exact	|/foo	    |/foo/	            |No
|Exact	|/foo/	    |/foo	            |No
|Prefix	|/foo	    |/foo, /foo/	    |Yes
|Prefix	|/foo/	    |/foo, /foo/	    |Yes
|Prefix	|/aaa/bb	|/aaa/bbb	        |No
|Prefix	|/aaa/bbb	|/aaa/bbb	        |Yes
|Prefix	|/aaa/bbb/	|/aaa/bbb	        |Yes, ignores trailing slash
|Prefix	|/aaa/bbb	|/aaa/bbb/	        |Yes, matches trailing slash
|Prefix	|/aaa/bbb	|/aaa/bbb/ccc	    |Yes, matches subpath
|Prefix	|/aaa/bbb	|/aaa/bbbxyz	    |No, does not match string prefix
|Prefix	|/, /aaa	|/aaa/ccc	        |Yes, matches /aaa prefix
|Prefix	|/, /aaa, /aaa/bbb  |	/aaa/bbb    |Yes, matches /aaa/bbb prefix
|Prefix	|/, /aaa, /aaa/bbb  |	/ccc    |Yes, matches / prefix
|Prefix	|/aaa	    |/ccc   |No, uses default backend
|Mixed	|/foo (Prefix), /foo (Exact)    |/foo   |Yes, prefers Exact

To add a TLS certificate to allow https connections to your Ingress, simply create a secret with your certificate adding as key: *tls.key* and *tls.crt* , obviously encoded as a Base64 string. 

You can do that on linux by issuing the command: 
```shell
cat my-key-file.key | base64 -w 0

cat my-cert-file.pem | base64 -w 0
```

After you have configured your secret in the same namespace as your Ingress resource, you can add the configuration to the yaml file of the Ingress as follows:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
spec:
   tls:
    - secretName: tls-secret
      hosts:
        - your-host.com
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /testpath
        pathType: Prefix
        backend:
          service:
            name: test
            port:
              number: 80
```

## GKE

When deploying an Ingress in GKE remember that it is recommended to use GCE as your Ingress Controller.

There are some differences that apply when using GCE instead of  Nginx. For example it is recommended to use ImplementationSpecific instead of Prefix. In addition to that you might have to apply a BackendConfig to achieve the same result you achieve with annotations in Ingress of type Nginx. The BackendConfig will then be attached to a specific service to add the configuration desired.
