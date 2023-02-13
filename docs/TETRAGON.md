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

    docker cp policy-exec.yaml tetragon-agent:/tmp/policy-exec.yaml

    docker exec -it tetragon-agent tetra tracingpolicy add /tmp/policy-exec.yaml

Test:

    # First console
    docker exec -it tetragon-agent tetra getevents -o compact
    
    # Second console
    docker exec -it tetragon-agent whoami


Usefull resources:

* https://b-nova.com/en/home/content/strengthen-your-system-with-tetragons-ebpf-based-security-observability-and-runtime-enforcement-capabilities
* https://grsecurity.net/tetragone_a_lesson_in_security_fundamentals