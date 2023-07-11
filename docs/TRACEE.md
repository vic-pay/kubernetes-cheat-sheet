Aqua Security Tracee
====================

Tracee - Runtime Security and Forensics using eBPF

Tracee can be used in K8S and standalone

[Official documentation](https://aquasecurity.github.io/tracee/v0.11/)


Tracee in Kubernetes
--------------------

Install:

    kubectl apply -f https://raw.githubusercontent.com/aquasecurity/tracee/v0.16.0/deploy/kubernetes/tracee/tracee.yaml

    # Or

    kubectl apply -f tracee/manifest-tracee.yml

Get status:

    microk8s kubectl get DaemonSet tracee -n infosec
    microk8s kubectl describe DaemonSet tracee -n infosec

    microk8s kubectl exec tracee-{id} -n infosec -- sh -c "ls"

Get logs:

    microk8s kubectl logs -l app.kubernetes.io/name=tracee --all-containers -f -n infosec

(Optional) Deploy filebeat to gather tracee logs:

    kubectl apply -f  tracee/manifest-filebeat.yml
    microk8s kubectl logs -l app.kubernetes.io/name=filebeat --all-containers -f -n infosec
    kubectl delete -f tracee/manifest-filebeat.yml

Delete assets:

    kubectl delete -f https://raw.githubusercontent.com/aquasecurity/tracee/v0.11.0/deploy/kubernetes/tracee/tracee.yaml

    # Or

    kubectl delete -f tracee/manifest-tracee.yml


Tracee in standalone mode
-------------------------

Tracee can get traces and alerts.

Docker-compose example with alerts in json format:

    # Install
    cd manifests/tracee
    docker-compose -f docker-compose.alert.yml up -d

    # Get alerts
    docker-compose logs -f

    # Delete assets:
    docker-compose -f docker-compose.alert.yml down
    docker image rm aquasec/tracee:0.11.0

Docker-compose example with traces:

    # Install
    cd manifests/tracee
    docker-compose -f docker-compose.trace.yml up -d

    # Gen events
    docker run -it --rm alpine /bin/sh -c 'touch /tmp/1.txt && cat /tmp/1.txt'

    docker run -it --rm alpine /bin/sh -c 'touch /mnt/1.txt && echo 1 >> /tmp/1.txt && rm /tmp/1.txt'

    docker run -it --rm alpine /bin/sh -c 'whoami'

    docker run -it --rm alpine /bin/sh
    whoami

    # Get logs
    docker-compose logs -f

    # Delete assets:
    docker-compose -f docker-compose.trace.yml down
    docker image rm aquasec/tracee:0.11.0

    