# Exercise

Ok, now you're going to use existing docker images to build your own Kubernetes cluster.  
Few things that may come in handy:  

1. Add these parameters to your `minikube start` command for more reliable cluster: `--memory='4000mb' --cpus=4` 
2. Avoid using `Pod` as your kind of Kubernetes resource. We're looking for something easier to manage.
3. Every module needs an additional resource. Something to balance the load and provide easy way to access one of the pods using the DNS.
4. If you're having troubles resist your urge to decompile provided jars in docker images. Just ask for some help! :)

### Final expected result
You go to your browser and open `http://<minikube_ip>:30000`. There is a frontend which was provided by us. You login using `astronaut/moon` credentials, click on `START` and new rocket appears on the right!

## Mongo Database

### Essential values
Docker image: `mongo`  
Port: `27017`

That's actually all you need to provide Mongo database in your cluster.  
It would be nice if you wouldn't lose your data after restarting Mongo pod though...

### Verification
`kubectl exec -it <mongo_pod_name> -- mongo --eval "db.adminCommand('ping')"`
```
MongoDB shell version v4.2.1
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("12b54bfc-6ffe-43dd-aab6-bfb412c80617") }
MongoDB server version: 4.2.1
{ "ok" : 1 }
```

## Backend (Spring Boot)

### Essential values
Docker image: `k8sworkshopharman/backend-service:latest`  
Port: `8080`

### Environment variables
1. `SPRING_DATA_MONGODB_HOST`
2. `SPRING_DATA_MONGODB_PORT`

### Verification
`kubectl exec -it <backend_pod_name> -- wget -qO- localhost:8080/actuator/health`
```
{"status":"UP"}
```

`kubectl exec -it <backend_pod_name> -- wget -qO- localhost:8080/actuator/help`
```
{"spring.data.mongodb.host":<CONFIDENTIAL>,"spring.data.mongodb.port":<CONFIDENTIAL>}
```

## Frontend (Spring Boot)

### Essential values
Docker image: `k8sworkshopharman/frontend-service:latest`  
Port: `8888`

### Environment variables
1. `WORKSHOP_BACKEND_HOST`
2. `WORKSHOP_BACKEND_PORT`
3. `WORKSHOP_USERNAME`
4. `WORKSHOP_PASWORD`

### Verification
`kubectl exec -it <frontend_pod_name> -- wget -qO- localhost:8888/actuator/health`
```
{"status":"UP"}
```

`kubectl exec -it <frontend_pod_name> -- wget -qO- localhost:8888/actuator/help`
```
{"backend":{"workshop.backend.host":<CONFIDENTIAL>,"workshop.backend.port":<CONFIDENTIAL>},"workshop.username":<CONFIDENTIAL>,"workshop.password":<CONFIDENTIAL>}
```

## Additional effort
Everything above is sufficient to finish the task.  
But if it was too easy or you just want to hang around some more here are some additional tasks:

### Scale up your resources
Your backend should have 3 replicas.  
After you do it, go to frontend and start sending many, many rocket into space. The hostnames should differ.

### Readiness and Liveness probes
Configure them for your Kubernetes resources

#### Mongo
```
exec:
  command:
    - mongo
    - --eval
    - "db.adminCommand('ping')"
```

#### Backend
```
httpGet:
  path: /actuator/health
  port: 8080
```

#### Frontend
```
httpGet:
  path: /actuator/health
  port: 8888
```

### Requests and Limits
The backend and frontend seems too hungry for resources.  
We believe `384Mi` should be enough for each pod.  
This might help:
```
name: JAVA_OPTS
value: "-Xmx256m -Xms64m -Xss256k"
```