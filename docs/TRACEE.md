Aqua Security Tracee
====================

Tracee - Runtime Security and Forensics using eBPF

Tracee can be used in K8S and standalone

[Official documentation](https://aquasecurity.github.io/tracee/v0.11/)


Tracee in Kubernetes
--------------------

Install:

    kubectl apply -f https://raw.githubusercontent.com/aquasecurity/tracee/v0.11.0/deploy/kubernetes/tracee/tracee.yaml

Get status:

    microk8s kubectl get DaemonSet tracee
    microk8s kubectl describe DaemonSet tracee

Get logs:

    microk8s kubectl logs -l app.kubernetes.io/name=tracee --all-containers -f

Delete assets:

    kubectl delete -f https://raw.githubusercontent.com/aquasecurity/tracee/v0.11.0/deploy/kubernetes/tracee/tracee.yaml


Tracee in standalone mode
-------------------------

Docker-compose example with json logs.

Install:

    cd manifests/tracee
    docker-compose up -d

Get logs:

    docker-compose logs -f

Delete assets:

    docker-compose down
    docker image rm aquasec/tracee:0.11.0