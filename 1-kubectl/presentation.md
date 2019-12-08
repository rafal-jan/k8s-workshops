---
marp: true
theme: gaia
class: lead
auto-scaling: code
---

# **kubectl**

CLI to run commands against Kubernetes clusters

---

## kubectl autocompletion

for `bash` and `zsh`

```bash
source <(kubectl completion bash)
source <(kubectl completion zsh)
```

---

## API resources

(redacted)

```console
$ kubectl api-resources
NAME          SHORTNAMES   APIGROUP   NAMESPACED   KIND
configmaps    cm                      true         ConfigMap
endpoints     ep                      true         Endpoints
namespaces    ns                      false        Namespace
nodes         no                      false        Node
pods          po                      true         Pod
services      svc                     true         Service
deployments   deploy       apps       true         Deployment
replicasets   rs           apps       true         ReplicaSet
```
