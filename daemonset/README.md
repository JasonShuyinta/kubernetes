# Daemonset

A Daemonset is a K8S object that ensures that a particular pod runs on all or some nodes inside of the cluster. 

Some typical uses of a DaemonSet are:
-   running a cluster storage daemon on every node
-   running a logs collection daemon on every node
-   running a node monitoring daemon on every node

You can run a specific pod on some nodes by following these rules:

If you specify a **.spec.template.spec.nodeSelector**, then the DaemonSet controller will create Pods on nodes which match that node selector. Likewise if you specify a .**spec.template.spec.affinity**, then DaemonSet controller will create Pods on nodes which match that node affinity. If you do not specify either, then the DaemonSet controller will create Pods on all nodes.