# Services

## Expose a deployment

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\4-deployments\deploy.yml
deployment.apps/simple-deploy created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl expose deploy simple-deploy --port 9898
service/simple-deploy exposed
```

```console
C:\dev\projects\kubernetes-workshops> kubectl get svc
NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
kubernetes      ClusterIP   10.96.0.1        <none>        443/TCP    23h
simple-deploy   ClusterIP   10.110.128.233   <none>        9898/TCP   6s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get svc simple-deploy -o yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2019-11-20T21:14:11Z"
  labels:
    app: simple
  name: simple-deploy
  namespace: default
  resourceVersion: "5079"
  selfLink: /api/v1/namespaces/default/services/simple-deploy
  uid: 8af58cdf-053a-4d66-9a0e-d1538b514c45
spec:
  clusterIP: 10.110.128.233
  ports:
  - port: 9898
    protocol: TCP
    targetPort: 9898
  selector:
    app: simple
  sessionAffinity: None
  type: ClusterIP
status:
  loadBalancer: {}
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\3-pods\pod.yml
pod/simple-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it simple-pod -- curl simple-deploy:9898/
{
  "hostname": "simple-deploy-7cf9979c88-qhmbm",
  "version": "3.1.5",
  "revision": "78658c03110c23dc011bbec5801f58c6f3f14420",
  "color": "cyan",
  "logo": "https://raw.githubusercontent.com/stefanprodan/podinfo/gh-pages/cuddle_clap.gif",
  "message": "greetings from podinfo v3.1.5",
  "goos": "linux",
  "goarch": "amd64",
  "runtime": "go1.13.4",
  "num_goroutine": "6",
  "num_cpu": "2"
}
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete svc simple-deploy
service "simple-deploy" deleted
```

## Create a headless service

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
simple-deploy-7cf9979c88-qhmbm   1/1     Running   0          105s
simple-pod                       1/1     Running   0          40s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl scale deploy simple-deploy --replicas=2
deployment.apps/simple-deploy scaled
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
simple-deploy-7cf9979c88-qhmbm   1/1     Running   0          2m
simple-deploy-7cf9979c88-qx5fp   1/1     Running   0          4s
simple-pod                       1/1     Running   0          55s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\services\headless.yml
service/simple-headless created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get services
NAME              TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
kubernetes        ClusterIP   10.96.0.1    <none>        443/TCP    23h
simple-headless   ClusterIP   None         <none>        9898/TCP   10s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po -o wide
NAME                             READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
simple-deploy-7cf9979c88-qhmbm   1/1     Running   0          2m42s   172.17.0.4   minikube   <none>           <none>
simple-deploy-7cf9979c88-qx5fp   1/1     Running   0          46s     172.17.0.6   minikube   <none>           <none>
simple-pod                       1/1     Running   0          97s     172.17.0.5   minikube   <none>           <none>
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it simple-pod -- nslookup simple-headless
nslookup: can't resolve '(null)': Name does not resolve

Name:      simple-headless
Address 1: 172.17.0.4 172-17-0-4.simple-headless.default.svc.cluster.local
Address 2: 172.17.0.6 172-17-0-6.simple-headless.default.svc.cluster.local
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete svc simple-headless
service "simple-headless" deleted
```

## Create a service of type ClusterIP

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\6-services\clusterip.yml
service/simple-cluster-ip created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get svc
NAME                TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)    AGE
kubernetes          ClusterIP   10.96.0.1     <none>        443/TCP    23h
simple-cluster-ip   ClusterIP   10.98.48.83   <none>        9898/TCP   8s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it simple-pod -- nslookup simple-cluster-ip
nslookup: can't resolve '(null)': Name does not resolve

Name:      simple-cluster-ip
Address 1: 10.98.48.83 simple-cluster-ip.default.svc.cluster.local
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete svc simple-cluster-ip
service "simple-cluster-ip" deleted
```
