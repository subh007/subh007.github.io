---
layout: post
title: Deploying docker private repository
---

# Creating insecure docker repository

Sometimes, it requires to manage your own docker repository. It will help
developers to speedup the deployment process.

But setting up the docker repository is not easy as we need to setup the
certificates so to simplify the process the of setting up we can setup insecure
repository and configure the docker client to use it.

## Setting up your own Docker Repository [[1]](https://docs.docker.com/registry/deploying/)

Below command will start your registry on port 5000 :

```
$ docker run -d -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 -p 5000:5000 --restart=always --name registry registry:2
```



## Configuring the docker client to use insecure docker repository [[2]](https://docs.docker.com/registry/insecure/)
To use the insecure docker registry one need to configure the docker client. By
default you can use 127.0.0.0/8 as insecure registry.

To add our new insecure registry crate or modify file at `/etc/docker/daemon.json` :
```
{
  "insecure-registries" : ["<ip-insecure-registry>:5000"]
}
```

save it and reload the docker service :

```
$ sudo service docker restart
$ sudo docker info   /// to check the added insecure repository
```


## Test your repository

```
$ docker pull ubuntu:16.04
$ docker tag ubuntu:16.04 <ip-insecure-registry>:5000/my-ubuntu
$ docker push <ip-insecure-registry>:5000/my-ubuntu
$ docker image rm ubuntu:16.04
$ docker image rm <ip-insecure-registry>:5000/my-ubuntu
$ docker pull <ip-insecure-registry>:5000/my-ubuntu
```

It will confirm if you are able to pull image from remote repository.

Reference:
[1] https://docs.docker.com/registry/deploying/
[2] https://docs.docker.com/registry/insecure/
