
# Set up a Kubernetes cluster

## General
[Kind](https://github.com/kubernetes-sigs/kind) will be used as the k8s runtime environment as it can easily run on all environments, and does not require any complicated steps to setup.

## Install Kind

See: [installing Kind](https://kind.sigs.k8s.io/docs/user/quick-start/)

## Create Kind Demo Cluster

Use kind to create the customized cluster:

```console
kind create cluster --config=env/cluster.yaml --name=demo --image=docker.io/kindest/node:v1.16.3@sha256:70ce6ce09bee5c34ab14aec2b84d6edb260473a60638b1b095470a3a0f95ebec
```
NOTE: The latest version of kind and the kind container version can be found [here](https://github.com/kubernetes-sigs/kind/releases)

## Set up the Kubernetes environment 

Install Calico for `NetworkPolicy` support:

```console
kubectl apply -f env/calico.yaml
```
Install Helm tiller on kube-system 

```console
kubectl apply -f kind/tiller.yaml
```

## Verifying the cluster is ready:

```console
kubectl get pods -A
```

Should show:

```console
NAMESPACE     NAME                                         READY   STATUS    RESTARTS   AGE
kube-system   calico-kube-controllers-648f4868b8-t6g28     1/1     Running   0          74s
kube-system   calico-node-kkt2g                            1/1     Running   0          74s
kube-system   calico-node-pr7cc                            1/1     Running   0          74s
kube-system   coredns-5644d7b6d9-rz4bn                     1/1     Running   0          3m28s
kube-system   coredns-5644d7b6d9-sb2jx                     1/1     Running   0          3m28s
kube-system   etcd-demo-control-plane                      1/1     Running   0          2m19s
kube-system   kube-apiserver-demo-control-plane            1/1     Running   0          2m19s
kube-system   kube-controller-manager-demo-control-plane   1/1     Running   0          2m27s
kube-system   kube-proxy-2mjzn                             1/1     Running   0          3m14s
kube-system   kube-proxy-ghj7w                             1/1     Running   0          3m28s
kube-system   kube-scheduler-demo-control-plane            1/1     Running   0          2m35s
kube-system   tiller-deploy-969865475-9xh24                1/1     Running   0          71s
```

## Environment Cleanup 
In order to cleanup the environment you can simply delete the kind cluster by running the fallowing command:

```console
kind delete cluster --name demo
```