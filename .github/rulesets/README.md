# Repository rulesets and settings

GitHub does not read these files automatically. They are the source of truth for the
repo's rulesets so changes get reviewed like code. To apply one, go to
**Settings → Rules → Rulesets → New ruleset → Import a ruleset** and pick the file.
Delete any older ruleset that targets all branches first.

| File | Targets | What it does |
| --- | --- | --- |
| `protect-main.json` | default branch only | No deletion or force-push, linear history, changes only through a squash-merged PR with resolved conversations, and the `manifests` and `branch-name` checks must pass. Admins can bypass only by merging a PR. |
| `branch-names.json` | every branch except `main` and the allowed prefixes | Blocks creating any branch not named `bug/`, `chore/`, `feature/`, `hotfix/`, `release/` or `dependabot/`. No bypass, so it applies to admins and agents alike. Existing branches aren't affected. |
| `protect-tags.json` | `v*` and `plugin@version` tags | Release tags can't be moved or deleted (admins can, if a release has to be pulled). |

Branches under the allowed prefixes have no other rules, so contributors and agents can
push freely to their own PR branches. Only `main` is guarded against direct pushes. The
`branch-name` CI job checks the same pattern on every PR, which also covers PRs from forks,
where rulesets on this repo don't apply.

No approving review is required because there is one maintainer and GitHub never lets you
approve your own PR. If a second maintainer joins, raise
`required_approving_review_count` to 1 and turn on `require_code_owner_review`.

## Settings to set by hand

These live outside the repo, so they're listed here to be checked off before (or right
after) the repo goes public.

**General**
- Pull Requests: allow squash merging only; turn on "Always suggest updating pull request
  branches" and "Automatically delete head branches".
- Features: turn off Wiki and Projects unless you use them. Keep Issues on.

**Code security** (all free on public repos)
- Private vulnerability reporting: on (SECURITY.md points people here).
- Dependency graph, Dependabot alerts, Dependabot security updates: on.
- Secret scanning and push protection: on. Also turn on "Scan for non-provider patterns"
  if offered.
- Code scanning: skip for now. There's no executable code to analyze yet; revisit when a
  plugin ships hooks or scripts.

**Actions → General**
- Actions permissions: "Allow evilbrettdesigns, and select non-evilbrettdesigns, actions" with
  "Allow actions created by GitHub" checked. Turn on "Require actions to be pinned to a
  full-length commit SHA".
- Fork pull request workflows from outside collaborators: "Require approval for all
  external contributors".
- Workflow permissions: "Read repository contents and packages permissions", and leave
  "Allow GitHub Actions to create and approve pull requests" off.

**Collaborators and apps**
- Keep only `gtibrett` as admin. Review installed GitHub Apps (Claude, Copilot) and scope
  them to this repo only.

**Before flipping to public**
- Close or merge stale PRs (the Copilot scaffold PR with `example-plugin`).
- Delete merged or abandoned branches.
