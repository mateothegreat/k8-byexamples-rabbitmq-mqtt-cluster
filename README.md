<!--
#                                 __                 __
#    __  ______  ____ ___  ____ _/ /____  ____  ____/ /
#   / / / / __ \/ __ `__ \/ __ `/ __/ _ \/ __ \/ __  /
#  / /_/ / /_/ / / / / / / /_/ / /_/  __/ /_/ / /_/ /
#  \__, /\____/_/ /_/ /_/\__,_/\__/\___/\____/\__,_/
# /____                     matthewdavis.io, holla!
#
#-->

[![Clickity click](https://img.shields.io/badge/k8s%20by%20example%20yo-limit%20time-ff69b4.svg?style=flat-square)](https://k8.matthewdavis.io)
[![Twitter Follow](https://img.shields.io/twitter/follow/yomateod.svg?label=Follow&style=flat-square)](https://twitter.com/yomateod) [![Skype Contact](https://img.shields.io/badge/skype%20id-appsoa-blue.svg?style=flat-square)](skype:appsoa?chat)

# Kubernetes @ RabbitMQ 3.7 Cluster using Peer Discovery

_can you hear me now?_
> k8 by example -- straight to the point, simple execution.

## Usage

```sh

Usage:

    make <target>
    
Targets:

    build                Build docker image
    push-gcloud          Push docker image using `gcloud`
    scale                Scale rabbitmq pods from the stateful set (make scale REPLICAS=3)
    manifests            Output manifests detected (used with make install, delete, get, describe, etc)
    install              Installs manifests to kubernetes using kubectl apply (make manifests to see what will be installed)
    delete               Deletes manifests to kubernetes using kubectl delete (make manifests to see what will be installed)
    get                  Retrieves manifests to kubernetes using kubectl get (make manifests to see what will be installed)
    describe             Describes manifests to kubernetes using kubectl describe (make manifests to see what will be installed)
    context              Globally set the current-context (default namespace)
    shell                Grab a shell in a running container
```

# Install

Make sure to run `git submodule update --init` first.

```sh
$ make install

[ INSTALLING MANIFESTS/DEPLOYMENT.YAML ]: deployment "rabbitmq" created
[ INSTALLING MANIFESTS/SERVICE.YAML ]: service "rabbitmq" created
[ INSTALLING MANIFESTS/CONFIGMAP.YAML ]: configmap "rabbitmq-config" created
[ INSTALLING MANIFESTS/STATEFULSET.YAML ]: statefulset "rabbitmq" created
[ INSTALLING MANIFESTS/RBAC-SERVICEACCOUNT.YAML ]: serviceaccount "rabbitmq" created
[ INSTALLING MANIFESTS/RBAC-ROLE.YAML ]: role "endpoint-reader" created
[ INSTALLING MANIFESTS/RBAC-ROLEBINDING.YAML ]: rolebinding "endpoint-reader" created
```

Now you can open your browser to http://rabbitmq:15672 (user/pass = guest).

## Adding a user

```sh
$ make adduser

kubectl exec rabbitmq-0 -it -- rabbitmqctl add_user admin P@55w0rd!!
Adding user "admin" ...
kubectl exec rabbitmq-0 -it -- rabbitmqctl set_user_tags admin administrator
Setting tags for user "admin" to [administrator] ...
kubectl exec rabbitmq-0 -it -- rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
Setting permissions for user "admin" in vhost "/" ...
```
## Scaling

```sh

[yomateo@instance-1 k8-byexamples-rabbitmq-cluster]$ make scale REPLICAS=3
kubectl scale statefulset/rabbitmq --namespace=default --replicas=3
statefulset "rabbitmq" scaled

[yomateo@instance-1 k8-byexamples-rabbitmq-cluster]$ make kube/logs

2018-02-17 11:36:57.086 [info] <0.373.0> rabbit on node 'rabbit@10.28.0.136' up
2018-02-17 11:37:13.371 [info] <0.373.0> node 'rabbit@10.28.0.137' up

2018-02-17 11:37:14.594 [info] <0.373.0> rabbit on node 'rabbit@10.28.0.137' up
2018-02-17 11:37:43.308 [info] <0.373.0> node 'rabbit@10.28.0.138' up

2018-02-17 11:37:45.329 [info] <0.373.0> rabbit on node 'rabbit@10.28.0.138' up
2018-02-17 11:40:00.459 [info] <0.373.0> node 'rabbit@10.28.0.139' up

```

## Cleanup

```sh
$ make delete

[ DELETING MANIFESTS/DEPLOYMENT.YAML ]: deployment "rabbitmq" deleted
[ DELETING MANIFESTS/SERVICE.YAML ]: service "rabbitmq" deleted
[ DELETING MANIFESTS/CONFIGMAP.YAML ]: configmap "rabbitmq-config" deleted
[ DELETING MANIFESTS/STATEFULSET.YAML ]: statefulset "rabbitmq" deleted
[ DELETING MANIFESTS/RBAC-SERVICEACCOUNT.YAML ]: serviceaccount "rabbitmq" deleted
[ DELETING MANIFESTS/RBAC-ROLE.YAML ]: role "endpoint-reader" deleted
[ DELETING MANIFESTS/RBAC-ROLEBINDING.YAML ]: rolebinding "endpoint-reader" deleted
```

## See also

* http://www.rabbitmq.com/blog/2018/02/12/peer-discovery-subsystem-in-rabbitmq-3-7/
* http://www.rabbitmq.com/cluster-formation.html
* https://rabbitmq.com/plugins.html
* https://rabbitmq.com/community-plugins.html
