Kubernetes cheat sheet
======================

This repository contains usefull commands for learning Kubernetes.


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


Basic commands
==============

Working with Pods
-----------------

Create pod:

    microk8s kubectl apply -f ./pod/init.yaml

Pod info:

    microk8s kubectl get pods
    microk8s kubectl describe pod nginx-name

Change pod (for example update pod image):

    microk8s kubectl apply -f ./pod/upgrade.yaml

    microk8s kubectl patch pod nginx-name -p '{"spec":{"containers":[{"name": "nginx-pod","image": "nginx:1.15.0"}]}}'

    KUBE_EDITOR=mcedit microk8s kubectl edit pod/nginx-name -o yaml 

Delete pod:

    microk8s kubectl delete pod nginx-name


Working with Deployments
------------------------

Create deployment:

    microk8s kubectl apply -f ./deplotment/init.yaml

Deployment info:

    microk8s kubectl get deployments
    microk8s kubectl describe deployment nginx-name
    microk8s kubectl get pods -l app=nginx-app

Change deployment:

    microk8s kubectl set image deployment/nginx-name nginx-pod=nginx:1.15.0

    KUBE_EDITOR=mcedit microk8s kubectl edit deployment/nginx-name -o yaml

    microk8s kubectl scale --replicas=3 deployment/nginx-name

    microk8s kubectl rollout status deployment/nginx-name
    microk8s kubectl rollout history deployment/nginx-name
    microk8s kubectl rollout history deployment/nginx-name  --revision=1
    microk8s kubectl rollout undo deployment/nginx-name
    microk8s kubectl rollout undo deployment/nginx-name --to-revision=1

Delete pod:

    microk8s kubectl delete deployment nginx-name


Another usefull commands
------------------------

Resources types:

    microk8s kubectl api-resources

Logs:

    microk8s kubectl logs nginx-name
    microk8s kubectl logs nginx-name -f
    microk8s kubectl logs -l app=nginx-app --all-containers -f

Processes:

    microk8s kubectl top pod -l app=nginx-app --containers 

Execute commands in containers:

    microk8s kubectl attach nginx-name -i
    microk8s kubectl exec nginx-name -- ls /

Port forwarding:

    microk8s kubectl port-forward nginx-name 8080:80
    # Check on another tab: 
    curl http://127.0.0.1:8080


Working with Services
---------------------

    microk8s kubectl apply -f ./service/init.yaml
    microk8s kubectl get services
    microk8s kubectl describe service service-name
    microk8s kubectl delete service service-name


K8S Security
============

Access control
--------------

https://kubernetes-tutorial.schoolofdevops.com/configuring_authentication_and_authorization/

Create account, role and binding:

    microk8s kubectl apply -f ./rbac/service-account.yaml
    microk8s kubectl apply -f ./rbac/role.yaml
    microk8s kubectl apply -f ./rbac/binding.yaml

Check settings:

    microk8s kubectl get serviceaccount custom-admin
    microk8s kubectl describe serviceaccount custom-admin
    microk8s kubectl get roles
    microk8s kubectl describe role custom-admin
    microk8s kubectl get rolebindings
    microk8s kubectl describe rolebinding custom-admin

Generate token and make context:

    microk8s kubectl create token custom-admin
    microk8s kubectl config set-credentials custom-admin --token={{ token }}
    microk8s kubectl config set-context custom-admin --cluster=microk8s-cluster --user=custom-admin

Check the functionality (you can use any commands):

    microk8s kubectl get nodes

Check settings:

    microk8s config
    microk8s kubectl config view
    microk8s kubectl config get-users
    microk8s kubectl config get-contexts

Delete resources:

    microk8s kubectl config use-context microk8s

    microk8s kubectl delete serviceaccount custom-admin
    microk8s kubectl delete rolebinding custom-admin
    microk8s kubectl delete role custom-admin

    microk8s kubectl config delete-context custom-admin
    microk8s kubectl config delete-user custom-admin

Usefull resources:

- https://kubernetes-tutorial.schoolofdevops.com/configuring_authentication_and_authorization/
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- https://kubernetes.io/docs/reference/access-authn-authz/authentication/#service-account-tokens
