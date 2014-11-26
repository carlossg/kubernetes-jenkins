Running Jenkins master and slaves in a Kubernetes cluster
==================

Kubernetes examples running Jenkins master and slaves

Bring the cluster up and create the services, from a Kubernetes distribution:

    export KUBERNETES_PROVIDER=vagrant
    export KUBERNETES_NUM_MINIONS=2
    ./cluster/kube-up.sh
    git clone https://github.com/carlossg/kubernetes-jenkins.git

    ./cluster/kubecfg.sh list minions
    ./cluster/kubecfg.sh -c kubernetes-jenkins/pod.json create pods
    ./cluster/kubecfg.sh list pods
    ./cluster/kubecfg.sh -c kubernetes-jenkins/service-http.json create services
    ./cluster/kubecfg.sh -c kubernetes-jenkins/service-slave.json create services
    ./cluster/kubecfg.sh list services
    ./cluster/kubecfg.sh -c kubernetes-jenkins/replication.json create replicationControllers
    ./cluster/kubecfg.sh list pods
    ./cluster/kubecfg.sh resize jenkins-slave 2


## Tearing down

    ./cluster/kubecfg.sh stop jenkins-slave
    ./cluster/kubecfg.sh rm jenkins-slave
    ./cluster/kubecfg.sh delete pods/jenkins
    ./cluster/kubecfg.sh delete services/jenkins
    ./cluster/kubecfg.sh delete services/jenkins-slave
