# Pods

## Creating a pod using imperative API

```console
PS C:\dev\projects\kubernetes-workshops> kubectl create -f .\3-pods\pod.yml
pod/simple-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po -w
NAME         READY   STATUS              RESTARTS   AGE
simple-pod   0/1     ContainerCreating   0          6s
simple-pod   1/1     Running             0          7s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl create -f .\3-pods\pod.yml
Error from server (AlreadyExists): error when creating ".\\3-pods\\pod.yml": pods "simple-pod" already exists
```

## Lack of durability

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete -f .\3-pods\pod.yml
pod "simple-pod" deleted
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
No resources found in default namespace.
```

## Creating a pod using declarative API

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\3-pods\pod.yml
pod/simple-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\3-pods\pod.yml
pod/simple-pod unchanged
```

## Investigating a pod

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pod simple-pod -o yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"name":"simple-pod","namespace":"default"},"spec":{"containers":[{"image":"stefanprodan/podinfo","name":"simple-pod"}]}}
  creationTimestamp: "2019-11-20T20:25:10Z"
  name: simple-pod
  namespace: default
  resourceVersion: "1226"
  selfLink: /api/v1/namespaces/default/pods/simple-pod
  uid: 91e58809-7374-4470-96ba-b0d50fabf35a
spec:
  containers:
  - image: stefanprodan/podinfo
    imagePullPolicy: Always
    name: simple-pod
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-9m45x
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: minikube
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: default-token-9m45x
    secret:
      defaultMode: 420
      secretName: default-token-9m45x
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2019-11-20T20:25:10Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2019-11-20T20:25:13Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2019-11-20T20:25:13Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2019-11-20T20:25:10Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: docker://7eb9034f0e1dc58b0a7da709933fe569fe657640157e94a3bfcfc752b3a2f97a
    image: stefanprodan/podinfo:latest
    imageID: docker-pullable://stefanprodan/podinfo@sha256:135d3a170246c6a5d68c6952cf8fc86666590a49d96aac9c207a3bdd4d996186
    lastState: {}
    name: simple-pod
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2019-11-20T20:25:12Z"
  hostIP: 192.168.99.103
  phase: Running
  podIP: 172.17.0.4
  podIPs:
  - ip: 172.17.0.4
  qosClass: BestEffort
  startTime: "2019-11-20T20:25:10Z"
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl describe pod simple-pod
Name:         simple-pod
Namespace:    default
Priority:     0
Node:         minikube/192.168.99.103
Start Time:   Wed, 20 Nov 2019 21:25:10 +0100
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"name":"simple-pod","namespace":"default"},"spec":{"containers":[{"image":"st...
Status:       Running
IP:           172.17.0.4
IPs:
  IP:  172.17.0.4
Containers:
  simple-pod:
    Container ID:   docker://7eb9034f0e1dc58b0a7da709933fe569fe657640157e94a3bfcfc752b3a2f97a
    Image:          stefanprodan/podinfo
    Image ID:       docker-pullable://stefanprodan/podinfo@sha256:135d3a170246c6a5d68c6952cf8fc86666590a49d96aac9c207a3bdd4d996186
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 20 Nov 2019 21:25:12 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-9m45x (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-9m45x:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-9m45x
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age        From               Message
  ----    ------     ----       ----               -------
  Normal  Scheduled  <unknown>  default-scheduler  Successfully assigned default/simple-pod to minikube
  Normal  Pulling    97s        kubelet, minikube  Pulling image "stefanprodan/podinfo"
  Normal  Pulled     95s        kubelet, minikube  Successfully pulled image "stefanprodan/podinfo"
  Normal  Created    95s        kubelet, minikube  Created container simple-pod
  Normal  Started    95s        kubelet, minikube  Started container simple-pod
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl logs simple-pod
{"level":"info","ts":"2019-11-20T20:25:12.400Z","caller":"podinfo/main.go:120","msg":"Starting podinfo","version":"3.1.5","revision":"78658c03110c23dc011bbec5801f58c6f3f14420","port":"9898"}
```

## Executing commands inside pods/containers

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it simple-pod sh
~ $ ls
podinfo  ui
~ $ ps
PID   USER     TIME  COMMAND
    1 app       0:00 ./podinfo
   12 app       0:00 sh
   20 app       0:00 ps
~ $ exit
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod simple-pod
pod "simple-pod" deleted
```

## Liveness probe

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\3-pods\liveness-pod.yml
pod/liveness-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po -w
NAME           READY   STATUS    RESTARTS   AGE
liveness-pod   1/1     Running   0          5s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po  
NAME           READY   STATUS    RESTARTS   AGE
liveness-pod   1/1     Running   2          39s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl describe pod liveness-pod
Name:         liveness-pod
Namespace:    default
Priority:     0
Node:         minikube/192.168.99.103
Start Time:   Wed, 20 Nov 2019 21:28:46 +0100
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"name":"liveness-pod","namespace":"default"},"spec":{"containers":[{"args":["...
Status:       Running
IP:           172.17.0.4
IPs:
  IP:  172.17.0.4
Containers:
  liveness-pod:
    Container ID:  docker://2cbf2ef5c9c69908912ae2db3743d1ccfe9e31fdfb9063cec24c4f0bded67c74
    Image:         k8s.gcr.io/liveness
    Image ID:      docker-pullable://k8s.gcr.io/liveness@sha256:1aef943db82cf1370d0504a51061fb082b4d351171b304ad194f6297c0bb726a
    Port:          <none>
    Host Port:     <none>
    Args:
      /server
    State:          Running
      Started:      Wed, 20 Nov 2019 21:29:42 +0100
    Last State:     Terminated
      Reason:       Error
      Exit Code:    2
      Started:      Wed, 20 Nov 2019 21:29:24 +0100
      Finished:     Wed, 20 Nov 2019 21:29:41 +0100
    Ready:          True
    Restart Count:  3
    Liveness:       http-get http://:8080/healthz delay=3s timeout=1s period=3s #success=1 #failure=3
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-9m45x (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-9m45x:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-9m45x
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason     Age                From               Message
  ----     ------     ----               ----               -------
  Normal   Scheduled  <unknown>          default-scheduler  Successfully assigned default/liveness-pod to minikube
  Normal   Pulled     47s (x3 over 82s)  kubelet, minikube  Successfully pulled image "k8s.gcr.io/liveness"
  Normal   Created    47s (x3 over 82s)  kubelet, minikube  Created container liveness-pod
  Normal   Started    47s (x3 over 82s)  kubelet, minikube  Started container liveness-pod
  Normal   Pulling    30s (x4 over 85s)  kubelet, minikube  Pulling image "k8s.gcr.io/liveness"
  Warning  Unhealthy  30s (x9 over 72s)  kubelet, minikube  Liveness probe failed: HTTP probe failed with statuscode: 500
  Normal   Killing    30s (x3 over 66s)  kubelet, minikube  Container liveness-pod failed liveness probe, will be restarted
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po
NAME           READY   STATUS             RESTARTS   AGE
liveness-pod   0/1     CrashLoopBackOff   3          89s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod liveness-pod
pod "liveness-pod" deleted
```

## Readiness probe

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\3-pods\readiness-pod.yml
pod/readiness-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po -w
NAME            READY   STATUS    RESTARTS   AGE
readiness-pod   0/1     Running   0          3s
readiness-pod   1/1     Running   0          8s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it readiness-pod -- curl localhost:9898/readyz
{
  "status": "OK"
}
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it readiness-pod -- curl -X POST localhost:9898/readyz/disable
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it readiness-pod -- curl localhost:9898/readyz -v
*   Trying 127.0.0.1:9898...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 9898 (#0)
> GET /readyz HTTP/1.1
> Host: localhost:9898
> User-Agent: curl/7.66.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 503 Service Unavailable
< Date: Wed, 13 Nov 2019 20:18:16 GMT
< Content-Length: 0
<
* Connection #0 to host localhost left intact
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po -w
NAME            READY   STATUS    RESTARTS   AGE
readiness-pod   1/1     Running   0          81s
readiness-pod   0/1     Running   0          93s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it readiness-pod -- curl -X POST localhost:9898/readyz/enable
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po -w
NAME            READY   STATUS    RESTARTS   AGE
readiness-pod   0/1     Running   0          109s
readiness-pod   1/1     Running   0          113s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod readiness-pod
pod "readiness-pod" deleted
```

## Assigning CPU and memory resources to pods

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\3-pods\resources-pod.yml
pod/resources-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME            READY   STATUS    RESTARTS   AGE
resources-pod   1/1     Running   0          89s
```

```console
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
CreationTimestamp:  Mon, 18 Nov 2019 20:34:36 +0100
Taints:             <none>
Unschedulable:      false
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Mon, 18 Nov 2019 20:52:42 +0100   Mon, 18 Nov 2019 20:34:31 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Mon, 18 Nov 2019 20:52:42 +0100   Mon, 18 Nov 2019 20:34:31 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Mon, 18 Nov 2019 20:52:42 +0100   Mon, 18 Nov 2019 20:34:31 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Mon, 18 Nov 2019 20:52:42 +0100   Mon, 18 Nov 2019 20:34:31 +0100   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.99.101
  Hostname:    minikube
Capacity:
 cpu:                2
 ephemeral-storage:  17784772Ki
 hugepages-2Mi:      0
 memory:             1985620Ki
 pods:               110
Allocatable:
 cpu:                2
 ephemeral-storage:  17784772Ki
 hugepages-2Mi:      0
 memory:             1985620Ki
 pods:               110
System Info:
 Machine ID:                 ca04266e63774528ad34b118474eb652
 System UUID:                c5b254ae-d0e1-4038-9a0f-f60d4297f362
 Boot ID:                    db7c83a5-796e-4d9f-9c28-c000fa7e48bd
 Kernel Version:             4.19.76
 OS Image:                   Buildroot 2019.02.6
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://18.9.9
 Kubelet Version:            v1.16.2
 Kube-Proxy Version:         v1.16.2
Non-terminated Pods:         (10 in total)
  Namespace                  Name                                CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                ------------  ----------  ---------------  -------------  ---
  default                    resources-pod                       1m (0%)       10m (0%)    16Mi (0%)        128Mi (6%)     2m17s
  kube-system                coredns-5644d7b6d9-f2gfx            100m (5%)     0 (0%)      70Mi (3%)        170Mi (8%)     18m
  kube-system                coredns-5644d7b6d9-zjhpr            100m (5%)     0 (0%)      70Mi (3%)        170Mi (8%)     18m
  kube-system                etcd-minikube                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         17m
  kube-system                kube-addon-manager-minikube         5m (0%)       0 (0%)      50Mi (2%)        0 (0%)         18m
  kube-system                kube-apiserver-minikube             250m (12%)    0 (0%)      0 (0%)           0 (0%)         17m
  kube-system                kube-controller-manager-minikube    200m (10%)    0 (0%)      0 (0%)           0 (0%)         17m
  kube-system                kube-proxy-zg895                    0 (0%)        0 (0%)      0 (0%)           0 (0%)         18m
  kube-system                kube-scheduler-minikube             100m (5%)     0 (0%)      0 (0%)           0 (0%)         17m
  kube-system                storage-provisioner                 0 (0%)        0 (0%)      0 (0%)           0 (0%)         18m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests     Limits
  --------           --------     ------
  cpu                756m (37%)   10m (0%)
  memory             206Mi (10%)  468Mi (24%)
  ephemeral-storage  0 (0%)       0 (0%)
Events:
  Type    Reason                   Age                From                  Message
  ----    ------                   ----               ----                  -------
  Normal  NodeHasSufficientMemory  18m (x8 over 18m)  kubelet, minikube     Node minikube status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    18m (x7 over 18m)  kubelet, minikube     Node minikube status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     18m (x8 over 18m)  kubelet, minikube     Node minikube status is now: NodeHasSufficientPID
  Normal  Starting                 18m                kube-proxy, minikube  Starting kube-proxy.
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod resources-pod
pod "resources-pod" deleted
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\3-pods\greedy-pod.yml
pod/greedy-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME         READY   STATUS    RESTARTS   AGE
greedy-pod   0/1     Pending   0          3s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl describe pod greedy-pod
Name:         greedy-pod
Namespace:    default
Priority:     0
Node:         <none>
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"name":"greedy-pod","namespace":"default"},"spec":{"containers":[{"image":"st...
Status:       Pending
IP:
IPs:          <none>
Containers:
  greedy-pod:
    Image:      stefanprodan/podinfo
    Port:       <none>
    Host Port:  <none>
    Requests:
      cpu:        4
      memory:     4Gi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-4zv4b (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  default-token-4zv4b:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-4zv4b
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age        From               Message
  ----     ------            ----       ----               -------
  Warning  FailedScheduling  <unknown>  default-scheduler  0/1 nodes are available: 1 Insufficient cpu, 1 Insufficient memory.
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod greedy-pod
pod "greedy-pod" deleted
```

## Forwarding port to a pod

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\pods\pod.yml
pod/simple-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl port-forward pod/simple-pod 9898:9898
Forwarding from 127.0.0.1:9898 -> 9898
Forwarding from [::1]:9898 -> 9898
Handling connection for 9898
```

`localhost:9898`

![podinfo frontend](podinfo.png)

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod simple-pod
pod "simple-pod" deleted
```
