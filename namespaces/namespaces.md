# Namespaces

- They allow to group resources (for example for Dev, Test, Prod)

- K8s creates a "default" namespace

- Objects in one namespace can access objects in another namespace

- Deleting a namespace will delete all of its resources

## Cheatsheet

```shell
kubectl get namespace
kubectl get ns
```

Set the current context to use a namespace

```shell
kubectl config set-context --current --namespace=[namespace]
```

```shell
kubectl create ns [namespace]
kubectl delete ns [namespace]
```

List all pods in all namespaces

```shell
kubectl get pods --all-namespaces
```

List pods in a specific namespace

```shell
kubectl get pods --namespace=[namespace]
```
