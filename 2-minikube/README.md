# minikube

```console
~/k8s-workshops $ minikube --help
Minikube is a CLI tool that provisions and manages single-node Kubernetes clusters optimized for development workflows.
...
```

```console
~/k8s-workshops $ minikube status
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

```console
~/k8s-workshops $ minikube ip
192.168.99.108
```

```console
~/k8s-workshops $ kubectl get nodes -o wide
NAME       STATUS   ROLES    AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE              KERNEL-VERSION   CONTAINER-RUNTIME
minikube   Ready    master   9m    v1.16.2   192.168.99.108   <none>        Buildroot 2019.02.6   4.19.76          docker://18.9.9
```

```console
~/k8s-workshops $ kubectl describe node minikube
Name:               minikube
Roles:              master
...
```

## Multi-node K8s cluster
