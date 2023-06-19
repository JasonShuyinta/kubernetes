# Kubernetes

I'm creating this Kubernetes cheatsheet in order to identify and debug common problems and always have useful commands at hand at any time.
The project is still in the making so please be sure to double-check before using any of the commands showed in this repository.

Feel free to make requests for updates or correct mistakes.

Useful image for Troubleshooting a Kubernetes infrastructure

(image courtesy of [ByteByteGo](https://www.linkedin.com/posts/bytebytego_systemdesign-coding-interviewtips-activity-7059045288400805888--Esb?utm_source=share&utm_medium=member_desktop))
![1683007546827](https://user-images.githubusercontent.com/50492920/235598157-d14fc5c6-de4c-477e-9fe6-b1d666697aab.jpg)


## Minikube vs Kubernetes Cluster

The main difference is that minikube has a single node that acts both as a master and as a worker node, thus removing some of the network complexity.

That's why when using Kubernetes Cluster, you cannot just build your docker images locally, but you need them to build on the node from where you will deploy your pods.

That's why it is better to use a customer container registry from where to retrieve the images each time you want to deploy something.

## Useful links

- [Sealed Secrets for K8s with GitOps](https://piotrminkowski.com/2022/12/14/sealed-secrets-on-kubernetes-with-argocd-and-terraform/)

## Skaffold

Another useful tool for K8S deployment is Skaffold

- [Skaffold](https://skaffold.dev/)

### Notes

#### Jib

Jib is a tool that lets you create Docker images without using Docker.
It's a maven/gradle plugin that you can integrate into your Java project.

One of the best practices that Jib uses is that it does not run your container as root.

#### Kompose

As per the official documentation, kompose is a tool that let's you transform your docker compose file into different kubernetes files (deployment and services).

#### ClusterWatch

Another very cool project is [ClusterWatch](https://github.com/oslabs-beta/ClusterWatch) :

ClusterWatch is an open-source tool which simplifies and provides an all-in-one hub for Kubernetes cluster monitoring. It reduces the need for DevOps engineers to configure their own Kubernetes monitoring stacks, and automates the process so you can get vital cluster information from various different tools, all in one place, in just a few seconds.
