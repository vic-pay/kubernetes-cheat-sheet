Access control
==============

Create account, role and binding:

    microk8s kubectl apply -f ./manifests/rbac/service-account.yaml
    microk8s kubectl apply -f ./manifests/rbac/role.yaml
    microk8s kubectl apply -f ./manifests/rbac/binding.yaml

Check settings:

    microk8s kubectl get serviceaccounts
    microk8s kubectl describe serviceaccount custom-admin
    microk8s kubectl get roles
    microk8s kubectl describe role custom-admin
    microk8s kubectl get rolebindings
    microk8s kubectl describe rolebinding custom-admin

Generate token and make context:

    microk8s kubectl create token custom-admin
    microk8s kubectl config set-credentials custom-admin --token={{ token }}
    microk8s kubectl config set-context custom-admin --cluster=microk8s-cluster --user=custom-admin
    microk8s kubectl config use-context custom-admin

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

