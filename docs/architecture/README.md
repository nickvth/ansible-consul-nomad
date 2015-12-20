# Architecture
The base platform contains control nodes that manage the cluster and any number of resource nodes. Containers automatically register themselves into Consul so that other services can locate them. The base platform should be cloud provider indenpendent (vcloud, amazone, openstack, digitalocean, etc.).

# Control Nodes
The control nodes manage a single datacenter. Each control node runs Consul server for service discovery, Nomad server for resource scheduling.

In general, it's best to provision 3 or 5 control nodes to achieve higher availability of services.

![Architecture](blaat.png)

# Resource Nodes
Resource nodes launch containers with Nomad client.

# Network

Below you can find a list of all services:

| Node | Service | Port(s) |
|------|---------|------|
|control-node|consul-master|8500,8600 (DNS)|
|control-node|dnsmasq|53|
|resource-node|consul-agent|8500,8600 (DNS)|
|resource-node|dnsmasq|53|

