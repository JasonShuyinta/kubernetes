# Job

A job is a workload for short lived tasks. Creates one
or more Pods and ensures that a specified number of them
successfully terminate.

As pods successfully complete, the Job tracks the successful
completions

When a specified number of successful completions is reached,
the Job completes.

By default, jobs with more then 1 pod will create them
one after the other. To create them at the same time you need to add parellelism.

## CheatSheet

```shell
# Imperative way
kubectl create job [jobName] --image=nginx

# Declarative way
kubectl apply -f [job.yaml]

kubectl get job
kubectl describe job
kubectl delete -f [job.yaml]
kubectl delete job [jobName]
```
