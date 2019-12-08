# Volumes

## Config map volumes

```console
~/k8s-workshops $ kubectl apply -f 7-volumes/configmap-volume.yml
pod/configmap-volume created
```

```console
~/k8s-workshops $ kubectl get pods
NAME               READY   STATUS    RESTARTS   AGE
configmap-volume   1/1     Running   0          6s
```

```console
~/k8s-workshops $ kubectl exec -it configmap-volume -- curl localhost:9898/configs
{
  "input.json": "{\n    \"key-from-file-1\": \"value-from-file-1\",\n    \"key-from-file-2\": \"value-from-file-2\"\n}"
}
```

```console
~/k8s-workshops $ kubectl exec -it configmap-volume -- cat /config/input.json
{
    "key-from-file-1": "value-from-file-1",
    "key-from-file-2": "value-from-file-2"
}
```

```console
~/k8s-workshops $ kubectl delete pod configmap-volume
pod "configmap-volume" deleted
```

## Host path volumes

```console
~/k8s-workshops $ kubectl apply -f 7-volumes/emptydir-volume.yml
deployment.apps/volume created
```

```console
~/k8s-workshops $ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
volume-848977d55b-5rlzm   1/1     Running   0          37s
```

```console
~/k8s-workshops $ kubectl exec -it volume-848977d55b-5rlzm -- curl -X POST -d "test" localhost:9898/store
{
  "hash": "a94a8fe5ccb19ba61c4c0873d391e987982fbbd3"
}
```

```console
~/k8s-workshops $ kubectl exec -it volume-848977d55b-5rlzm -- curl localhost:9898/store/a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
test
```

```console
~/k8s-workshops $ kubectl delete pod volume-848977d55b-5rlzm
pod "volume-848977d55b-5rlzm" deleted
```

```console
~/k8s-workshops $ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
volume-848977d55b-rkf46   1/1     Running   0          12s
```

```console
~/k8s-workshops $ kubectl exec -it volume-848977d55b-rkf46 -- curl localhost:9898/store/a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
{
  "code": 500,
  "message": "reading file failed"
}
```

```console
~/k8s-workshops $ minikube ssh
                         _             _
            _         _ ( )           ( )
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __  
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ mkdir persistent-volume
$ chmod 777 persistent-volume/
$ exit
logout
```

```console
~/k8s-workshops $ kubectl apply -f 7-volumes/hostpath-volume.yml
deployment.apps/volume configured
```

```console
~/k8s-workshops $ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
volume-667657496d-qnmlf   1/1     Running   0          11s
```

```console
~/k8s-workshops $ kubectl exec -it volume-667657496d-qnmlf -- curl -X POST -d "test" localhost:9898/store
{
  "hash": "a94a8fe5ccb19ba61c4c0873d391e987982fbbd3"
}
```

```console
~/k8s-workshops $ kubectl exec -it volume-667657496d-qnmlf -- curl localhost:9898/store/a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
test
```

```console
~/k8s-workshops $ kubectl delete pod volume-667657496d-qnmlf
pod "volume-667657496d-qnmlf" deleted
```

```console
~/k8s-workshops $ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
volume-667657496d-6z5gl   1/1     Running   0          17s
```

```console
~/k8s-workshops $ kubectl exec -it volume-667657496d-6z5gl -- curl localhost:9898/store/a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
test
```

```console
~/k8s-workshops $ minikube ssh -- ls -l /home/docker/persistent-volume
total 4
-rw-r--r-- 1 100 65533 4 Dec  8 19:30 a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
```
