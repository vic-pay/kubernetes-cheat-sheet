Access control
==============

Enable rbac:

    microk8s enable rbac

Create account:

    microk8s kubectl apply -f ./manifests/rbac/namespace.yaml
    microk8s kubectl apply -f ./manifests/rbac/account.yaml

    microk8s kubectl get serviceaccounts -n dev
    microk8s kubectl describe serviceaccount dev-admin -n dev

# Cluster-admin via ClusterRoles

Set role:

    microk8s kubectl apply -f ./manifests/rbac/binding_cluster.yaml

Check settings:

    microk8s kubectl describe clusterrole cluser-admin
    microk8s kubectl get clusterrolebindings
    microk8s kubectl describe clusterrolebinding dev-admin

    kubectl auth can-i list nodes --as=system:serviceaccount:dev:dev-admin
    kubectl auth can-i get  nodes --as=system:serviceaccount:dev:dev-admin
    kubectl auth can-i --list --as=system:serviceaccount:dev:dev-admin
    kubectl auth can-i --list --as=system:serviceaccount:dev:dev-admin -n dev

# Admin of dev namespace

Set role:

    microk8s kubectl apply -f ./manifests/rbac/role.yaml
    microk8s kubectl apply -f ./manifests/rbac/binding_role.yaml

Check settings:

    microk8s kubectl get roles -n dev
    microk8s kubectl describe role dev-admin -n dev
    microk8s kubectl get rolebindings -n dev
    microk8s kubectl describe rolebinding dev-admin -n dev

# Use new account

Generate token and make context:

    microk8s kubectl create token dev-admin
    microk8s kubectl config set-credentials dev-admin --token={{ token }}
    microk8s kubectl config set-context dev-admin --cluster=microk8s-cluster --user=dev-admin
    microk8s kubectl config use-context dev-admin

Check the functionality (you can use any commands):

    microk8s kubectl get nodes

    KUBE_EDITOR="mcedit" kubectl edit clusterrolebinding dev-admin

Check settings:

    microk8s config
    microk8s kubectl config view
    microk8s kubectl config get-users
    microk8s kubectl config get-contexts

# Delete resources

    microk8s kubectl config use-context microk8s

    microk8s kubectl delete serviceaccount dev-admin
    microk8s kubectl delete clusterrolebinding dev-admin
    microk8s kubectl delete clusterrole dev-admin
    microk8s kubectl delete rolebinding dev-admin -n dev
    microk8s kubectl delete role dev-admin -n dev
    microk8s kubectl delete namespace dev

    microk8s kubectl config delete-context dev-admin
    microk8s kubectl config delete-user dev-admin

Usefull resources:

- https://kubernetes-tutorial.schoolofdevops.com/configuring_authentication_and_authorization/
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- https://kubernetes.io/docs/reference/access-authn-authz/authentication/#service-account-tokens

