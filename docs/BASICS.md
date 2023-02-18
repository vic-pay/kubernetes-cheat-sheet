Basic commands
==============

Working with Pods
-----------------

Create pod:

    microk8s kubectl apply -f ./manifests/pod/init.yaml

Pod info:

    microk8s kubectl get pods
    microk8s kubectl describe pod nginx-name

Change pod (for example update pod image):

    microk8s kubectl apply -f ./manifests/pod/upgrade.yaml

    microk8s kubectl patch pod nginx-name -p '{"spec":{"containers":[{"name": "nginx-pod","image": "nginx:1.15.0"}]}}'

    KUBE_EDITOR=mcedit microk8s kubectl edit pod/nginx-name -o yaml 

Delete pod:

    microk8s kubectl delete pod nginx-name


Working with Deployments
------------------------

Create deployment:

    microk8s kubectl apply -f ./manifests/deployment/init.yaml

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

    microk8s kubectl apply -f ./manifests/service/init.yaml
    microk8s kubectl get services
    microk8s kubectl describe service service-name
    microk8s kubectl delete service service-name
