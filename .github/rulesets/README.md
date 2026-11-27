# Repository rulesets and settings

GitHub does not read these files automatically. They are the source of truth for the
repo's rulesets so changes get reviewed like code. To apply one, go to
**Settings → Rules → Rulesets → New ruleset → Import a ruleset** and pick the file.
Delete any older ruleset that targets all branches first.

| File | Targets | What it does |
| --- | --- | --- |
| `protect-main.json` | default branch only | Applies to everyone, you included, with no bypass: no deletion or force-push, linear history, changes only through a squash-merged PR with resolved conversations, and the `manifests` and `branch-name` checks must pass. |
| `require-review.json` | default branch only | Requires 1 approving review from a code owner (you), re-approval after new pushes. The Repository admin role bypasses it in "pull requests only" mode, so you can merge your own PRs without an approval. Everyone else needs yours. |
| `branch-names.json` | every branch except `main` and the allowed prefixes | Blocks creating any branch not named `bug/`, `chore/`, `feature/`, `hotfix/`, `release/` or `dependabot/`. No bypass, so it applies to admins and agents alike. Existing branches aren't affected. |
| `protect-tags.json` | `v*` and `plugin@version` tags | Release tags can't be moved or deleted (admins can, if a release has to be pulled). |

Branches under the allowed prefixes have no other rules, so contributors and agents can
push freely to their own PR branches. Only `main` is guarded against direct pushes. The
`branch-name` CI job checks the same pattern on every PR, which also covers PRs from forks,
where rulesets on this repo don't apply.

**Approvals: everyone but you.** The rules are split in two on purpose. A bypass skips
every rule in its ruleset, so if the admin bypass lived in `protect-main` you could also
merge with red CI. With the split, the bypass in `require-review` only skips the approval,
and `protect-main` still forces a PR and green checks on you too. Anyone else (forks,
future collaborators, bots) needs your approval as code owner, and pushing after your
approval dismisses it. Only you hold the admin role, so keep it that way: anyone you give
admin also gets the bypass.

Before importing, check that nothing live still requires an approval, or you'll be locked
out of merging:
- Settings → Rules → Rulesets: delete the old all-branches ruleset. Any other ruleset on
  `main` that requires approvals must also list Repository admin as a bypass in "pull
  requests only" mode.
- Settings → Branches: if a classic branch protection rule exists for `main`, delete it
  (the ruleset replaces it) or untick "Require approvals" there.

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
