> mvn -f myhapp/pom.xml clean package

> docker-compose -f docker-compose.yml \
                 -f docker-compose.db.yml \
                 up -d

> docker-compose -f docker-compose.yml \
                 -f docker-compose.db.yml \
                 ps

> docker-compose -f docker-compose.yml \
                 -f docker-compose.db.yml \
                 down

> docker-compose -verbose up -d

> extends:
    file: configuration.yml
    service: config



docker service scale web=5



> docker service ps -f "desired-state=running" web

rolling update


> docker service update web --image wildfly:2 --update-parallelism 2 --update-delay 10s


Swarm Node: Drain Node

> docker node update --availability drain <nodename>
> docker node update --availability pause <nodename>
> docker node update --availability active <nodename>

> DOCKER_OPTS="--label=wildfly.storage=ssd"

> docker service create --replicas=3 --name=web --constraint engine.labels.wildfly.storage=ssd jbos/wildfly


Swarm Mode: Global service


> docker service create --mode=global --name=prom prom/prometheus


Persistent Containers

xxx | Implicit Per-Container | Explicit Per-Container | Per-Host | MultiHost
--- | ---------------------- | ---------------------- | -------  | ----------
What | Default sandbox       | Explicit volume        | Mounted withing the container | Storage on distributed file system
Location | /var/lib/docker/volumes on the host | /var/lib/docker/volumes on the host | Mounted within the container | Ceph, GlusterFS, and NFS ...
Container Crash | Directory unavailable | Directory unavailable | Yes | Yes
Host Crash | Directory unavailable | Directory unavailable | No | Yes
Shared  | No | Yes | Yes(host only) | Yes(cluster wide)



> docker inspect --format "{{json .Mounts}}" db | jq '.'

# enter the docker vm 
> docker container run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
> ls /var/lib/docker/volumes/


# Docker Volume plugin

+ Portworx

Container granular volumes
Cross-availability zone HA
Support for enterprise data operations
Ease of deployment and provisioning

> PX-Dev



# monitor


docker stats 
docker stats  db
docker stats  --format "{{.Container}}: {{.CPUPerc}}" db
docker stats  --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}" db
docker stats   db --no-stream

curl --unix-socket /var/run/docker.sock http://localhost/containers/db/stats
curl --unix-socket /var/run/docker.sock http://localhost/containers/redis/stats | jq

> docker system events


"metrics-addr":"0.0.0.0:1337",
"experimental": true


> curl http:/localhost:1337/metrics
> curl http:/localhost:1337/metrics | grep engine


cadvisor


DCOS
> https://dcos.io/

ECS

openshift

