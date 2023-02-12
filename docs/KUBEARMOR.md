KubeArmor
=========

Preparation

    sudo snap alias microk8s.kubectl kubectl
    microk8s kubectl config view --raw | tee $HOME/.kube/config
    sudo apt install linux-tools-generic

Download https://github.com/kubearmor/KubeArmor/releases

Install package with commands:
    
    sudo apt install ./kubearmor_${VER}_linux-amd64.deb
    sudo systemctl start kubearmor
    karmor install

Apply policies:

    kubectl apply -f manifests/kubearmor/policy_process_whoami.yaml
    kubectl apply -f manifests/kubearmor/policy_network_ping.yaml

Test policy:

    kubectl apply -f ./manifests/pod/init.yaml
    kubectl get pods

    kubectl exec -it nginx-name -- bash
    ping 8.8.8.8
    whoami

    karmor logs --json

Delete assets:

    kubectl delete -f manifests/pod/init.yaml
    kubectl delete -f manifests/kubearmor/policy_network_ping.yaml 
    kubectl delete -f manifests/kubearmor/policy_process_whoami.yaml 

    karmor uninstall

    sudo apt purge kubearmor
    

Usefull resources:

- https://github.com/kubearmor/KubeArmor/blob/main/getting-started/kubearmor_vm.md
- https://docs.kubearmor.com/kubearmor/specification/host_security_policy_specification