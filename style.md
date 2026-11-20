# [Style guide](#style-guide)

In general the [Ansible best practices](https://docs.ansible.com/ansible/latest/tips_tricks/ansible_tips_tricks.html) are used. These are the explicit conventions for the roles. The same text is available to coding assistants in `AGENTS.md` in every role.

- Prefer readability over complexity.
- Split logic into roles to allow composing a combination of roles.
- Roles do not solve business logic, they offer system logic. Playbooks combine roles into business logic.
- Roles need to be compatible with the distributions listed in `meta/main.yml`.
- `ansible-lint` should succeed without errors.

## [Development focus ordering](#development-focus-ordering)

1. Focus on the intent of the role in `meta/main.yml`.
2. Focus on user variables, defined in `defaults/main.yml`.
3. Focus on role variables, defined in `vars/main.yml`.
4. Focus on tasks, templates and files.

## [Meta](#meta)

- `galaxy_info.description` in `meta/main.yml` captures the intent of the role.
- `meta/main.yml` is used to generate many other files with [ansible-generator](https://github.com/robertdebock/ansible-generator).
- Only use `dependencies` if it is really required, for example for a shared handler or variable. It may surprise a user with extra roles being downloaded and executed. Soft dependencies belong in `requirements.yml` and `molecule/default/prepare.yml`.
- The distributions mentioned are tested using Molecule, see [testing](testing.html).

## [User variables](#user-variables)

Defined in `defaults/main.yml`.

- The user will need to adjust these settings, so making life easier in this file is helpful.
- Variables start with the name of the role. The role `ansible-role-xyz` has variables that start with `xyz_`. This limits variable bleeding and helps a user understand where a variable is applicable.
- Consider carefully if a variable is a string, integer, boolean, list or map. Changing a variable type later is difficult for users.
- Above the variable, in a comment, describe the behaviour of the variable.
- Test all variables with `ansible.builtin.assert` in `tasks/assert.yml`, and describe them in `meta/argument_specs.yml`. Fail as fast as possible.

## [Role variables](#role-variables)

Defined in `vars/main.yml`.

- These are "internal variables". The user should not be interested in them, they are difficult to overwrite.
- Determine a value with a lookup map:

  ```yaml
  _xyz_packages:
    default:
      - foo
      - bar
    Suse:
      - foobar

  xyz_packages: "{{ _xyz_packages[ansible_facts['os_family']] | default(_xyz_packages['default']) }}"
  ```

  This pattern can be used for packages, service names, users and groups, and logic in `tasks/main.yml`.
- Use `ansible_facts['os_family']` and not `ansible_os_family`.
- Resolve values in `vars/main.yml` to prevent unnecessary `when` conditions.

## [Tasks](#tasks)

- Use `name:` on every task and describe the intent of the task, which may differ slightly from what the task does:

  ```yaml
  # Describes the intent of a task (strong)
  - name: Configure xyz

  # Describes what the task does (weak)
  - name: Place xyz.conf
  ```

- Spread lines vertically as much as possible, it is more readable:

  ```yaml
  # Easy to read (vertical)
  - name: Do something
    ansible.builtin.file:
      src: foo
      dest: /foo
    when:
      - xyz
    failed_when:
      - xyz == "xyz"
    notify:
      - XYZ Handler

  # Difficult to read (horizontal)
  - name: Do something
    ansible.builtin.file:
      src: foo
      dest: /foo
    when: xyz
    failed_when: xyz == "xyz"
    notify: XYZ Handler
  ```

  An added benefit: it is obvious that `when`, `failed_when`, `changed_when` and handlers are (AND) lists.
- Never use an equal sign (`=`) where a colon (`:`) is possible.
- Never use Jinja expressions in tasks or handlers: no filters, no `default()`, no conditionals, no string joining. Compute the value in `vars/main.yml` (for example with a mapping) and refer to that role variable by its name, `"{{ xyz_state }}"`.
- Write lists top to bottom, one item per line, also when there is a single item (`when`, `failed_when`, `changed_when`, `notify`, `tags`, `loop`, packages). Never `[ "a", "b" ]`. The reason is readability: a task has to be understandable years later.
- Do not write an empty list or map (`[]`, `{}`) as a default. Leave the variable undefined (show an example in a comment) and guard its use with `when: xyz is defined`.
- A condition that needs `and` and `or` is written as list items (AND) with the `or` group in parentheses, one condition per line:

  ```yaml
  when:
    - >-
      (
        ansible_facts['distribution'] == "Amazon"
        and ansible_facts['distribution_major_version'] == "2"
      )
      or
      (
        ansible_facts['os_family'] == "RedHat"
      )
  ```

- Task names start with a lowercase prefix that names the area, then a capitalised sentence, for example `assert | Test xyz_port`. Tasks in `tasks/main.yml` don't need a prefix: `Install xyz`.
- Use FQCNs for modules, and mention the used collections in the role's `requirements.txt` and `requirements.yml`.
- Prefer modules over `ansible.builtin.shell` and `ansible.builtin.command`.
- Always mention `state:` where a module has it.
- `ansible.builtin.file` mentions `owner`, `group` and `mode`.
- Packages do not use `latest`, unless the role is intended to update the package.
- Avoid `ansible.builtin.set_fact`.
- For error handling, prefer `block` and `rescue`. `failed_when: true` is never preferred, just as `ignore_errors: true`.
- Only use `gather_facts: true` when the facts are required.

## [Handlers](#handlers)

- Don't use the `listen` directive.

## [Templates](#templates)

- Put `{{ ansible_managed | comment }}` (with the correct comment style) at the top of a template.
- Although difficult, try to prevent Jinja in templates. There are exceptions.

## [Files](#files)

- It is nearly always beneficial to use a template. The exception is a binary file (keep in mind that Ansible is not a packaging solution).

## [Molecule](#molecule)

- `molecule/default/verify.yml` tests the intent of the role. That can be: test that a port is open, or that an installed product can show a version number.

## [Linting](#linting)

All roles pass `ansible-lint`, configured in `.ansible-lint` (it excludes the molecule and meta preferences files and enables `name[prefix]`). Lint has to pass before the Molecule job runs in CI, see [testing](testing.html).

No role skips lint rules for the whole role. Where a single task can't meet a rule, an inline `# noqa` comment is used on that task only, with the rule name.
