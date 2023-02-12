Preparation steps
=================

Install MicroK8S:

    sudo snap install microk8s --classic

Set permissions:

    sudo usermod -a -G microk8s {{ username }}
    sudo chown -f -R {{ username }} ~/.kube

Check MicroK8S status:

    microk8s status --wait-ready
    microk8s kubectl cluster-info

Add dashboard (Optional):

    microk8s dashboard-proxy
    microk8s enable dashboard
