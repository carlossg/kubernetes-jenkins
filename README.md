Running Jenkins master and slaves in a Kubernetes cluster
=========================================================

Kubernetes examples running Jenkins master and slaves

Creating a cluster
==================

Local with Docker Compose
-------------------------

A local testing cluster with one node can be created with Docker Compose

```
docker-compose up
```

When using boot2docker or Docker Engine with a remote host, the remote Kubernetes API can be exposed
with `docker-machine ssh MACHINE_NAME -- -L 0.0.0.0:8080:localhost:8080` or `boot2docker ssh -L 0.0.0.0:8080:localhost:8080`

More info

* [Docker CookBook examples](https://github.com/how2dock/docbook/tree/master/ch05/docker)
* [Kubernetes Getting started with Docker](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/docker.md)

Google Compute Engine
---------------------

```
export KUBERNETES_HOME=~/kubernetes
export KUBERNETES_PROVIDER=gce
export KUBERNETES_NUM_MINIONS=2
export KUBE_GCE_ZONE=us-central1-a
$KUBERNETES_HOME/cluster/kube-up.sh
```

Creating the pods and services
==============================

```
kubectl get nodes

# Vagrant
kubectl create --validate -f pod-vagrant.yml
# or GKE
kubectl create --validate -f pod-gke.yml

kubectl get pods
kubectl create --validate -f service.yml
kubectl get services
kubectl create --validate -f replication.yml
kubectl get rc
kubectl get pods
kubectl scale replicationcontrollers --replicas=2 jenkins-slave

# if running in GCE, create the forwarding rule
gcloud compute forwarding-rules list
gcloud compute forwarding-rules describe --region us-central1 kubernetes-default-jenkins
gcloud compute firewall-rules create --validate jenkins-node-master --allow=tcp:8888 --target-tags kubernetes-minion

```

Rolling update
==============

```
kubectl rolling-update jenkins-slave --update-period=10s -f replication-v2.yml
```

Tearing down
============

```
kubectl stop jenkins-slave
kubectl delete replicationcontrollers jenkins-slave
kubectl delete pods jenkins
kubectl delete services jenkins
$KUBERNETES_HOME/cluster/kube-down.sh
gcloud compute firewall-rules delete jenkins-node-master
```

Demo
====

Kubernetes cluster up
[![asciicast](https://asciinema.org/a/18161.png)](https://asciinema.org/a/18161)

Jenkins master and slaves provisioning
[![asciicast](https://asciinema.org/a/18162.png)](https://asciinema.org/a/18162)

Kubernetes cluster teardown
[![asciicast](https://asciinema.org/a/18163.png)](https://asciinema.org/a/18163)
