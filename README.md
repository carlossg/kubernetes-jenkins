Running Jenkins master and slaves in a Kubernetes cluster
==================

Kubernetes examples running Jenkins master and slaves

Bring the cluster up and create the services:

    export KUBERNETES_PROVIDER=vagrant
    export KUBERNETES_NUM_MINIONS=2
    vagrant up

    ./cluster/kubecfg.sh list minions
    ./cluster/kubecfg.sh -c pod.json create pods
    ./cluster/kubecfg.sh list pods
    ./cluster/kubecfg.sh -c service-http.json create services
    ./cluster/kubecfg.sh -c service-slave.json create services
    ./cluster/kubecfg.sh list services
    ./cluster/kubecfg.sh -c replication.json create replicationControllers
    ./cluster/kubecfg.sh list pods
    ./cluster/kubecfg.sh resize jenkins-slave 2


## Tearing down

    ./cluster/kubecfg.sh stop jenkins-slave
    ./cluster/kubecfg.sh rm jenkins-slave
    ./cluster/kubecfg.sh delete pods/jenkins
    ./cluster/kubecfg.sh delete services/jenkins
    ./cluster/kubecfg.sh delete services/jenkins-slave
