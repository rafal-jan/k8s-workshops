# Deployments

## Creating a deployment

```console
~/k8s-workshops $ kubectl apply -f 4-deployments/deploy.yml
deployment.apps/simple-deploy created
```

```console
~/k8s-workshops $ kubectl get deploy,rs,pod
NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/simple-deploy   1/1     1            1           8s

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/simple-deploy-7cf9979c88   1         1         1       8s

NAME                                 READY   STATUS    RESTARTS   AGE
pod/simple-deploy-7cf9979c88-sbbfp   1/1     Running   0          8s
```

## Desired state management

```console
~/k8s-workshops $ kubectl delete pod simple-deploy-7cf9979c88-sbbfp
pod "simple-deploy-7cf9979c88-sbbfp" deleted
```

```console
~/k8s-workshops $ kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
simple-deploy-7cf9979c88-p6hvt   1/1     Running   0          17s
```

## Scaling a deployment

```console
~/k8s-workshops $ kubectl scale deploy simple-deploy --replicas=2
deployment.apps/simple-deploy scaled
```

```console
~/k8s-workshops $ kubectl get deploy,rs,pod
NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/simple-deploy   2/2     2            2           118s

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/simple-deploy-7cf9979c88   2         2         2       118s

NAME                                 READY   STATUS    RESTARTS   AGE
pod/simple-deploy-7cf9979c88-ftftw   1/1     Running   0          13s
pod/simple-deploy-7cf9979c88-p6hvt   1/1     Running   0          87s

```

## Rolling back a deployment

```console
~/k8s-workshops $ kubectl rollout history deploy simple-deploy
deployment.apps/simple-deploy
REVISION  CHANGE-CAUSE
1         <none>
```

```console
~/k8s-workshops $ kubectl apply -f 4-deployments/deploy-v2.yml
deployment.apps/simple-deploy configured
```

```console
~/k8s-workshops $ kubectl get deploy,rs,po
NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/simple-deploy   2/2     1            2           3m3s

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/simple-deploy-7cf9979c88   2         2         2       3m3s
replicaset.apps/simple-deploy-d789858b     1         1         0       36s

NAME                                 READY   STATUS         RESTARTS   AGE
pod/simple-deploy-7cf9979c88-ftftw   1/1     Running        0          78s
pod/simple-deploy-7cf9979c88-p6hvt   1/1     Running        0          2m32s
pod/simple-deploy-d789858b-k8wjv     0/1     ErrImagePull   0          36s
```

```console
~/k8s-workshops $ kubectl rollout history deploy simple-deploy
deployment.apps/simple-deploy
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

```console
~/k8s-workshops $ kubectl rollout undo deploy simple-deploy --to-revision=1
deployment.apps/simple-deploy rolled back
```

```console
~/k8s-workshops $ kubectl get deploy,rs,po
NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/simple-deploy   2/2     2            2           3m58s

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/simple-deploy-7cf9979c88   2         2         2       3m58s
replicaset.apps/simple-deploy-d789858b     0         0         0       91s

NAME                                 READY   STATUS    RESTARTS   AGE
pod/simple-deploy-7cf9979c88-ftftw   1/1     Running   0          2m13s
pod/simple-deploy-7cf9979c88-p6hvt   1/1     Running   0          3m27s
```

```console
~/k8s-workshops $ kubectl rollout history deploy simple-deploy
deployment.apps/simple-deploy
REVISION  CHANGE-CAUSE
2         <none>
3         <none>
```

## Deleting a deployment

```console
~/k8s-workshops $ kubectl delete deploy simple-deploy
deployment.apps "simple-deploy" deleted
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS        RESTARTS   AGE
simple-deploy-7cf9979c88-7cxfb   1/1     Terminating   0          15m
simple-deploy-7cf9979c88-mjmcm   1/1     Terminating   0          16m
```

```console
~/k8s-workshops $ kubectl get deploy,rs,po
No resources found in default namespace.
```
