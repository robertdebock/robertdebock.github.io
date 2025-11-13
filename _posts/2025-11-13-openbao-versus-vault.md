---
title: OpenBao versus Vault (Enterprise)
---

# OpenBao versus Vault (Enterprise)

[OpenBao](https://openbao.org/) (A fork of HashiCorp Vault before HashiCorp changed its license to be less-open-source) is developing quickly, so this article may be outdated in a few weeks. It's now November 2025, please keep that in mind.

## Feature comparison

There are differences between OpenBao and Vault (and Vault Enterprise). Let's compare. I'll explain details below this table.

| Feature | OpenBao | Vault | Vault Enterprise |
| --- | --- | --- | --- |
| Namespaces | ✅ | ❌ | ✅ |
| High Availability | ✅ | ✅ | ✅ |
| Auto unsealing | ✅ | ✅ | ✅ |
| Replication | ❌ | ❌ | ✅ |
| Performance standby | ❌ | ❌ | ✅ |
| Auto Snapshot | ❌ | ❌ | ✅ |
| Sentinel | ❌ | ❌ | ✅ |


> In general, you can clearly see that OpenBao is a fork of Vault (Community/Open Source) and lacks some "enterprise" features.

### Namespaces

[Namespaces](https://openbao.org/blog/namespaces-announcement/) can be used to offer teams their own piece of OpenBao or Vault. They are administratively separated.

> Note: Vault Enterprise offers namespaces, but offering these namespaces to teams introduces a risk for Vault Enterprise only: The "client count" can grow uncontrollably. Since OpenBao does not have the concept of "client-count", this issue is not applicable to OpenBao.

### High Availability

When using (the default and recommended) storage backend ["Raft"](https://thesecretlivesofdata.com/raft/), high availability is available on OpenBao, Vault, and Vault Enterprise. In general, Raft works really well; the only consideration to keep in mind is that the latency between nodes should be 8ms or less. That's typically easy to achieve in a single datacenter.

### Auto unsealing

OpenBao/Vault will start up in a sealed state. That means the backend where data is stored is encrypted and requires to be unsealed to make the data readable. By default, the "Shamir" method is used to unseal, which is a manual process. Especially in containers, this proves to be difficult to manage; if a pod spins up, it's sealed and requires manual intervention to be unsealed and available. For this reason, auto-unsealing can help. There are several methods:

| Method | OpenBao | Vault | Vault Enterprise |
| --- | --- | --- | --- |
| Shamir | ✅ | ✅ | ✅ |
| Cloud key | ✅ | ✅ | ✅ |
| HSM | ✅ | ❌ | ✅ |
| Transit | ✅ | ✅ | ✅ |
| Static | ✅ | ❌ | ❌ |

> Note: The ["Static"](https://openbao.org/docs/configuration/seal/static/) unseal method is nice for development purposes, but not for production use; the unseal key is stored on the node that runs OpenBao, which makes it quite insecure. Great for development though!

### Replication

Vault Enterprise supports "replication". There are two possibilities to use replication:

- Disaster Recovery replication (DR): One Vault Enterprise cluster synchronizes data to another (standby/dr) Vault Enterprise cluster. The other (standby/dr) Vault Enterprise cluster receives data and can be promoted to become a primary in case of a disaster. This promotion is a manual action.
- Performance Replication (PR): One Vault Enterprise cluster synchronizes data to another Vault Enterprise cluster. Both clusters are available for requests. This is typically used to lower the amount of time it takes to request a secret to Vault Enterprise. Typically, PR clusters are spread over the globe.

OpenBao is missing this replication feature. There are some workarounds that can be used to restore service in a disaster:

1. Save a snapshot frequently and store it off-site. In a disaster scenario, the remaining infrastructure can be used to spin up (or maybe it was already spun up) an OpenBao cluster and restore the snapshot. This method may sound a bit simplistic. I would, however, recommend considering this very-simple-to-maintain method.
2. Use PostgreSQL as a storage backend for Vault. In this scenario, you move the responsibility to synchronize data to PostgreSQL. The PostgreSQL environment that supports synchronization has quite a few moving parts, making it a bit more difficult to implement. I would recommend this pattern if your company/team is capable of delivering such a PostgreSQL environment.
3. Spread a cluster over datacenters, having one datacenter with "voters" and one datacenter with "non-voters". In case the datacenter with the voters is lost, the data is replicated to the non-voter datacenter. With a couple of commands, this non-voter cluster can be changed to a voter-cluster again. There is a limitation of less than 8ms latency between the datacenters.

> Note: There is [a](https://github.com/openbao/openbao/issues/1462) [lot](https://github.com/openbao/openbao/issues/38) of [work](https://github.com/openbao/openbao/issues/1528) planned to support replication.
### Performance standby

OpenBao/Vault using Raft have a "leader"-"follower" concept. With OpenBao and Vault (non-enterprise), followers will forward any request to the leader. With Vault Enterprise, a follower can be told to answer read-requests. This makes Vault Enterprise a bit more scalable. There is an [issue](https://github.com/openbao/openbao/issues/1528) to implement this on OpenBao.

> Note: Writes will always be dealt with by the leader. So the amount of scalability will depend on your workload. Typically, there are more reads than writes.

### Auto Snapshot

Making a ["snapshot"](https://openbao.org/api-docs/system/storage/raft/#take-a-snapshot-of-the-raft-cluster) (A file containing a backup) is an authenticated call. Which makes scripting this a bit annoying. Vault Enterprise has a concept of "auto-snapshot", where Vault Enterprise is configured to drop a snapshot to a destination and clean up old snapshots.

This is quite a convenient feature, although scripting this is certainly possible. Keep in mind that auto-snapshot cleans up old snapshots according to its configuration. This clean-up has to be configured differently; for example, on S3 a lifecycle policy can be set up.

> Note: This snapshot is only applicable to Raft.

### Sentinel

Sentinel is a "pre-policy-engine". It can check for certain conditions before even attempting to fetch (or write) a secret.

Regular policies are mostly sufficient to limit access, but there are edge-cases where Sentinel proves valuable.

I've implemented Sentinel a few times for customers and always struggled with the non-intuitive and very product-specific configuration.

OpenBao can use [CEL](https://openbao.org/docs/concepts/cel/) for this. Here too, it's quite specific and not particularly easy, but it exists.

### Extra: audit devices

All versions of Vault support "audit-devices". These are destinations to store audit logs. Typically "file", "syslog", or "socket" are used. OpenBao has an extra type: ["http"](https://openbao.org/docs/audit/http/).

| Device type | OpenBao | Vault | Vault Enterprise |
| --- | --- | --- | --- |
| file | ✅ | ✅ | ✅ |
| syslog | ✅ | ✅ | ✅ |
| socket | ✅ | ✅ | ✅ |
| http | ✅ | ❌ | ❌ |

Besides the extra "http" type, OpenBao has an extra method to configure the audit device: in the configuration file. This sounds trivial, but it's quite important. It means all requests, from the very first request, are audited. With Vault, any request done before the audit device is configured is not logged.

| Product | method(s) |
| --- | --- |
| OpenBao | Configuration file AND API calls |
| Vault | API calls |

### Extra: self init

OpenBao is sponsored by GitLab. GitLab would like to include a better secrets manager in their GitLab product. Since the HashiCorp Vault license does not allow redistributing Vault in another product, GitLab has invested in the OpenBao project. I just got (November 2025) information that OpenBao will likely be included in GitLab at the beginning of 2026.

Self init is a feature where OpenBao will configure secrets engines, authentication engines, and policies on first launch.

Now that sounds like a small feature, but just as the audit-device difference in OpenBao, it's quite important. It means no root-token is required anymore; you configure OpenBao to connect to, for example, LDAP and allow certain people to administer OpenBao.

The issue with the root-token is:

1. It's "god-mode", very powerful.
2. It's anonymous. The audit logs will never tell you WHO has used the root-token.

So with self-init, administrators can log in personally, making it much more traceable who did what. In combination with the audit devices in a configuration file, this closes the gaps that Vault potentially has with anonymous logins and with missed audit lines.

## Migration paths

### Vault (Community/OSS) to OpenBao

This is the simplest migration. You can (either):

- Replace the `vault` binary for `openbao`. This scenario keeps the data as Vault wrote it and lets OpenBao read the data.
- Make a snapshot from Vault (Community/OSS) and restore that snapshot on OpenBao. This allows you to try the migration before touching production.

### Vault Enterprise to OpenBao

This migration proves more difficult. The structure that Vault Enterprise (actually Raft) stores data is incompatible with both Vault (Community/OSS) and OpenBao.

Happily I work for [Adfinis](https://adfinis.com/) where we support many OpenBao/Vault/Vault Enterprise environments and have tooling available to migrate from Vault Enterprise to OpenBao.

## Conclusion

OpenBao is pretty capable, in some areas more capable than Vault. I'm expecting OpenBoa to be developed further and cover missing functionality soon.
