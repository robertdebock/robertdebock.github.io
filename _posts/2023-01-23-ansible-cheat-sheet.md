---
title: Ansible Cheat Sheet
---

# Ansible Cheat Sheet

Besides the [many modules](https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html), there are actually not that many tricks to Ansible. I'm assuming you know a bit about Ansible and would like to explore the edges of how Ansible works.

## Conditions

Conditionals determine when a task should run. You can check to see if something is in the state it should be before executing the task.

```yaml
- name: Do something
  ansible.builtin.debug:
    msg: "I will run if some_variable is defined."
  when:
    - some_variable is defined
```

The task `Do something` will on run if `some_variable` is set. You can [test many things](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_tests.html).

Conditions can be applied to:

- Tasks
- Blocks
- Playlists
- Roles

## Determining what makes a task `changed` or `failed`.

Sometimes Ansible needs some help determining what the result of a task is. For example the `ansible.builtin.command` module returns `changed`, because Ansible assumes you've modified the node. In these cases, you will need to tell Ansible when it's changed. The same can be used to `fail` a task.

```yaml
- name: Do something that does not change the system
  ansible.builtin.command:
    cmd: ls
  changed_when: no

- name: Do something and help Ansible determine when the task is changed
  ansible.builtin.command:
    cmd: some_command_that_can_change_things
  register: my_command
  changed_when:
    - '"modified" in my_command.stdout'
  failed_when:
    - '"error" in my_command.stdout' 
```

The first example never changes, in this case, that's true; `ls` is a read-only command.

The second example reports `changed` when `modified` can be found in the output. As you can see, you do need to use `register` to save the output of the command.
Ansible reports `failed` when `error` is found in the output.

> Note: Ansible will fail by default if the `cmd` returns a non-0 exit code.

## `command`, `shell` or `raw`.

## Notify and handlers

Handlers are used to run after some task changed. It's useful to "gather" handlers and run them at the end. A classic example:

```yaml
tasks:
  - name: Place a configuration.
    ansible.builtin.template:
      src: some_config.conf.j2
      dest: /etc/some_config.conf
    notify:
      - Restart some_service

handlers:
  - name: Restart some_service
    ansible.builtin.service:
      name: some_service
      state: restarted
```

In the above snippet, the task places a configuration and informs the handler `Restart some_service`. This restart will occur at the end of a play.

There are some tricks:

## It's a list!

The `notify` argument takes a list:

```yaml
- name: Do something
  ansible.builtin.command:
    cmd: ls
  notify:
    - First handler
    - Second handler
```

I like to spread things vertically, it improves readability.

### Ordering

The order of the handlers is determined the order of the tasks as written in the `handlers`. Take this example:

```yaml
handlers:
  - name: First
    # Something
  - name: Second
    # Something
  - name: Third
    # Something
```

Using the task in the handlers above, the order will always be:

1. `First`
2. `Second`
3. `Third`

(And a handler can be skipped by not notifying it.)

### Chaining

You can chain handlers. Sometimes this helps to get a role idempotent.

```yaml
tasks:
  - name: Do the first thing
    ansible.builtin.command:
      cmd: ls
    notify:
      - Do the second thing

handlers:
  - name: Do the second thing
    ansible.builtin.command:
      cmd: ls
    notify:
      - Do the third thing

  - name: Do the third thing
    ansible.builtin.command:
      cmd: ls

Makes me wonder if we can loop...

### Conditions

You can add a condition to a handler, just like any other task.

```yaml
- name: I am a handler
  ansible.builtin.command:
    cmd: ls
  when:
    - some_variable is defined
```

### Running handlers now

If required, you can tell Ansible to run the handlers right now. You'd typically wait until the end of the play, but there are certainly situations where the handlers need to run right now.

```yaml
tasks:
  - name: Do something
    ansible.command.command:
      cmd: ls
    notify:
      - Some handler

  - name: Flush the handlers
    ansible.builtin.meta: flush_handlers

  - name: Do other things
    ansible.command.command:
      cmd: ls

handlers:
  - name: Some handler
    ansible.command.command:
      cmd: ls
```

The order of the above tasks will be:

1. Do something
2. Flush the handlers
3. Some handler
4. Do other things

## Delegating tasks

There are not many use-cases for `delegate_to`, but some will apply. Consider this snippet:

```yaml
- name: Do something to the API
  ansible.builtin.uri:
    url: "https://some.host.name/api/v1/user
    method: GET
  delegate_to: localhost
```

The example above gets information from an API. There is no specific need to run this API call on the targeted node. In such cases, you can use `delegate_to`.

The examples I can think of:

1. Using the `ansible.builtin.uri` module.
2. Copying files between nodes using the `ansible.posix.synchronize` module.

## Running a job just once

In some situations you want to do something to just one node from all targeted nodes.

```yaml
tasks:
  - name: Do something
    ansible.builtin.command:
      cmd: ls
    run_once: yes
```

These situations may call for `run_once`, there are probably many more:

- Selecting a host that will become a cluster leader/master/primary.
- Checking if a variable is set correctly.
- Using `delegate_to` to do something once locally.
- Managing a cluster that's also created in the same role.

## Checking user input

argument_specs
assert

## Determine the amount of nodes to run on simultanious.

serial

## Grouping tasks

block


### Catching failures

block
rescue
always

## Variables

## General

1. Optimize for readability.
2. Spread things vertically when possible.
