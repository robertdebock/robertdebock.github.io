# My Manifesto

To keep a sharp eye on what is "right" in software development, here are a few rules I agree with.

## Priorities
When working on code, these are priorities:
1. User reported bugs.
2. CI/CD reported bugs.
3. Features.

## Where to fix
Solve bugs and dependencies in the lowest level possible. From low to high:
1. Package management.
2. Configuration management.
3. Application.

## Packages
A package should be autonomously:
- Installable.
- Updateable.
- Removable.
- Reinstallable.

## Configuration management
A configuration management contains:
- References to packages.
- Configuration files.
- Commands to activate configuration.
- A method to persistently enable software.

## Simplicity
- Code should be understandable by anybody. If it's to difficult to draw on a single piece of paper, simplify it.
- Code (RPM, playbook, etc) serves the smallest functionallity possible.

## Dependencies
Use dependencies when absolutely required, in other words: only use dependecies when two entities have no value without eachother. This ensures:
- Code can be reused maximally.
- Code can be forked.
- Assumptions are left over to the integrator.
 
## Testability
Keep the smallest (testable) related code in a repository. This ensures autonomous development, most independant testing and easy collaboration.
 
There are multiple types of code:
- The code for the application - Typically Python, C, PHP, etc.
- Code packaging - Typically RPM, NPM, or PIP.
- Code for configuration - Typically Ansible or Puppet.
- The pipeline - Typically Travis-CI or GitLab-CI.
- Test code - Typically Ansible playbooks, bats, goss or bash.

## Integration
Testing (integration) happens on an environment that's production-like.

Also; see [my purpose](https://github.com/robertdebock/purpose)
