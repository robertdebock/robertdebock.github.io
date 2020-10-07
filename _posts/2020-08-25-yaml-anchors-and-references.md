---
title: YAML Anchors and References
---

# YAML Anchors and References

This is a post to help me remind how YAML Anchors and References work.

## What?

In YAML you can make an Anchor:

```yaml
- first_name: &me Robert
```

Now the Anchor `me` contains `Robert`. To refer to it, do something like this:

```yaml
- first_name: &me Robert
  give_name: *me
```

The value for `given_name` has been set to `Robert`.

You can also anchor to a whole list item:

```yaml
people:
  - person: &me
    name: Robert
    family_name: de Bock
```

No you may refer to the `me` anchor:

```yaml
access:
  - person: *me
```

Now `Robert` has `access`.
  
Good to prevent [dry](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).
