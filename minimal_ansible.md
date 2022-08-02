# Minimal Ansible

Here is the bare minimum to learn about Ansible. If this is your first encounter of Ansible, this is likely not the best start; it's pretty condensed.

## Playbooks

A playbook contains a header to indicate the environment specific details and a list of `roles` and `tasks`. (Also `pre_tasks` and `post_tasks` can be used.)

```yaml
- name: setup my servers
  hosts: all
  become: yes
  gather_facts: yes

  pre_tasks:
    - name: show something
      ansible.builtin.debug:
        msg: "something"

  roles:
    - role: role_x

  tasks:
    - name: show something else
      ansible.builtin.debug:
        msg: "something else"

  post_tasks:
    - name: show a final thing
      ansible.builtin.debug:
        msg: "a final thing"
```

The order is:

1. pre_tasks. (And flush handlers in `pre_tasks`.)
2. roles.
3. tasks. (And flush called handlers for `roles` and `tasks`.)
4. post_tasks. (And flush called handlers in `post_tasks`.)

## Handlers

A task can call a `handler` using notify, which is a list of references to the name of the handler. A change on the task that calls the handler, will execute the handler. Order of running the handlers is the order of appearance in `handlers/main.yml`.

```yaml
- name: setup my servers
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: do something
      ansible.builtin.command:
        cmd: echo "A command is changed by default."
      notify:
        - run last
        - run first

  handlers:
    - name: run first
      ansible.builtin.debug:
        msg: "I will run first"
    - name: run last

      ansible.builtin.debug:
        msg: "I will run last" 
```

## Blocks

Blocks can be used to organize your list of tasks. A block can share:
- conditions. (when)
- variables.

The `rescue` (list of tasks) will run when a task in the `block` (list of tasks) errors. The `always` (list of tasks) will always run.

```yaml
- name: setup my servers
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: do all
      block:
        - name: fail on purpose
          ansible.builtin.command:
            cmd: /bin/false

        - name: this task not started
          ansible.builtin.debug:
            msg: "You will not see this."

      resuce:
        - name: run on failures
          ansible.builtin.debug:
            msg: "This will be seen."

      always:
        - name: always run
          ansible.builtin.debug:
            msg: "This will run, no matter of the success or failures."
```

## changed_when, failed_when, when

Tasks typically report correctly;
- "OK" (Green) when the state matches, no changes are required.
- "CHANGED" (Yellow) when the state is changed.
- "FAILED" (Red) when the task failed.
- "SKIPPED" (Blue) when the condion (`when`) on the task is false.

You may however "help" Ansible:

```yaml
- name: setup my servers
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: read some state
      ansible.builtin.command:
        cmd: id
      changed_when: no

    - name: write some other state
      ansible.builtin.command:
        cmd: start_backup.sh
      register: write_some_other_state
      changed_when:
        - '"DONE" in write_some_other_state.stdout'
      failed_when:
        - '"FAILED" in  write_some_other_state.stdout'
```

Note; if a program exits with a non-zero exit code, Ansible will mark the task as failed.

## check mode

You can run a playbook in check mode and/or run a task in check mode. Check mode does not make any changes to the targeted system.

Run a playbook without actually doing anything:

```shell
ansible-playbook playbook.yml --check
```

Note, some playbooks have tasks that depend on each other. Image this playbook:

```yaml
- name: will not work in check mode
  hosts: all
  become: no
  gather_facts: no

  tasks:
    - name: step one
      ansible.builtin.file:
        path: /tmp/one
    
    - name: 