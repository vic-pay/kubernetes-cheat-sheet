Cillum Tetragon
===============

Tetragon - eBPF-based Security Observability and Runtime Enforcement

Tetragon can be used in K8S and standalone

[Github](https://github.com/cilium/tetragon)


Tetragon in Kubernetes
----------------------

Install:

    microk8s helm repo add cilium https://helm.cilium.io
    microk8s helm repo update
    microk8s helm install tetragon cilium/tetragon -n kube-system
    microk8s kubectl rollout status -n kube-system ds/tetragon -w

Apply policy:

    microk8s kubectl create -f manifests/tetragon/policy-write.yaml

Start getting logs:

    kubectl exec -it -n kube-system ds/tetragon -c tetragon -- tetra getevents -o compact-stdout -f

Gen events (in another console):

    kubectl exec -it -n kube-system ds/tetragon -c tetragon -- sh
    touch /tmp/1.txt

Delete resources:

    microk8s helm uninstall tetragon -n kube-system
    microk8s kubectl create -f manifests/tetragon/policy-write.yaml


Tetragon in standalone mode
---------------------------

`Docker-compose` example

Install: 

    cd manifests/tetragon
    docker-compose up -d

Apply policy:

    docker cp policy-exec.yaml tetragon-agent:/tmp/policy-exec.yaml && \
    docker exec -it tetragon-agent tetra tracingpolicy add /tmp/policy-exec.yaml

    docker cp policy-write.yaml tetragon-agent:/tmp/policy-write.yaml && \
    docker exec -it tetragon-agent tetra tracingpolicy add /tmp/policy-write.yaml

Test:

    # First console
    docker exec -it tetragon-agent tetra getevents -o compact
    
    # Second console
    docker exec -it tetragon-agent whoami

    docker run -it --rm alpine /bin/sh
    echo 1 >> /tmp/1.txt


How to write policies?
----------------------

1. Check [examples](https://github.com/cilium/tetragon/tree/main/examples)

2. Check fields and possible params in [policy specs](https://github.com/cilium/tetragon/blob/main/pkg/k8s/apis/cilium.io/client/crds/v1alpha1/cilium.io_tracingpolicies.yaml)

Another usefull resources:

* https://b-nova.com/en/home/content/strengthen-your-system-with-tetragons-ebpf-based-security-observability-and-runtime-enforcement-capabilities
* https://grsecurity.net/tetragone_a_lesson_in_security_fundamentals


Monitoring Tetragon
-------------------

Tetragon can be monitored via Prometheus. 

1. Run tetragon with `/usr/bin/tetragon --metrics-server 0.0.0.0:2112` entrypoint

2. Set `prometheus.yml`:

    scrape_configs:
    - job_name: node
        scrape_interval: 5s
        static_configs:
        - targets: ['tetragon-agent:2112']
