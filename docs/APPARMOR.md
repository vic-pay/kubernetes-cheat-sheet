Apparmor
========

Deploy Pod with default profile
-------------------------------

Create pod:

    microk8s kubectl apply -f ./manifests/apparmor/pod.yaml

Test:

    microk8s kubectl exec nginx-name -- cat /proc/1/attr/current
    # Output
    # cri-containerd.apparmor.d (enforce)

Delete pod:

    microk8s kubectl delete -f ./manifests/apparmor/pod.yaml

Default profile rules
---------------------

    cat /etc/apparmor.d/cri-containerd.apparmor.d

<details>
  <summary>Content</summary>

    #include <tunables/global>

    profile cri-containerd.apparmor.d flags=(attach_disconnected,mediate_deleted) {

        #include <abstractions/base>

        network,
        capability,
        file,
        umount,

        deny @{PROC}/* w,   # deny write for all files directly in /proc (not in a subdir)
        # deny write to files not in /proc/<number>/** or /proc/sys/**
        deny @{PROC}/{[^1-9],[^1-9][^0-9],[^1-9s][^0-9y][^0-9s],[^1-9][^0-9][^0-9][^0-9]*}/** w,
        deny @{PROC}/sys/[^k]** w,  # deny /proc/sys except /proc/sys/k* (effectively /proc/sys/kernel)
        deny @{PROC}/sys/kernel/{?,??,[^s][^h][^m]**} w,  # deny everything except shm* in /proc/sys/kernel/
        deny @{PROC}/sysrq-trigger rwklx,
        deny @{PROC}/mem rwklx,
        deny @{PROC}/kmem rwklx,
        deny @{PROC}/kcore rwklx,

        deny mount,

        deny /sys/[^f]*/** wklx,
        deny /sys/f[^s]*/** wklx,
        deny /sys/fs/[^c]*/** wklx,
        deny /sys/fs/c[^g]*/** wklx,
        deny /sys/fs/cg[^r]*/** wklx,
        deny /sys/firmware/efi/efivars/** rwklx,
        deny /sys/kernel/security/** rwklx,

        # suppress ptrace denials when using 'docker ps' or using 'ps' inside a container
        ptrace (trace,read) peer=cri-containerd.apparmor.d,

        signal (receive) peer=snap.microk8s.daemon-kubelite,
        signal (receive) peer=snap.microk8s.daemon-containerd,
    }
</details>

Develop custom Apparmor profile
-------------------------------

1) Make and load "deny all" profile in `complain` mode
2) Link profile to container

        annotations:
            container.apparmor.security.beta.kubernetes.io/nginx-pod: localhost/<profile_name>

3) Use `aa-logprof` to update profile

        aa-logprof [-m <mark in logfile>]

Usefull links
-------------

- https://kubernetes.io/docs/tutorials/security/apparmor/