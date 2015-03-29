Running Jenkins master and slaves in a Kubernetes cluster
==================

Kubernetes examples running Jenkins master and slaves

Bring the cluster up and create the services, from a Kubernetes distribution:

```
export KUBERNETES_PROVIDER=gce
export KUBERNETES_NUM_MINIONS=2
export KUBE_GCE_ZONE=us-central1-a
cluster/kube-up.sh
git clone https://github.com/carlossg/kubernetes-jenkins.git

cluster/kubectl.sh get minions
cluster/kubectl.sh create -f kubernetes-jenkins/pod.json
cluster/kubectl.sh get pods
cluster/kubectl.sh create -f kubernetes-jenkins/service-http.json
cluster/kubectl.sh create -f kubernetes-jenkins/service-slave.json
cluster/kubectl.sh get services
gcloud compute forwarding-rules list
gcloud compute forwarding-rules describe k8s-jenkins-default-jenkins
gcloud compute firewall-rules create jenkins-node-master --allow=tcp:8888 --target-tags k8s-jenkins-minion
cluster/kubectl.sh create -f kubernetes-jenkins/replication.json
cluster/kubectl.sh get pods
cluster/kubectl.sh resize replicationcontrollers --replicas=2 jenkins-slave
```


## Tearing down

```
cluster/kubectl.sh stop jenkins-slave
cluster/kubectl.sh delete replicationcontrollers jenkins-slave
cluster/kubectl.sh delete pods jenkins
cluster/kubectl.sh delete services jenkins
cluster/kubectl.sh delete services jenkins-slave
cluster/kube-down.sh
gcloud compute firewall-rules delete jenkins-node-master
```

# Demo

Kubernetes cluster up
[![asciicast](https://asciinema.org/a/18161.png)](https://asciinema.org/a/18161)

Jenkins master and slaves provisioning
[![asciicast](https://asciinema.org/a/18162.png)](https://asciinema.org/a/18162)

Kubernetes cluster teardown
[![asciicast](https://asciinema.org/a/18163.png)](https://asciinema.org/a/18163)
