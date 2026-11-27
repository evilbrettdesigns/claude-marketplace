# Security Policy

## Reporting a vulnerability

Please do not open a public issue for security problems.

Report privately through GitHub: go to the **Security** tab of this repository and choose
**Report a vulnerability**. That opens a private advisory that only the maintainers can see.

Useful things to include:

- The plugin, skill, agent or command affected, and the file path.
- What an attacker could do (for example, a prompt that makes a skill run an unexpected
  shell command, leak an environment variable, or write a secret to disk).
- Steps or a prompt that reproduces it.

You can expect an acknowledgement within 7 days. Fixes ship as a new plugin version on `main`.

## Scope

In scope: everything in this repository, including plugin manifests, skills, agents, commands,
hooks and the marketplace catalog.

Out of scope: vulnerabilities in Claude Code itself (report those to Anthropic) and in
third-party services a plugin talks to, such as the Font Awesome API.

## Supported versions

Only the latest version of each plugin on `main` receives fixes.
