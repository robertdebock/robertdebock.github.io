# My Manifesto

The keep a sharp eye on what is "right" in software development, here are a few rules I agree with.

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

## Simplicity
- Code should be understandable by anybody. If it's to difficult to draw on a single piece of paper, simplify it.
- Code (RPM, playbook, etc) serves the smallest functionallity possible.

## Dependencies
Tight dependencies explicitly define dependencies, loose dependecies indicate what the requirements are (for example in documentation) but do not include the dependecies by default.
Popular software typically use loose dependencies.

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
