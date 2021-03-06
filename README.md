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

$ kubectl create -f persistent-volume.yaml
Below is the link to persistenet volume yaml file : https://github.com/sauravdas79/Cassandra-Kubernetes/blob/master/Yaml/persistent-volume.yaml
Yaml/persistent-volume.yaml

# StatefulSet 
StatefulSet Cassandra configuration object is created and through environment variables exposed by the specific Cassandra Docker image here I have used Bitnami/Cassandra Image. One important aspect is the specification of Cassandra seeds. It is recommended that at least one seed node is specified from each data center. So the very first pod in each data center is specified as a seed node. 
Now it is time to create the StatefulSets:
Below is the yaml file which you use and modify accordingly.
https://github.com/sauravdas79/Cassandra-Kubernetes/blob/master/Yaml/cassandra-statefulset.yaml
$ kubectl create -f https://github.com/sauravdas79/Cassandra-Kubernetes/blob/master/Yaml/cassandra-statefulset.yaml

Persistent Volume claim template is used to bind to persistent volume is part of StatefulSet ymal file

# Service
StatefulSet can use a headless service to control the domain of its pods. The domain managed by this service takes the form $(service name).$(namespace).svc.cluster.local, where cluster.local is the cluster domain. As each pod is created, it gets a matching DNS subdomain, taking the form $(podname).$(service name).$(namespace).svc.cluster.local. 
Below is the service file yaml
https://github.com/sauravdas79/Cassandra-Kubernetes/blob/master/Yaml/cassandar-service.yaml

$ kubectl create -f https://github.com/sauravdas79/Cassandra-Kubernetes/blob/master/Yaml/cassandar-service.yaml

So we are done your Cassandra 3 nodes cluster is created 
