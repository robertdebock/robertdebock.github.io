---
title: Event Driven Ansible
---

# Event Driven Ansible

Okay, so RedHat has released [Event Driven Ansible](https://www.redhat.com/en/technologies/management/ansible/event-driven-ansible). It's a new way to execute playbooks, based on events.

These events originate from [sources](https://github.com/ansible/event-driven-ansible/tree/main/extensions/eda/plugins/event_source), a couple to highlight:

- Kafka: If a certain message is found on the bus, execute a playbook.
- Webhook: If a certain URL (on the Ansible Automation Platform Event Driven Ansible node) is called, execute a playbook.

This feels like a new way to remediate issues in an environment, for example: If a monitoring system detects a situation, call Ansible to remediate.

I've worked at companies that did "self healing" as a core-business, my conclusion so far.

> If something is broken, there is a problem with the design. For example:

- If a disk is full, you may want to clean-up/compress data. The actual solution would be to add more storage.
- If a service stops unexpectedly, you may want to restart it. The actual problem is that the software needs to be fixed or the configuration needs to be improved.

Anyway, even with the above in mind, here is how Event Driven Ansible works.

```text
+--- event source ---+   +--- event driven ansible ---+   +--- ansible controller ---+
| Kafka              | > | Rulebook                   | > | Playbook                 |
+--------------------+   +----------------------------+   +--------------------------+
```

Now Ansible Automation Platform has the Ansible Controller, on the right in the diagram above. AAP has an API already, so it feels somewhat redundant to have Event Driven Ansible. But. A difference is that Event Driven Ansible can react to something.

Use-cases I can think of:

- A change starts, Event Driven Ansible can put a node in maintenance mode.
- An issue occurs, Event Driven Ansible collects information about a node. This means the engineer in charge can start troubleshooting with the latest information.
- A deployment of a new node is done, Event Driven Ansible can configure the node. (Drawback; it becomes quite unpredictable when the node is ready, as this happens asynchronously from the deployment.)
- A node is removed, Event Driven Ansible can remove the node from monitoring/backup/etc.

I'm looking forward to see where Event Driven Ansible will go to in the future, it's at least a new way to interact with Ansible.
