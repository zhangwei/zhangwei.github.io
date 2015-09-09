title: docker-ports
date: 2015-09-09 18:25:01
tags:
 - docker
 - network
---

### 1. Dockerfile EXPOSE

The EXPOSE instructions informs Docker that the container will listen on the specified network ports at runtime. Docker uses this information to interconnect containers using links and to determine which ports to expose to the host when using the -P flag.

Note: EXPOSE doesn’t define which ports can be exposed to the host or make ports accessible from the host by default. To expose ports to the host, at runtime, use the -p flag or the -P flag

### 2. docker run / create -P, --publish-all=true|false

When that container was created, the -P flag was used to automatically map any network port inside it to a random high port within an ephemeral port range on your Docker host. Next, when docker ps was run, you saw that port 5000 in the container was bound to port 49155 on the host.

```
$ docker run -d -P training/webapp python app.py
$ docker ps nostalgic_morse
CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse
```

### 3. docker run / create -p, --publish=[]

You also saw how you can bind a container’s ports to a specific port using the -p flag. Here port 80 of the host is mapped to port 5000 of the container:

```
$ docker run -d -p 80:5000 training/webapp python app.py
```