# [My Manifesto](#my-manifesto)

To keep a sharp eye on what is "right" in software development, here are a few rules I agree with.

## [Priorities](#priorities)
When working on code, these are priorities:
1. Failing CI
2. User reported bugs.
3. Self reported bugs.
4. New features.

## [Where to fix](#where-to-fix)
Solve bugs and dependencies in the lowest level possible. From low to high:
1. Package management.
2. Configuration management.
3. Application.

## [Packages](#packages)
A package should be able to autonomously:
- Install.
- Update.
- Remove.
- Re-install.

## [Configuration management](#configuration-management)
A configuration management contains:
- References to packages.
- Configuration files.
- Commands to activate configuration.
- A method to persistently enable software.

## [Simplicity](#simplicity)
- Code should be understandable by anybody. If it's to difficult to draw on a single piece of paper, simplify it.
- Code (RPM, playbook, etc) serves the smallest functionality possible.
- Push complexity to locations that users will not be bothered, keep the "interface" as simple as possible.
- Start with code that *barely* works. That means some assumptions will be made.

## [Dependencies](#dependencies)
Use dependencies when absolutely required, in other words: only use dependencies when two entities have no value without each other. This ensures:
- Code can be reused maximally.
- Code can be forked.
- Assumptions are left over to the integrator.
 
## [Testability](#testability)
Keep the smallest (testable) related code in a repository. This ensures autonomous development, most independent testing and easy collaboration.
 
There are multiple types of code:
- The code for the application - Typically Python, C, PHP, etc.
- Code packaging - Typically RPM, NPM, or PIP.
- Code for configuration - Typically Ansible or Puppet.
- The pipeline - Typically Travis-CI or GitLab-CI.
- Test code - Typically Ansible playbooks, bats, goss or bash.

## [Integration](#integration)
Testing (integration) happens on an environment that's production-like.

Also; see [my purpose](https://github.com/robertdebock/purpose)
