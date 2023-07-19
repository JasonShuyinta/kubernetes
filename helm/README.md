# Helm

Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.

It is a way to package your k8s application, similar to apk or yum in linux.

Helm has a templating engine that helps you in avoiding to rewrite boilerplate code and instead substitute only the values that changes from deployments to deployments, or services to services.

It helps also in versioning each of your "chart", so you can keep track of the changes that happened. It also has a Rollback feature that can come in handy when you roll out a change that breaks something, and with just one command you can restore the previous working version of your application.

This are the main command you are going to use in helm:

```shell
#It creates a default chart file structure for you to work with
helm create chart-name

#It checks that your yaml files are syntacly correct
helm template ./chart-directory

#It is the "kubectl apply" for all of the yaml files in the chart
helm install chart-name ./chart-directory

#Once you make some changes to the chart, it upgrade the version and rolls out the changed yaml and applies them to the cluster
helm upgrade chart-name ./chart-directory

#You can apply a custom value file to the templated files
helm install/upgrade chart-name ./chart-directory -f ./values-file.yaml

# To make sure you are following the best practices when writing Helm Charts
helm lint chart-name

```

Helm is particularly useful when you are working with multiple environments (eg. dev, test, prod) where some of these value changes and you want to avoid to manually search and change on each and every yaml file of your kubernetes cluster.

### Subcharts
Helm is able to create subcharts as well. Under the "chart" folder you can insert other charts and add dependencies to the main chart so that you only need one command to launch all of the charts. 


## Artifact Hub 

Artifact Hub is a web-application where you can publish your Helm charts for collaboration, versioning or pipeline integration purposes.
It is much similar to DockerHub where you can publish either private or publisc docker images that can be used by the community.
