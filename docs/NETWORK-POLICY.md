# Network policy

Used for filtering network trafic

## Usage

Install and check Nginx pod:

    kubectl apply -f ./manifests/network-policy/nginx-pod.yaml

    kubectl describe pod nginx-pod

    curl http://10.1.184.67 # Get IP from describe command

Install and check Curl pod:

    kubectl apply -f ./manifests/network-policy/curl-pod.yaml

    kubectl describe pod curl-tester-pod

Apply and check policy default deny policy:

    kubectl apply -f ./manifests/network-policy/nginx-deny.yaml

    kubectl exec --stdin --tty curl-tester-pod -- curl http://10.1.184.67

    # No output

Apply and check policy allow policy:

    kubectl apply -f ./manifests/network-policy/nginx-allow.yaml

    kubectl exec --stdin --tty curl-tester-pod -- curl http://10.1.184.67

    # Nginx welcome page output

## Clean up

    kubectl delete \
    -f ./manifests/network-policy/nginx-allow.yaml \
    -f ./manifests/network-policy/nginx-deny.yaml \
    -f ./manifests/network-policy/curl-pod.yaml \
    -f ./manifests/network-policy/nginx-pod.yaml