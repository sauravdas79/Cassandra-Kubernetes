# Cassandra-Kubernetes

Setting up 3 node Cassandra Cluster as StatefulSets on Kubernetes on a local machine or Virtualbox or any managed or unmanaged K8s cluster by any cloud provider.
to complete the steps we will use the Kubernetes concepts of pod, StatefulSet, headless service, and PersistentVolume. Here's what you need:
• A Kubernetes cluster with nodes in a namespace. Make sure Kubernetes is V1.8.x or higher
• At least three nodes in namepace where Kubernetes can deploy pods.

FOR Cassandra container Image: I have used bitnami's Cassandar image. https://hub.docker.com/r/bitnami/cassandra/dockerfile.

# Creating the namespace
 $ kubectl create namespace database-ns
 
# Create persistance host volume if your cluster is on VirtualBox or cloud provided Storage Volume.
 Persistent volumes is needed for databases to keep the state of the Database. Cassandra local storage is needed for each of its nodes, So we need to provision
 the storage in advance before we create the StatefulSets.
 
 3 nodes. First, create a directory on each of the nodes to store Cassandra data, with hostpath and path: /data/cassandra/cassandra-data-a
 capacity 1GB of storage is available in this directory you have to create the director before. Create this directory on each of the three nodes, on each site.
 Then create all three PersistentVolumes using the YAML file provided in GitHub. You can adjust the capacity and location of the directory in this file as needed.
$ mkdir -p /data/cassandra/cassandra-data-a
$ mkdir -p /data/cassandra/cassandra-data-b
$ mkdir -p /data/cassandra/cassandra-data-c

You can create a storage class to do auto provision the pv for cloud base pv.below is the link
https://kubernetes.io/docs/concepts/storage/storage-classes/

 
