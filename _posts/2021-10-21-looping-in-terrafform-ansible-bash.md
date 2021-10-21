---
title: Looping in Terraform, Ansible and Bash
---

# Looping in Terraform, Ansible and Bash

Looping is quite an important mechanism in coding. (Thanks [@Andreas](https://in0rdr.github.io/) for the word `coding`, a mix of scripting and programming.)

Looping allows you to write a piece of logic once, and reuse it as many times are required.

Looping is difficult to understand for new people new to coding. It's sometimes difficult for me to. This article will probably help me a bit too!

## A sequence

The simplest loop I know is repeating a piece of logic for a set of number or letters, like this:

- `1 2 3`

First off, here is how to generate a sequence:

| bash      | ansible                        | terraform     |
|-----------|--------------------------------|---------------|
| `seq 1 3` | `with_sequence: start=1 end=3` | `range(1, 3)` |
| `{1..3}`  |                                |               |

For all languages, this returns a list of numbers.

| bash    | ansible                     | terraform      |
|---------|-----------------------------|----------------|
| `1 2 3` | `item: 1, item: 2, item: 3` | `[ 1, 2, 3, ]` |

So, a more complete example for the three languages:

### Bash

```shell
for number in {1..3} ; do
  echo "number: ${number}
done
```

The above script returns:

```shell
1
2
3
```

### Ansible

```yaml
- name: show something
  debug:
    msg: "number: {{ item }}"
  with_sequence:
    start=1
    end=3
```

Note: See that `=` sign, I was expecting a `:` so made an [issue](https://github.com/ansible/ansible/issues/76102).

The above script returns:

```text
ok: [localhost] => (item=1) => {
    "msg": "1"
}
ok: [localhost] => (item=2) => {
    "msg": "2"
}
ok: [localhost] => (item=3) => {
    "msg": "3"
}
```

### Terraform

```hcl
locals {
  numbers = range(1,4)
}

output "number" {
  value = local.numbers
}
```

The above script returns:

```text
number = tolist([
  1,
  2,
  3,
])
```
