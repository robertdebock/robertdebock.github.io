---
title: Why would you write Ansible roles for multiple distributions?
---

# Why would you write Ansible roles for multiple distributions?

I got some feedback in a discussion with the audience at [DevOps Amsterdam](https://www.meetup.com/DevOpsAmsterdam/).

My statement are:

- "Keep your code as simple as possible"
- "Write roles for multiple distributions" (To improve logic.)

These two contradict each other: simplicity would mean 1 role for 1 (only my) distribution.

Hm, that's a very fair point. Still I think writing for multiple operating systems is a good thing, for these reasons:

1. You get a better understanding of all the operating systems. For example Ubuntu is (nearly) identical to Debian, SUSE is very similar to Red Hat.
2. By writing for multiple distributions, the logic (in `tasks/main.yml`) becomes more stable.
3. It's just very useful to be able to switch distributions without switching roles.
