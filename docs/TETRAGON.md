Cillum Tetragon
===============

Tetragon - eBPF-based Security Observability and Runtime Enforcement

Tetragon can be used in K8S and standalone

[Github](https://github.com/cilium/tetragon)


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

1. Check [examples](https://github.com/cilium/tetragon/tree/main/crds/examples)

2. Check fields and possible params in [policy specs](https://github.com/cilium/tetragon/blob/main/pkg/k8s/apis/cilium.io/client/crds/v1alpha1/cilium.io_tracingpolicies.yaml)

Another usefull resources:

* https://b-nova.com/en/home/content/strengthen-your-system-with-tetragons-ebpf-based-security-observability-and-runtime-enforcement-capabilities
* https://grsecurity.net/tetragone_a_lesson_in_security_fundamentals