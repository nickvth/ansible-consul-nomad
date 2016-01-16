# Vagrant instructions to run stack local with virtualbox

***Requirements***

- Ansible 1.9.2 >
- VirtualBox 4.3.30 >
- Vagrant 1.7.4 >

***Versions***

- Consul 0.6.0
- Nomad 0.2.3    Not available yet
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

Run vm's with virtualbox provider. After starting the vm's ansible will provision the nodes.

```
vagrant up 
```

***Services***

Consul UI

* http://192.168.33.10:8500

Nomad API "NOT WORKING YET"

* http://192.168.33.10:4646/

Docs
* https://www.nomadproject.io/docs/http/index.html

