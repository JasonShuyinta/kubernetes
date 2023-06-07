# Kubernetes

I'm creating this Kubernetes cheatsheet in order to identify and debug common problems and always have useful commands at hand at any time.
The project is still in the making so please be sure to double-check before using any of the commands showed in this repository.

Feel free to make requests for updates or correct mistakes.

Useful image for Troubleshooting a Kubernetes infrastructure

(image courtesy of [ByteByteGo](https://www.linkedin.com/posts/bytebytego_systemdesign-coding-interviewtips-activity-7059045288400805888--Esb?utm_source=share&utm_medium=member_desktop))
![1683007546827](https://user-images.githubusercontent.com/50492920/235598157-d14fc5c6-de4c-477e-9fe6-b1d666697aab.jpg)

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