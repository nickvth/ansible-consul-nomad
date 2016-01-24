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

<b>Consul and DnsMasq</b>

| Node | Service | Port(s) |
|------|---------|------|
|control-node|consul-master|8500,8600 (DNS)|
|control-node|dnsmasq|53|
|resource-node|consul-agent|8500,8600 (DNS)|
|resource-node|dnsmasq|53|

<b>Nomad</b>

- http: The port used to run the HTTP server. Applies to both client and server nodes. Defaults to 4646.
- rpc: The port used for internal RPC communication between agents and servers, and for inter-server traffic for the consensus algorithm (raft). Defaults to 4647. Only used on server nodes.
- serf: The port used for the gossip protocol for cluster membership. Both TCP and UDP should be routable between the server nodes on this port. Defaults to 4648. Only used on server nodes.