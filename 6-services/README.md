# Services

## ClusterIP

```console
~/k8s-workshops $ kubectl apply -f 4-deployments/deploy.yml
deployment.apps/simple-deploy created
```

```console
~/k8s-workshops $ kubectl scale deploy simple-deploy --replicas=2
deployment.apps/simple-deploy scaled
```

```console
~/k8s-workshops $ kubectl get pods -o wide
NAME                             READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
simple-deploy-7cf9979c88-28x6n   1/1     Running   0          27s   172.17.0.4   minikube   <none>           <none>
simple-deploy-7cf9979c88-ww95j   1/1     Running   0          23s   172.17.0.5   minikube   <none>           <none>
```

```console
~/k8s-workshops $ kubectl apply -f 3-pods/pod.yml
pod/simple-pod created
```

```console
~/k8s-workshops $ kubectl apply -f 6-services/clusterip.yml
service/simple-cluster-ip created
```

```console
~/k8s-workshops $ kubectl get services
NAME                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
kubernetes          ClusterIP   10.96.0.1        <none>        443/TCP    3d23h
simple-cluster-ip   ClusterIP   10.102.155.244   <none>        9898/TCP   7s
```

```console
~/k8s-workshops $ kubectl get svc simple-cluster-ip -o yaml
apiVersion: v1
kind: Service
...
```

```console
~/k8s-workshops $ kubectl get endpoints
NAME                ENDPOINTS                         AGE
kubernetes          192.168.99.106:8443               3d23h
simple-cluster-ip   172.17.0.4:9898,172.17.0.5:9898   30s
```

```console
~/k8s-workshops $ kubectl describe endpoints simple-cluster-ip
Name:         simple-cluster-ip
...
Subsets:
  Addresses:          172.17.0.4,172.17.0.5
  NotReadyAddresses:  <none>
...
```

```console
~/k8s-workshops $ kubectl exec -it simple-pod -- curl simple-cluster-ip:9898
{
  "hostname": "simple-deploy-7cf9979c88-ww95j",
...
```

```console
~/k8s-workshops $ kubectl exec -it simple-pod -- curl simple-cluster-ip:9898
{
  "hostname": "simple-deploy-7cf9979c88-28x6n",
...
```

```console
~/k8s-workshops $ kubectl delete deployment simple-deploy
deployment.apps "simple-deploy" deleted
```

## Readiness probe

```console
~/k8s-workshops $ kubectl apply -f 6-services/readiness-deploy.yml
deployment.apps/simple-deploy created
```

```console
~/k8s-workshops $ kubectl get po -w
NAME         READY   STATUS    RESTARTS   AGE
...
simple-deploy-7d7ffbb88-x9l4s   0/1     Running             0          4s
simple-deploy-7d7ffbb88-x9l4s   1/1     Running             0          12s
```

```console
~/k8s-workshops $ kubectl exec -it simple-deploy-7d7ffbb88-x9l4s -- curl localhost:9898/readyz
{
  "status": "OK"
}
```

```console
~/k8s-workshops $ kubectl exec -it simple-deploy-7d7ffbb88-x9l4s -- curl -X POST localhost:9898/readyz/disable
```

```console
~/k8s-workshops $ kubectl get po -w
NAME                            READY   STATUS    RESTARTS   AGE
simple-deploy-7d7ffbb88-x9l4s   1/1     Running   0          5m
simple-deploy-7d7ffbb88-x9l4s   0/1     Running   0          5m32s
```

```console
~/k8s-workshops $ kubectl exec -it simple-deploy-7d7ffbb88-x9l4s -- curl localhost:9898/readyz -v
...
< HTTP/1.1 503 Service Unavailable
< Date: Sun, 08 Dec 2019 18:30:41 GMT
< Content-Length: 0
<
* Connection #0 to host localhost left intact
```

```console
~/k8s-workshops $ kubectl describe endpoints simple-cluster-ip
Name:         simple-cluster-ip
...
Subsets:
  Addresses:          <none>
  NotReadyAddresses:  172.17.0.4
...
```

```console
~/k8s-workshops $ kubectl exec -it simple-pod -- curl simple-cluster-ip:9898
curl: (7) Failed to connect to simple-cluster-ip port 9898: Connection refused
command terminated with exit code 7
```

```console
~/k8s-workshops $ kubectl exec -it simple-deploy-7d7ffbb88-x9l4s -- curl -X POST localhost:9898/readyz/enable
```

```console
~/k8s-workshops $ kubectl get po -w
NAME                            READY   STATUS    RESTARTS   AGE
simple-deploy-7d7ffbb88-x9l4s   0/1     Running   0          5m32s
simple-deploy-7d7ffbb88-x9l4s   1/1     Running   0          6m47s
```

```console
~/k8s-workshops $ kubectl exec -it simple-pod -- curl simple-cluster-ip:9898
{
  "hostname": "simple-deploy-7d7ffbb88-x9l4s",
...
```

```console
~/k8s-workshops $ kubectl delete svc simple-cluster-ip
service "simple-cluster-ip" deleted
```

## NodePort

```console
~/k8s-workshops $ kubectl apply -f 6-services/nodeport.yml
service/simple-node-port created
```

```console
~/k8s-workshops $ kubectl get svc
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes         ClusterIP   10.96.0.1       <none>        443/TCP          3d22h
simple-node-port   NodePort    10.103.39.176   <none>        9898:30000/TCP   3s
```

```console
~/k8s-workshops $ minikube ssh curl localhost:30000
{
  "hostname": "simple-deploy-7d7ffbb88-x9l4s",
...
```

```console
~/k8s-workshops $ minikube ip
192.168.99.106
```

`http://192.168.99.106:30000/`

```console
~/k8s-workshops $ kubectl delete svc simple-node-port
service "simple-node-port" deleted
```
