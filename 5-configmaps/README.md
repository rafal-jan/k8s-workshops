# Config maps

## Set environment variable on a deployment

```console
~/k8s-workshops $ kubectl apply -f 5-configmaps/env-deploy.yml
deployment.apps/env-deploy created
```

```console
~/k8s-workshops $ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
env-deploy-7d6b7f55c5-qxhnx   1/1     Running   0          51s
```

```console
~/k8s-workshops $ kubectl exec -it env-deploy-7d6b7f55c5-qxhnx -- curl localhost:9898/env
[
  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
  "HOSTNAME=env-deploy-7d6b7f55c5-qxhnx",
  "TEST=success",
  "KUBERNETES_SERVICE_HOST=10.96.0.1",
  "KUBERNETES_SERVICE_PORT=443",
  "KUBERNETES_SERVICE_PORT_HTTPS=443",
  "KUBERNETES_PORT=tcp://10.96.0.1:443",
  "KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443",
  "KUBERNETES_PORT_443_TCP_PROTO=tcp",
  "KUBERNETES_PORT_443_TCP_PORT=443",
  "KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1",
  "HOME=/home/app"
]
```

## Create a config map

```console
~/k8s-workshops $ kubectl apply -f 5-configmaps/cm.yml
configmap/simple-cm created
```

```console
~/k8s-workshops $ kubectl get cm
NAME        DATA   AGE
simple-cm   2      4s
```

```console
~/k8s-workshops $ kubectl get cm simple-cm -o yaml
apiVersion: v1
data:
  key1: value1
  key2: value2
kind: ConfigMap
...
```

## Create a config map using file as input

```console
~/k8s-workshops $ kubectl create cm file-cm --from-file 5-configmaps/input.json
configmap/file-cm created
```

```console
~/k8s-workshops $ kubectl get cm
NAME        DATA   AGE
file-cm     1      12s
simple-cm   2      82s
```

```console
~/k8s-workshops $ kubectl get cm file-cm -o yaml
apiVersion: v1
data:
  input.json: |-
    {
        "key-from-file-1": "value-from-file-1",
        "key-from-file-2": "value-from-file-2"
    }
kind: ConfigMap
...
```

## Set environment variable on a deployment using value from config map

```console
~/k8s-workshops $ kubectl apply -f 5-configmaps/env-deploy-v2.yml
deployment.apps/env-deploy configured
```

```console
~/k8s-workshops $ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
env-deploy-7d796f4cdd-pg5d9   1/1     Running   0          18s
```

```console
~/k8s-workshops $ kubectl exec -it env-deploy-7d796f4cdd-pg5d9 -- curl localhost:9898/env
[
  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
  "HOSTNAME=env-deploy-7d796f4cdd-pg5d9",
  "TEST=success",
  "KEY1=value1",
  "KUBERNETES_SERVICE_PORT_HTTPS=443",
  "KUBERNETES_PORT=tcp://10.96.0.1:443",
  "KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443",
  "KUBERNETES_PORT_443_TCP_PROTO=tcp",
  "KUBERNETES_PORT_443_TCP_PORT=443",
  "KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1",
  "KUBERNETES_SERVICE_HOST=10.96.0.1",
  "KUBERNETES_SERVICE_PORT=443",
  "HOME=/home/app"
]
```

```console
~/k8s-workshops $ kubectl delete deploy env-deploy 
deployment.apps "env-deploy" deleted
```
