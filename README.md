# Cassandra-Kubernetes

Setting up 3 node Cassandra Cluster as StatefulSets on Kubernetes on a local machine or Virtualbox or any managed or unmanaged K8s cluster by any cloud provider.
to complete the steps we will use the Kubernetes concepts of pod, StatefulSet, headless service, and PersistentVolume. Here's what you need:
• A Kubernetes cluster with nodes in a namespace. Make sure Kubernetes is V1.8.x or higher
• At least three nodes in namepace where Kubernetes can deploy pods.
