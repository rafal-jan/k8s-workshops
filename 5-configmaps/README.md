# Config maps

## Set environment variable on a deployment

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\4-deployments\deploy.yml
deployment.apps/simple-deploy created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl set env deploy simple-deploy TEST=success
deployment.apps/simple-deploy env updated
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS        RESTARTS   AGE
simple-deploy-7cf9979c88-h2jm7   1/1     Terminating   0          47s
simple-deploy-7d6b7f55c5-d5tws   1/1     Running       0          4s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get deploy simple-deploy -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "2"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"simple-deploy","namespace":"default"},"spec":{"replicas":1,"selector":{"matchLabels":{"app":"simple"}},"template":{"metadata":{"labels":{"app":"simple"}},"spec":{"containers":[{"image":"stefanprodan/podinfo","name":"simple-pod"}]}}}}
  creationTimestamp: "2019-11-13T21:51:11Z"
  generation: 2
  name: simple-deploy
  namespace: default
  resourceVersion: "46040"
  selfLink: /apis/apps/v1/namespaces/default/deployments/simple-deploy
  uid: 4170e0c5-0471-4e9d-806a-8d72332de6c1
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: simple
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: simple
    spec:
      containers:
      - env:
        - name: TEST
          value: success
        image: stefanprodan/podinfo
        imagePullPolicy: Always
        name: simple-pod
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2019-11-13T21:51:14Z"
    lastUpdateTime: "2019-11-13T21:51:14Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2019-11-13T21:51:11Z"
    lastUpdateTime: "2019-11-13T21:51:58Z"
    message: ReplicaSet "simple-deploy-7d6b7f55c5" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 2
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it simple-deploy-7d6b7f55c5-d5tws -- curl localhost:9898/env
[
  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
  "HOSTNAME=simple-deploy-7d6b7f55c5-d5tws",
  "TEST=success",
  "KUBERNETES_PORT=tcp://10.96.0.1:443",
  "KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443",
  "KUBERNETES_PORT_443_TCP_PROTO=tcp",
  "KUBERNETES_PORT_443_TCP_PORT=443",
  "KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1",
  "KUBERNETES_SERVICE_HOST=10.96.0.1",
  "KUBERNETES_SERVICE_PORT=443",
  "KUBERNETES_SERVICE_PORT_HTTPS=443",
  "HOME=/home/app"
]
```

## Create a config map

```console
PS C:\dev\projects\kubernetes-workshops> kubectl apply -f .\5-configmaps\cm.yml
configmap/simple-cm created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get cm
NAME        DATA   AGE
simple-cm   2      4s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get cm simple-cm -o yaml
apiVersion: v1
data:
  key1: value1
  key2: value2
kind: ConfigMap
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","data":{"key1":"value1","key2":"value2"},"kind":"ConfigMap","metadata":{"annotations":{},"name":"simple-cm","namespace":"default"}}
  creationTimestamp: "2019-11-13T21:57:28Z"
  name: simple-cm
  namespace: default
  resourceVersion: "46447"
  selfLink: /api/v1/namespaces/default/configmaps/simple-cm
  uid: f6243291-73b1-4e52-bc55-8067c31f4ebf
```

## Create a config map using file as input

```console
PS C:\dev\projects\kubernetes-workshops> kubectl create cm file-cm --from-file .\5-configmaps\input.json
configmap/file-cm created
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get cm
NAME        DATA   AGE
file-cm     1      3s
simple-cm   2      58s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get cm file-cm -o yaml
apiVersion: v1
data:
  input.yml: "key-from-file-1: value-from-file-1\r\nkey-from-file-2: value-from-file-2"
kind: ConfigMap
metadata:
  creationTimestamp: "2019-11-13T21:58:23Z"
  name: file-cm
  namespace: default
  resourceVersion: "46514"
  selfLink: /api/v1/namespaces/default/configmaps/file-cm
  uid: ee7d7b05-7e92-4ef5-a50b-1b2528827c56
```

## Set environment variable on a deployment using value from config map

```console
PS C:\dev\projects\kubernetes-workshops> kubectl set env --keys key1 --from configmap/simple-cm deploy simple-deploy
deployment.apps/simple-deploy env updated
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get deploy simple-deploy -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "3"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"simple-deploy","namespace":"default"},"spec":{"replicas":1,"selector":{"matchLabels":{"app":"simple"}},"template":{"metadata":{"labels":{"app":"simple"}},"spec":{"containers":[{"image":"stefanprodan/podinfo","name":"simple-pod"}]}}}}
  creationTimestamp: "2019-11-13T21:51:11Z"
  generation: 3
  name: simple-deploy
  namespace: default
  resourceVersion: "46951"
  selfLink: /apis/apps/v1/namespaces/default/deployments/simple-deploy
  uid: 4170e0c5-0471-4e9d-806a-8d72332de6c1
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: simple
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: simple
    spec:
      containers:
      - env:
        - name: TEST
          value: success
        - name: KEY1
          valueFrom:
            configMapKeyRef:
              key: key1
              name: simple-cm
        image: stefanprodan/podinfo
        imagePullPolicy: Always
        name: simple-pod
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2019-11-13T21:51:14Z"
    lastUpdateTime: "2019-11-13T21:51:14Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2019-11-13T21:51:11Z"
    lastUpdateTime: "2019-11-13T22:03:59Z"
    message: ReplicaSet "simple-deploy-7d796f4cdd" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 3
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
simple-deploy-7d796f4cdd-vf5p5   1/1     Running   0          27s
```

```console
PS C:\dev\projects\kubernetes-workshops> kubectl exec -it simple-deploy-7d796f4cdd-vf5p5 --curl localhost:9898/env
[
  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
  "HOSTNAME=simple-deploy-7d796f4cdd-vf5p5",
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
PS C:\dev\projects\kubernetes-workshops> kubectl delete deploy simple-deploy
deployment.apps "simple-deploy" deleted
```
