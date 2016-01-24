# Vagrant instructions to run stack local with virtualbox

***Requirements***

- Ansible 1.9.x
- VirtualBox 4.3.30 >
- Vagrant 1.7.4 >

***Versions***

- Consul 0.6.0
- Nomad 0.2.3   
- Docker 1.8.2

***Instructions***

Installation time is betweeen 15 min and 30 min.

If you want more memory for each vm, then edit Vagrantfile

```
vi Vagrantfile
      v.customize ["modifyvm", :id, "--memory", 1536]
```

Set correct ansible vars

```
cp group_vars/all customizations
vi customizations
```

Run vm's with virtualbox provider. After starting the vm's, ansible will provision the nodes.

```
vagrant up 
```

***Services***

Consul UI

* http://192.168.33.10:8500

Nomad API 

* http://192.168.33.10:4646


```
$ vagrant ssh control1
$ sudo su - nomad
$ nomad init
$ vi example.nomad  ( change datacenters to datacenters = ["hashicorpdc1"] )
$ export NOMAD_ADDR=http://nomad-server.service.consul:4646
$ nomad run example.nomad
==> Monitoring evaluation "2d0c52d0-777c-2873-9b39-6af52ad9649a"
    Evaluation triggered by job "example"
    Allocation "cd6f4a8f-cb52-699b-1e7b-26d6bf814864" created: node "resource1", group "cache"
    Evaluation status changed: "pending" -> "complete"
==> Evaluation "2d0c52d0-777c-2873-9b39-6af52ad9649a" finished with status "complete" 
$ nomad status example
nomad status example
ID          = example
Name        = example
Type        = service
Priority    = 50
Datacenters = hashicorpdc1
Status      = <none>

==> Evaluations
ID                                    Priority  TriggeredBy     Status
2d0c52d0-777c-2873-9b39-6af52ad9649a  50        job-register    complete
36369ee6-984a-8760-4398-2f2c537e0a02  50        job-deregister  complete
9deb068c-9483-44f0-a2da-96d0ddfd1458  50        node-update     complete
0b83a331-030a-cfff-9c86-6d7e672ef96b  50        node-update     complete
28f1dd8f-c4ea-430b-f70b-80ccc3f2c54c  50        node-update     complete
c5fba8a2-f06f-24ff-466a-986470777e7a  50        node-update     complete
5606fc3f-ea1e-9c31-b23a-3b7f88ff22d8  50        job-register    complete
0daa5cd4-3fe4-6355-1ae9-cb338476a035  50        job-register    complete
73d83e00-ce76-1238-bbbe-e7cdf7e12e44  50        node-update     complete
6aaa07e4-d4b6-4d8f-fd2b-40fa10dfc3ad  50        job-deregister  complete
f86d35de-a630-7afd-ec77-2aa1f5d8c5de  50        job-register    complete
cd6c1df4-b2e8-3d7c-2367-3bb5ca62de8b  50        job-deregister  complete
b619c3cb-717a-64f0-ca3b-9459147b5334  50        job-register    complete
7ad6d508-0cb6-2fac-123f-2ed1fff3bf34  50        job-deregister  complete
c0d67710-db43-32ec-e097-fae5f2287523  50        job-register    complete

==> Allocations
ID                                    EvalID                                NodeID     TaskGroup  Desired  Status
cd6f4a8f-cb52-699b-1e7b-26d6bf814864  2d0c52d0-777c-2873-9b39-6af52ad9649a  resource1  cache      run      running
a5b805b0-93a7-7b1d-e3ec-fe294a28544a  0b83a331-030a-cfff-9c86-6d7e672ef96b  resource1  cache      stop     dead
54969e62-1506-6aaa-f29d-4660519d7b01  28f1dd8f-c4ea-430b-f70b-80ccc3f2c54c  <none>     cache      failed   failed
9559aa99-6014-d943-e25b-2d678bf4c35f  5606fc3f-ea1e-9c31-b23a-3b7f88ff22d8  resource1  cache      stop     dead
00408321-27f1-4e54-bd3a-20062c6224bc  f86d35de-a630-7afd-ec77-2aa1f5d8c5de  resource1  cache      stop     dead
2580bf24-486f-44d1-1fae-0e7564b45bec  b619c3cb-717a-64f0-ca3b-9459147b5334  resource1  cache      stop     dead
839b90aa-fbb8-11f8-3aa7-cd116447f2c9  c0d67710-db43-32ec-e097-fae5f2287523  <none>     cache      failed   failed

```

Check in consul http://192.168.33.10:8500 ( you will see redis-cache service )

```
$ vagrant ssh resource1
$ sudo su - 
$ docker ps
# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                                                          NAMES
39723f245968        redis:latest        "/entrypoint.sh redis"   About a minute ago   Up About a minute   192.168.33.11:52587->6379/tcp, 192.168.33.11:52587->6379/udp   redis-cd6f4a8f-cb52-699b-1e7b-26d6bf814864
```

Docs
* https://www.nomadproject.io/docs/http/index.html

