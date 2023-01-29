KubeArmor
=========

Preparation

    sudo snap alias microk8s.kubectl kubectl
    microk8s kubectl config view --raw | tee $HOME/.kube/config
    sudo apt install linux-tools-generic

Download https://github.com/kubearmor/KubeArmor/releases
Install
    
    sudo apt install ./kubearmor_${VER}_linux-amd64.deb
    sudo systemctl start kubearmor

    karmor install
    kubectl apply -f kubearmor/policy_process_whoami.yaml
    kubectl apply -f kubearmor/policy_network_ping.yaml

Test:

    kubectl apply -f ./pod/init.yaml
    kubectl get pods

    kubectl exec -it nginx-name -- bash
    ping 8.8.8.8
    whoami

    karmor logs --json

usefull resources:

- https://github.com/kubearmor/KubeArmor/blob/main/getting-started/kubearmor_vm.md
- https://docs.kubearmor.com/kubearmor/specification/host_security_policy_specification