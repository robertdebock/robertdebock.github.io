---
title: Ansible Cheat Sheet
---

# Ansible Cheat Sheet

Besides the [many modules](https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html), there are actually not that many tricks to Ansible. I'm assuming you know a bit about Ansible and would like to explore the edges what can be done with Ansible.

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

Good to know: a `when:` statement accepts strings (`when: "{{ variable }}" == "hello'`) or can accept lists:

```yaml
- name: Do something
  ansible.builtin.debug:
    msg: "I will run if some_variable is defined."
  when:
    - some_variable is defined
    - somm_variable ==  "hello"
```

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

### It's a list!

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
```

You can create an not-so-infinite loop, by letting handler_one call handler_two and let handler_two call handler_one. This loops up to 3 times, not sure what prevents Ansible from an infinite loop, but thanks Ansible developers!

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

When writing Ansible roles, checking user input ensures your role can work. Imagine you are asking for a list, but a user specifies a boolean.

```yaml
# This is a list.
my_variable_list:
  - one
  - two
  - three

# This is a string, not a list.
my_variable_list: hello
```

In the example above, the same varialbe is set twice (last one wins). The bottom one seems incorrect, as it's not a list.

You can use at least two methods to verify if the input is correct; argument_specs or assert.

### argument_specs

The [documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html#role-argument-validation) contains all details, but let's take one example:

```yaml
---

argument_specs:
  main:
    short_description: Install and configure Elasticsearch on your system.
    description: Install and configure Elasticsearch on your system.
    author: Robert de Bock
    options:
      elasticsearch_type:
        type: "str"
        default: oss
        description: "The license to use, `elastic` uses the \"Elastic\" license, `oss` uses the \"Apache 2.0\" license."
        choices:
          - elastic
          - oss
      elasticsearch_network_host:
        type: "str"
        default: "0.0.0.0"
        description: The IP address to bind on.
      elasticsearch_network_port:
        type: "int"
        default: 9200
        description: The port to bind on.
      elasticsearch_discovery_seed_hosts:
        type: "list"
        default: []
        description: Provides a list of the addresses of the master-eligible nodes in the cluster.
      elasticsearch_cluster_initial_master_nodes:
        type: "list"
        default: []
        description: Sets the initial set of master-eligible nodes in a brand-new cluster.
```

Benefits of using arguments_specs include:

- Fast
- Built in (since +- 2020)
- Can be used to generate docs.

A limitation is that you can only test a single variable, no combinations and the tests on that single variable are limited.

### assert

You can also `ansible.builtin.assert` variables.

```yaml
---

- name: Include assert.yml
  ansible.builtin.import_tasks: assert.yml
  run_once: yes
  delegate_to: localhost
```

```yaml
---

- name: Test if elasticsearch_type is set correctly
  ansible.builtin.assert:
    that:
      - elasticsearch_type is defined
      - elasticsearch_type is string
      - elasticsearch_type in [ "elastic", "oss" ]
    quiet: yes

- name: Test if elasticsearch_network_host is set correctly
  ansible.builtin.assert:
    that:
      - elasticsearch_network_host is defined
      - elasticsearch_network_host is string
    quiet: yes

- name: Test if elasticsearch_http_port is set correctly
  ansible.builtin.assert:
    that:
      - elasticsearch_http_port is defined
      - elasticsearch_http_port is number
      - elasticsearch_http_port >=1
      - elasticsearch_http_port <= 65536
    quiet: yes

- name: Test if elasticsearch_discovery_seed_hosts is set correctly
  ansible.builtin.assert:
    that:
      - elasticsearch_discovery_seed_hosts is defined
      - elasticsearch_discovery_seed_hosts is iterable
    quiet: yes

- name: Test if elasticsearch_cluster_initial_master_nodes is set correctly
  ansible.builtin.assert:
    that:
      - elasticsearch_cluster_initial_master_nodes is defined
      - elasticsearch_cluster_initial_master_nodes is iterable
    quiet: yes
```

This does something similar as the `argument_specs` described earlier.

Benefits of `assert` include:

- You can test more things of a single variable.
- You can set an error message.
- You can combine tests on multiple variables.

## Determine the amount of nodes to run on simultanious.

Sometimes you want to limit the number of nodes being addressed at one time. One-host-at-a-time for example:

```yaml
- name: Do something machine-by-machine
  hosts: web_servers
  serial: 1

  ...
```

In the example above, only one node will be targeted at a time. This can be useful for updates to hosts behind a load balancer for example.

## Host by host or as fast as possible

The normal `strategy` of Ansible is `linear`: Execute one task on targeted nodes, after that, continue with the next task:


```text
TASK [Do step one] ***
changed: [node_1]
changed: [node_2]
changed: [node_3]

TASK [Do step two] ***
changed: [node_1]
changed: [node_2]
changed: [node_3]
```

You can also instruct Ansible to "go as fast as possible" by setting the `strategy` to `free`. This runs each task on targeted nodes, but does not wait until all hosts have completed a task. This also means there are limitations; when ordering of the tasks is important, this strategy can't be used.


```yaml
- name: Do something as fast as possible
  hosts: web_servers
  strategy: free

  tasks:
    - name: Update all software
      ansible.builtin.package:
        name: "*"
        state: latest
      notify:
        - Reboot

  handlers:
    - name: Reboot
      ansible.builtin.reboot:
```

The above sequence runs the jobs (`Update all software` and `Reboot`) as fast as possible; one node may be updating, the other can be rebooting, or even already be finished.

## Grouping tasks

In a task-list it can be useful to group tasks. Likely because they share something. (A `notify` or `when` for example.) You can even group tasks because you like it, or if it increases readability.

```yaml
- name: Do something to Debian
  when:
    - ansible_distribution == "Debian"
  notify:
    - Some Debian handler
  block:
    - name: Taks 1
      ...
    - name: Task 2
```

The block (`Do something to Debian`) shares a `notify` and `when` for both tasks. 

- When the condition (`when`) is met, two tasks will run.
- When any of the tasks is `changed`, the handler `Some Debian handler` will be called.

### Catching failures

The `block` statement can also be used to "catch" a possible failure.

```yaml
- name: Do something
  hosts: web_servers
  become: no
  gather_facts: no

  tasks:
    - name: Start the webserver
      block:
        - name: Start httpd
          ansible.builtin.service:
            name: httpd
            state: started
      rescue:
        - name: Install httpd
          ansible.builtin.package:
            name: httpd
      always:
        - name: Show a message
          ansible.builtin.debug:
            msg: "Hello!"
```

Although the example above makes no sense, it does demonstrate how the `rescue` statement works. "If starting httpd fails, install httpd." (What's incorrect here is that there can be many more reasons for httpd to fail, plus if it fails, httpd would be installed, but never started. Weird.
The `alwasy` will run anyway, no matter what the first (`Start httpd`) task does.

I'm not a big fan of `block`, `rescue` & `always`, it increases complexity, can possible show red output (errors), which can throw of people. I would rather use some tasks sequence that first checks things, and based on the results, execute some action.

## Variables

Wow, this is going to be a large chapter. Variables can be set and used in many ways in Ansible. Let's cover a few edge-cases.

### Gathering facts

`facts` are details of a node, for example `ansible_distribution`. These can be used anywhere to make desicions. A common way to use this:

```yaml
- name: Do something on Debian systems
  ansible.builtin.debug:
    msg: "I am running {{ ansible_distribution }}."
  when:
    - ansible_distribution == "Debian"
```

You can also map values to a distribution in `vars/main.yml`:

```yaml
_package_to_install:
  Debian: x
  RedHat:
    - y
    - z

package_to_install: "{{ _package_to_install[ansible_distibution] }}"
```

The `package_to_install` variable can now be used in a regular task, for example: `tasks/main.yml`

```yaml
- name: install package
  ansible.builtin.package:
    name: "{{ package_to_install }}"
```

You can also introduce a `default` value:

```yaml
_package_to_install:
  default: x
  Debian: y
  RedHat: z

package_to_install: "{{ _package_to_install[ansible_distibution] | default(_package_to_install['default']) }}"
```

This way of working allows the `tasks/main.yml` to be readable.

### Use a variable from another host

Image you have a couple of hosts and you've `set_fact` or `register`-ed something on `node-1` that you need on `node-2`.

```yaml
- name: Set a variable on node-1
  hosts: node-1

  tasks:
    ansible.builtin.set_fact:
      some_variable: some_value

- name: Get a variable from node-1 on node-2
  hosts: node-2

  tasks:
    ansible.builtin.debug:
      msg: "The value of some_variable on node-1 is: hostvars['node-1']['some_variable']
```

## Complexity

Sometimes you need a bit of complexity to finish a role or playbook. I would suggest to use this list, starting with "keep it simple", going down to "hide your complexity here".

1. `default/main.yml` (Or any other user-overwritable variable) - Keep this very simple. Users will interface with your code here.
2. `tasks/*.yml` and `handlers/*.yml` - This can be a little more complex. Users are not expected to read this, some will peek here.
3. `vars/main.yml` - This is where complexity lives. Keep all your data, filters and lookups here when possible.

## Lint

Ansible lint is always right.

Well, that being said, you can overrule ansible-lint by setting the `  # noqa` argument. (Mind you: space-space-hash-space-identifier)

```yaml
- name: Update all software (yum)
  ansible.builtin.yum:
    name: "*"
    state: latest  # noqa package-latest This role is to update packages.
```

Try to understand what Ansible lint is trying to tell you, likely you need to restructure your code until Ansible lint is happy.

## General

1. Optimize for readability.
2. Spread things vertically when possible.
