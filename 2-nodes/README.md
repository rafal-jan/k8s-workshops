# Nodes

## Minikube

```powershell
PS C:\dev\projects\kubernetes-workshops> kubectl get nodes
NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   6d23h   v1.16.2
```

```powershell
PS C:\dev\projects\kubernetes-workshops> kubectl describe node minikube
Name:               minikube
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=minikube
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Mon, 11 Nov 2019 19:50:36 +0100
Taints:             <none>
Unschedulable:      false
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Mon, 18 Nov 2019 19:27:35 +0100   Mon, 11 Nov 2019 19:50:31 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Mon, 18 Nov 2019 19:27:35 +0100   Mon, 11 Nov 2019 19:50:31 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Mon, 18 Nov 2019 19:27:35 +0100   Mon, 11 Nov 2019 19:50:31 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Mon, 18 Nov 2019 19:27:35 +0100   Mon, 11 Nov 2019 19:50:31 +0100   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  172.17.246.13
  Hostname:    minikube
Capacity:
 cpu:                2
 ephemeral-storage:  17784772Ki
 hugepages-2Mi:      0
 memory:             1987656Ki
 pods:               110
Allocatable:
 cpu:                2
 ephemeral-storage:  17784772Ki
 hugepages-2Mi:      0
 memory:             1987656Ki
 pods:               110
System Info:
 Machine ID:                 312d9c0c631140cbb8f2da9c5d3b190b
 System UUID:                b60d2bde-bebe-2c4f-8c6e-b4ba6d35beb8
 Boot ID:                    4d773505-8b05-4e0c-82f0-89e48ebbf777
 Kernel Version:             4.19.76
 OS Image:                   Buildroot 2019.02.6
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://18.9.9
 Kubelet Version:            v1.16.2
 Kube-Proxy Version:         v1.16.2
Non-terminated Pods:         (9 in total)
  Namespace                  Name                                CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                ------------  ----------  ---------------  -------------  ---
  kube-system                coredns-5644d7b6d9-fd7sl            100m (5%)     0 (0%)      70Mi (3%)        170Mi (8%)     6d23h
  kube-system                coredns-5644d7b6d9-mcdzm            100m (5%)     0 (0%)      70Mi (3%)        170Mi (8%)     6d23h
  kube-system                etcd-minikube                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         24m
  kube-system                kube-addon-manager-minikube         5m (0%)       0 (0%)      50Mi (2%)        0 (0%)         6d23h
  kube-system                kube-apiserver-minikube             250m (12%)    0 (0%)      0 (0%)           0 (0%)         24m
  kube-system                kube-controller-manager-minikube    200m (10%)    0 (0%)      0 (0%)           0 (0%)         6d23h
  kube-system                kube-proxy-7bsvv                    0 (0%)        0 (0%)      0 (0%)           0 (0%)         6d23h
  kube-system                kube-scheduler-minikube             100m (5%)     0 (0%)      0 (0%)           0 (0%)         6d23h
  kube-system                storage-provisioner                 0 (0%)        0 (0%)      0 (0%)           0 (0%)         6d23h
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                755m (37%)  0 (0%)
  memory             190Mi (9%)  340Mi (17%)
  ephemeral-storage  0 (0%)      0 (0%)
Events:
  Type    Reason                   Age                From                  Message
  ----    ------                   ----               ----                  -------
  Normal  Starting                 24m                kubelet, minikube     Starting kubelet.
  Normal  NodeHasSufficientMemory  24m (x8 over 24m)  kubelet, minikube     Node minikube status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    24m (x7 over 24m)  kubelet, minikube     Node minikube status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     24m (x8 over 24m)  kubelet, minikube     Node minikube status is now: NodeHasSufficientPID
  Normal  NodeAllocatableEnforced  24m                kubelet, minikube     Updated Node Allocatable limit across pods
  Normal  Starting                 24m                kube-proxy, minikube  Starting kube-proxy.
```

## Multi-node K8s cluster
