---
title: Terraform loops
---

# Terraform loops

There are a couple of ways to loop in Terraform. Let's have a look.

## Count

This is the "oldest" method. It's quite powerful.

```hcl
resource "some_resource" "some_name" {
  count = 3
  name  = "my_resource_${count.index}"
}
```

As you can see, the resource `some_resource` is being created 3 (`count = 3`) times.
The `name` should be unique, so the `count.index` variable is used. This variable is available when using `count`.

The variable `count.index` has these values:

| itteration | `count.index` value |
|------------|---------------------|
| first      | `0`                 |
| second     | `1`                 |
| third      | `2`                 |

And so on.

### Counting and looking up values

The parameter `count` sounds simple, but is actually quite powerful. Lets have a look at this example.


Here is a sample `.tfvars` file:

```hcl
virtual_machines = [
  {
    name = "first"
    size = 16
  },
  {
    name = "second"
    size = 32
  }
]
```

The above structure is a "list of maps":

- List is indicated by the `[` and `]` character.
- Map is indicated by the `{` and `}` character.

Now lets loop over that list:

```hcl
resource "fake_virtual_machine" "default" {
  count = length(var.virtual_machines)
  name  = var.virtual_machines[count.index].name
  size  = var.virtual_machines[count.index].size
}
```

A couple of tricks happen here:

1. `count` is calculated by the [`length`](https://www.terraform.io/docs/language/functions/length.html) function. It basically counts how many maps there are in the list `virtual_machines`.
2. `name` is looked-up in the variable `var.virtual_machines`. Pick the first (`0`) entry from `var.virtual_machines` in the first itteration.
3. Similarly `size` is looked up.

This results in two resources:

```hcl
resource "fake_virtual_machine" "default" {
  name  = "first"
  size  = 16
}

# NOTE: This code does not work; `default` may not be repeated. It's just to explain what happens.
resource "fake_virtual_machine" "default" {
  name  = "second"
  size  = 32
}
```

## For each

The looping mechanism `for_each` can also be used. Similar to the `count` example, let's think of a data structure to make virtual machines:

```hcl
virtual_machines = [
  {
    name = "first"
    size = 16
  },
  {
    name = "second"
    size = 32
  }
]
```

And let's use `for_each` to loop over this structure.

```hcl
resource "fake_virtual_machine" "default" {
  for_each = var.virtual_machines
  name = each.value.name
  size = each.value.size
}
```

This pattern creates exactly the same resources as the `count` example.

## Dynamic block

Imagine there is some block in the `fake_virtual_machine` resource, like this:

```hcl
resource "fake_virtual_machine" "default" {
  name = "example"
  disk {
    name = "os"
    size = 32
  }
  disk {
    name = "data"
    size = 128
  }
}
```

The variable structure we'll use looks like this:

```hcl
virtual_machines = [
  {
    name = "first"
    disks [
      {
        name = "os"
        size = 32
      },
      {
        name = "data"
        size = 128
      }
    ]
  },
  {
    name = "second"
    disks [
      {
        name = "os"
        size = 64
      },
      {
        name = "data"
        size = 256
      }
    ]
  }
]
```

As you can see:

- A list of `virtual_machines`.
- Each virtual_machine has a list of `disks`.

Now let's introduce the dynamic block:

```hcl
resource "fake_virtual_machine" "default" {
  for_each = var.virtual_machines
  name = each.value.name
  dynamic "disk" {
    for_each = each.value.disks
    content {
      name = disk.value.name
      size = disk.value.size
    }
  }
}
```

Wow, that's a lot to explain:

1. The `dynamic "disk" {` starts a dynamic block. The name ("disk") must reflect the parameter in the resource, not juts any name. Now a new object is available; `disk`.
2. The `for_each = each.value.disks` loops the dynamic block. The loop runs over an already looped value; `var.virtual_machines`.
3. The `content {` block will be rendered by Terraform.
4. The `name = disk.value.name` uses the `disk` variable (created by the block `iterator` `disk`) to find the value from the `disks` map.

Hope that helps a bit when writing loops in Terraform!
