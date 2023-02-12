Audit
=====

Add lines in `/var/snap/microk8s/current/args/kube-apiserver`

    # define the path of the audit policy file
    --audit-policy-file=/etc/microk8s/policy.yaml

    # define the path where the server will be keeping the logs
    --audit-log-path=/var/log/microk8s/audit.log

    # the max age in days for keeping audit logs
    --audit-log-maxage=7

    # the max number of log files to be kept
    --audit-log-maxbackup=5

    # the max size of the log file in MB
    --audit-log-maxsize=1

Set config:
    
    sudo mkdir /etc/microk8s
    sudo cp manifests/audit/policy.yaml /etc/microk8s/policy.yaml

Restart service:

    sudo microk8s stop && sudo microk8s start
    
Set `custom-admin context` from article below

Make a request (you can use any commands):

    microk8s kubectl get nodes 

Check logs:

    sudo cat /var/log/microk8s/audit.log | grep custom

Usefull resources:

- Documentation - https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/
- Audit schema - https://kubernetes.io/docs/reference/config-api/apiserver-audit.v1/#audit-k8s-io-v1-Policy