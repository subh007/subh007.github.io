---
layout: post
title: Mocking Rest Interface
---

In this blog we will cover one of the important aspect to boost the developer performance by mocking the dependent rest interface for seamless development.

Let's say you are developing a utility which consumes a rest service provided by other developer. To avoid this any development blocker you can place a request response agreement and use this agreement in [`https://github.com/dreamhead/moco`](https://github.com/dreamhead/moco) to mock the APIs.

## Preparing API agreement

This agreement is provided as `moco.json` to moco. Here you will write your request and expected response details e.g. :

``` json
[
    {
  "request" :
    {
      "uri" : "/foo"
    },
  "response" :
    {
      "text" : "bar"
    }
}
]
```

Your request and expected response can be expressed with fine granularity. Here this [page](https://github.com/dreamhead/moco/blob/master/moco-doc/apis.md) you can follow for more details.

## Deployment of moco server

Now we are ready for the deployment :
1. Docker
2. Kubernetes

### Deployment in docker

Following command with deploy the moco server as docker and `moco.json` can be kept in volumen mount.
```
$ docker run -v /my/own/moco-directory:/var/moco -p 8000:8000 rezzza/docker-moco:latest
```

### Deployment using kubernetes

Following `.yaml` can be used to deploy the moco server in kubernetes environment. Here we pass the `moco.json` as file

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: moco
  labels:
    name: moco
    app: moco
spec:
  containers:
  - name: moco
    image: rezzza/docker-moco:latest
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
    ports:
      - containerPort: 8000
        protocol: TCP
    volumeMounts:
    - name: moco-volume
      mountPath: /var/moco
  volumes:
  - name: moco-volume
    configMap:
      name: moco-config
---
apiVersion: v1
kind: Service
metadata:
  name: moco-service
spec:
  selector:
    app: moco
  ports:
  - port: 8000
    targetPort: 8000
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: moco-config
data:
  moco.json: >
    [
      {
        "request" :
        {
        "uri" : "/foo"
        },
        "response" :
        {
          "text" : "bar"
        }
      } 
    ]
```

know you can use  this server to fire your REST request to avoid any dependency.