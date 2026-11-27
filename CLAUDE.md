# CLAUDE.md

Instructions for Claude (and anyone else) working in this repo. This is a public,
MIT-licensed Claude Code plugin marketplace. Everything committed here is published.

## Branches

- Name every branch `<type>/<short-description>`, where `<type>` is one of
  `bug`, `chore`, `feature`, `hotfix` or `release`, and the description is lowercase
  kebab-case. Examples: `feature/project-lead-plugin`, `bug/fontawesome-kit-import`,
  `release/fontawesome-1.1.0`.
- Never use `claude/*`, `copilot/*`, session names, dates or random names. If your
  environment hands you a branch like that, create a correctly named branch and work there.
- Never push to `main`. All changes land through a pull request with the `validate`
  checks green. History on `main` stays linear: no merge commits.
- Merge method: **rebase** when every commit on the branch is clean and meaningful on its
  own (typical for the maintainer's PRs); **squash** for anything messy, including most
  outside contributions. Before asking for a rebase merge, tidy the branch so each commit
  passes CI and follows the commit rules below.
- The only other allowed branch prefix is `dependabot/`, which Dependabot creates itself.

## Commits and pull requests

- Commit subject in the imperative, under 72 characters ("Add stack compositing to
  fontawesome-stylist"). Explain the why in the body when it isn't obvious.
- Commit as `gtibrett <47423+gtibrett@users.noreply.github.com>` or another noreply
  address. Never commit a personal email address.
- One topic per PR. Keep unrelated cleanups out of feature PRs.
- PR title follows the commit subject rules, since a squash merge uses it as the commit. Fill in what changed and how it was checked.

## Nothing private, nothing borrowed

- No personal emails, home paths (`/Users/...`), machine usernames, internal hostnames
  or account IDs. Use `Evil Brett Designs` or `gtibrett` for authorship fields.
- No references to any employer, client or their products, code, names or features. This
  repo contains only work that is free to release under MIT.
- No secrets, ever. Plugins must read credentials from environment variables and must
  write config that interpolates them (`${FONTAWESOME_PACKAGE_TOKEN}`), never literal
  values. Instructions that check a secret must not print it.
- Don't commit third-party paid or licensed assets (Font Awesome Pro SVGs, fonts, kit
  code). Link to the vendor instead.

## Repo layout

```
.claude-plugin/marketplace.json   catalog of every plugin in the repo
plugins/<name>/.claude-plugin/plugin.json
plugins/<name>/README.md          what it does, install, required env vars
plugins/<name>/skills|agents|commands|hooks/
```

## Changing a plugin

- Bump `version` in `plugins/<name>/.claude-plugin/plugin.json` on every user-visible
  change, using semver: patch for fixes and wording, minor for new skills, agents or
  commands, major for removals or behaviour that breaks existing use.
- Keep `marketplace.json` in sync: every plugin directory has an entry, the entry's
  `name` matches `plugin.json`, `source` points at the directory, and the descriptions
  say the same thing.
- A new plugin needs a README that lists its required environment variables and any
  network calls it makes.
- Keep `allowed-tools` in skills and agents as narrow as the task needs, for example
  `Bash(curl:*)` rather than `Bash`. Agents that only read must not get write tools.
- Hooks and scripts run on users' machines. Keep them small, readable and
  dependency-free, and never download and execute code.
- Release tags are `<plugin>@<version>` (for example `fontawesome@1.1.0`).

## Before opening a PR

Run the same checks CI runs:

```sh
for f in .claude-plugin/*.json plugins/*/.claude-plugin/*.json; do jq empty "$f" || echo "bad: $f"; done
claude plugin validate .   # if the Claude Code CLI is available
```

Then re-read the diff for anything in the "Nothing private" list above.

## Security

Vulnerabilities go through private reporting, see [SECURITY.md](SECURITY.md). Rulesets
and repo settings are tracked in [.github/rulesets/](.github/rulesets/README.md); change
them there and in GitHub together.
