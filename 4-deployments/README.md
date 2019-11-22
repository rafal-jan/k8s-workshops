# Deployments

## Creating a deployment using imperative API

```console
PS C:\dev\projects\kubernetes-workshops> kubectl create deployment first-deploy --image stefanprodan/podinfo --dry-run -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: first-deploy
  name: first-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: first-deploy
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: first-deploy
    spec:
      containers:
      - image: stefanprodan/podinfo
        name: podinfo
        resources: {}
status: {}
```

## Creating a deployment using declarative API

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\4-deployments\deploy.yml
deployment.apps/simple-deploy created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get deploy
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
simple-deploy   1/1     1            1           51s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get rs
NAME                       DESIRED   CURRENT   READY   AGE
simple-deploy-7cf9979c88   1         1         1       55s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get po
NAME                             READY   STATUS    RESTARTS   AGE
simple-deploy-7cf9979c88-2vght   1/1     Running   0          57s
```

## Desired state management

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod simple-deploy-7cf9979c88-2vght
pod "simple-deploy-7cf9979c88-2vght" deleted
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
simple-deploy-7cf9979c88-bptmr   1/1     Running   0          11s
```

## Scaling a deployment

```console
PS C:\dev\projects\kubernetes-workshops> kubectl scale deploy simple-deploy --replicas=2
deployment.apps/simple-deploy scaled
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get deploy
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
simple-deploy   2/2     2            2           2m49s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get rs
NAME                       DESIRED   CURRENT   READY   AGE
simple-deploy-7cf9979c88   2         2         2       3m
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
simple-deploy-7cf9979c88-42dj5   1/1     Running   0          39s
simple-deploy-7cf9979c88-bptmr   1/1     Running   0          91s
```

## Rolling back a deployment

```console
PS C:\dev\projects\kubernetes-workshops> kubectl rollout history deploy simple-deploy
deployment.apps/simple-deploy
REVISION  CHANGE-CAUSE
1         <none>
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl set image deploy simple-deploy simple-pod=k8s.gcr.io/liveness
deployment.apps/simple-deploy image updated
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS                 RESTARTS   AGE
simple-deploy-7cf9979c88-7cxfb   1/1     Running                0          12m
simple-deploy-7cf9979c88-mjmcm   1/1     Running                0          13m
simple-deploy-d848b455-8w24p     0/1     CreateContainerError   0          30s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl rollout history deploy simple-deploy
deployment.apps/simple-deploy
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl rollout undo deploy simple-deploy --to-revision=1
deployment.apps/simple-deploy rolled back
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
simple-deploy-7cf9979c88-7cxfb   1/1     Running   0          13m
simple-deploy-7cf9979c88-mjmcm   1/1     Running   0          14m
```

## Deleting a deployment

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete deploy simple-deploy
deployment.apps "simple-deploy" deleted
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS        RESTARTS   AGE
simple-deploy-7cf9979c88-7cxfb   1/1     Terminating   0          15m
simple-deploy-7cf9979c88-mjmcm   1/1     Terminating   0          16m
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
No resources found in default namespace.
```
