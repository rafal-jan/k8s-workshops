# Volumes

## Config map volumes

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\7-volumes\cm-vol-pod.yml
pod/cm-vol-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
cm-vol-pod   1/1     Running   0          4s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it cm-vol-pod -- curl localhost:9898/configs
{
  "input.json": "{\r\n    \"key-from-file-1\": \"value-from-file-1\",\r\n    \"key-from-file-2\": \"value-from-file-2\"\r\n}"
}
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod cm-vol-pod
pod "cm-vol-pod" deleted
```

## Host path volumes

```console
PS C:\dev\projects\kubernetes-workshops> minikube ssh
                         _             _
            _         _ ( )           ( )
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ pwd
/home/docker
$ mkdir config
$ echo "key1: value1" > config/test.yml
$ exit
logout
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\7-volumes\hp-vol-pod.yml
pod/hp-vol-pod created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
hp-vol-pod   1/1     Running   0          5s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it hp-vol-pod -- curl localhost:9898/configs
{
  "test.yml": "key1: value1\n"
}
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl delete pod hp-vol-pod
pod "hp-vol-pod" deleted
```
