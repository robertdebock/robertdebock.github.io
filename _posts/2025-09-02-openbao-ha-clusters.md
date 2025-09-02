---
title: Openbao highly available (HA) clusters
---

# Openbao highly available (HA) clusters

This article focuses around the design of an Openbao (or Vault) Raft cluster. There is always some doubt when setting up an Openbao cluster using Raft. These are some designs, with a focus to the amount of datacenters that are available.

> Note: Openboa is under development, these suggestions are relevant in September 2025. Features for replication are changing rapidly however.

## On prem

On prem, maybe using VMWare or so, allows you to run Openbao on your own infrastructure.

### Single datacenter

In case a single datacenter is available, your options are limited. This is the design I would recommend.

```text
+--------------------- DC 1 ----------------------+
|            +--- load balancer ---+              |
|            |                     |              |
|            +---------------------+              |
|                                                 |
| +--- bao-1 ---+ +--- bao-2 ---+ +--- bao-3 ---+ |
| |             | |             | |             | |
| +-------------+ +-------------+ +-------------+ |
+-------------------------------------------------+
```

In the ASCII-ART diagram above, you'll find:

- A load balancer, this can be an F5 LTM, HAProxy, whatever. The load balancer is configured with a health-check to determine what node is the leader.
- 3 Openbao nodes, boa-[1-3]. Any (single) one of those nodes will be the leader, the rest follwers. Only leaders can serve traffic as of now.

> Note: When possible, place the 3 bao nodes in different "availability zones", where network, cooling and power are unrelated to another zone.

### Double datacenter

There are some more options here, but still quite some limitations. This is what I would recommend:

```text
                                              +--- DNS ---+
                         +--------------------|           |
                         |                    +-----------+
                         V
+--------------------- DC 1 ----------------------+   +--------------------- DC 2 ----------------------+
|            +--- load balancer ---+              |   |            +--- load balancer ---+              |
|            |                     |              |   |            |                     |              |
|            +---------------------+              |   |            +---------------------+              |
|                                                 |   |                                                 |
| +--- bao-1 ---+ +--- bao-2 ---+ +--- bao-3 ---+ |   | +--- bao-1 ---+ +--- bao-2 ---+ +--- bao-3 ---+ |
| |             | |             | |             | |   | |    OFF      | |    OFF      | |    OFF      | |
| +-------------+ +-------------+ +-------------+ |   | +-------------+ +-------------+ +-------------+ |
+-------------------------------------------------+   +-------------------------------------------------+
```

Here you see two datacenters, each with a unique Openboa cluster. They don't synchronize data, as this feature is not available at this moment.
The intent is to let some DNS routing mechanism (F5 GTM or AWS Route 53 with health-checks for example) determine what cluster is available. The cluster on DC 1 is up and running, the cluster on DC 2 is installed, but not initialized. So the health check for the DC 2 cluster will not return an HTTP status of 200.
In case of an disaster, a stored snapshot should be restored to the DC 2 cluster. This is manual labour however.

### Triple (or more) datacenter

In case you have 3 datacenters, you can create a cluster as following. Keep in mind the latency between the datacenters needs to be less than 8 ms.

```text
                    +--- load balancer ---+
                    |                     |
                    +---------------------+

+----- DC 1 ------+   +----- DC 2 ------+   +----- DC 3 ------+
| +--- bao-1 ---+ |   | +--- bao-2 ---+ |   | +--- bao-3 ---+ |
| |             | |   | |             | |   | |             | |
| +-------------+ |   | +-------------+ |   | +-------------+ |
+-----------------+   +-----------------+   +-----------------+
```

> Note: The load balancer is not placed in a DC, it should not be impacted by a disaster. This can be a DNS record as well, or a VIP.

> Note: DNS based routing techniques can be slower to populate, as DNS records have an "time to live". The time to live can be short however.

## Alternative

You can move the storage backend to (for example) PostgreSQL. This means quorum is no longer important for Openbao. (PostgreSQL is responsible for getting the data from on data center to the other.

## Future

I'm sure Openbao will introduce new features to allow for replication and multi datacenter setups, which would dramatically change the above advices.

## FAQ

### How many nodes should be in a HA Openbao cluster?

| amount | effect                                                                 |
|--------|------------------------------------------------------------------------|
| 1      | No redundancy, loss of a node is loss of data and service.             |
| 2      | No quorum when a node is lost. This setup is more fragile than 1 node. |
| 3      | Yes, quorum, finally. You can loose 1 node.                            |
| 4      | You can loose 1 node, just as the 3-node setup, no benefit.            |
| 5      | Nice, you have a majority and can loose 2 nodes. Better than 3 nodes.  |
| more   | When it's uneven: fine.                                                |

> Note: Nodes should be spread over availability zones. A hyper scaler typically has 3 availability zones.

A typical setup of Openbao has 1 or 3 nodes. I would recommend 3. Sometimes I see a 5 node cluster where there are 3 availability zones, which makes no sense; losing an availability zone has the same effect as a 3 node cluster.
