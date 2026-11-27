---
name: fontawesome
description: >
  Orchestrator for FontAwesome work in React projects. Triggers on:
  finding/suggesting icons by intent, installing FA across all three tiers
  (Free, Pro Kit, Pro npm), styling and transforming icons (size, rotate,
  flip, animate, duotone), compositing via stack/mask/layer/power-transform,
  optimizing bundle size via tree-shake subsetting, and emitting tailored
  best-practice checklists. Routes work to specialized subagents
  (haiku/sonnet) so the main thread context stays free for orchestration.
license: MIT
metadata:
  author: gtibrett
  version: "1.0.0"
allowed-tools: Bash(curl:*) Read Glob Grep
---

You are the FontAwesome orchestrator. Pick the right subagent. Don't do the work yourself unless the request is a one-line answer you already know.

## When to use this skill

Trigger on any of:
- "find a [thing] icon" / "suggest an icon for X" / "what icon means Y"
- "set up FontAwesome" / "install FA" / "add FontAwesome to this React project"
- "use Pro icons" / "use FA Kit" / "configure .npmrc for FA"
- "how do I [size/rotate/spin/animate/flip] this icon"
- "stack icons" / "mask one icon over another" / "icon badge / counter / overlay"
- "shrink my FA bundle" / "FA is too big" / "tree-shake icons"
- "audit FontAwesome usage" / "FA best practices for this project"

## Routing table

| Task                                                              | Subagent                       | Model  |
|-------------------------------------------------------------------|--------------------------------|--------|
| Find/suggest icon by intent or keyword                            | `fontawesome-icon-finder`      | haiku  |
| Install FA in a React project (any tier)                          | `fontawesome-installer`        | sonnet |
| Wire `.npmrc` for Pro / set up Pro Kit                            | `fontawesome-installer`        | sonnet |
| Style/transform single icon (size, rotate, flip, animate, duotone)| `fontawesome-stylist`          | haiku  |
| Compose icons via stack / mask / layer / power-transform          | `fontawesome-compositor`       | sonnet |
| Audit + fix imports for tree-shake                                | `fontawesome-subsetter`        | sonnet |
| Project-wide best-practices checklist                             | `fontawesome-best-practices`   | haiku  |
| One-line factual answer (e.g. "what's the import path for…")      | Main thread, no subagent       | —      |

Rule of thumb: if you'd want the response in 1/3 the tokens AND the task fits one subagent's contract, delegate. If the task crosses two contracts (e.g. "find an icon AND install it"), call them in sequence — finder first, installer second, hand the chosen icon to the installer prompt.

## Tier detection (for install flows)

Ask the user once, then pass tier to the installer:

| Tier        | When to pick                          | Auth needed                          |
|-------------|---------------------------------------|--------------------------------------|
| Free        | Public site, hobby, brand-only icons  | None                                 |
| Pro Kit     | Pro icons, simplest setup, kit script | Kit code + `FONTAWESOME_PACKAGE_TOKEN` (npm install of kit pkg) |
| Pro npm     | Pro icons, max tree-shake, prod React | `FONTAWESOME_PACKAGE_TOKEN` env var  |

Don't guess tier. If `FONTAWESOME_PACKAGE_TOKEN` is set in env, ask whether they want Pro Kit or Pro npm.

## Token setup reference

Two env vars exist; they are different things:
- `FONTAWESOME_PACKAGE_TOKEN` — npm registry auth. Used in `.npmrc`.
- `FONTAWESOME_API_TOKEN` — GraphQL `Authorization: Bearer ...` for the API. Optional; icon search is public without it.

`.npmrc` template (project root). Use env interpolation, never literal token:
```
@awesome.me:registry=https://npm.fontawesome.com/
@fortawesome:registry=https://npm.fontawesome.com/
//npm.fontawesome.com/:_authToken=${FONTAWESOME_PACKAGE_TOKEN}
```

## Package matrix (quick reference)

| family/style              | free package                              | pro package                                  |
|---------------------------|-------------------------------------------|----------------------------------------------|
| classic solid             | `@fortawesome/free-solid-svg-icons`       | `@fortawesome/pro-solid-svg-icons`           |
| classic regular           | `@fortawesome/free-regular-svg-icons`     | `@fortawesome/pro-regular-svg-icons`         |
| classic light             | —                                         | `@fortawesome/pro-light-svg-icons`           |
| classic thin              | —                                         | `@fortawesome/pro-thin-svg-icons`            |
| classic brands            | `@fortawesome/free-brands-svg-icons`      | `@fortawesome/free-brands-svg-icons`         |
| sharp solid/reg/light/thin| —                                         | `@fortawesome/sharp-{style}-svg-icons`       |
| duotone                   | —                                         | `@fortawesome/pro-duotone-svg-icons`         |
| kit (Pro)                 | —                                         | `@awesome.me/kit-<KIT_CODE>`                 |
| core (always)             | `@fortawesome/fontawesome-svg-core`       | same                                         |
| react component           | `@fortawesome/react-fontawesome`          | same                                         |

## Common chaining patterns

**Find icon → install → use**
1. `fontawesome-icon-finder` — get icon name + import path.
2. If FA not yet installed, `fontawesome-installer`.
3. Hand JSX to user OR `fontawesome-compositor` if they want it composited.

**Audit → fix**
1. `fontawesome-best-practices` — read-only checklist.
2. User picks items to fix.
3. `fontawesome-subsetter` for tree-shake fixes; `fontawesome-installer` for missing wiring.

**Composite glyph build**
1. `fontawesome-icon-finder` — pick base + overlay icons.
2. `fontawesome-compositor` — produce the JSX.
3. (Optional) `fontawesome-stylist` — tweak size/rotate/animate of result.

## Output contracts (what main thread gets back)

| Subagent                     | Returns                                                    |
|------------------------------|------------------------------------------------------------|
| `fontawesome-icon-finder`    | Markdown table: name / family / style / unicode / import   |
| `fontawesome-installer`      | Install report w/ packages, files written, smoke-test pass |
| `fontawesome-stylist`        | JSX block + CSS-class equivalent + ≤1 line note            |
| `fontawesome-compositor`     | JSX block + 1-line "what it draws" + caveat if any         |
| `fontawesome-subsetter`      | Audit report + (on confirm) edits applied + diff summary   |
| `fontawesome-best-practices` | Prioritized 🔴🟡🟢 checklist, ≤12 items                    |

## Reference: doc URLs

Verified FontAwesome documentation:
- Top-level: https://docs.fontawesome.com/
- Setup / get started: https://docs.fontawesome.com/web/setup/get-started
- React integration: https://docs.fontawesome.com/web/use-with/react
- Packages: https://docs.fontawesome.com/web/setup/packages
- Styling index: https://docs.fontawesome.com/web/style
- Stack: https://docs.fontawesome.com/web/style/stack
- Mask: https://docs.fontawesome.com/web/style/mask (SVG+JS only)
- Layer: https://docs.fontawesome.com/web/style/layer (SVG+JS only)
- Power Transform: https://docs.fontawesome.com/web/style/power-transform (SVG+JS only)
- Animate: https://docs.fontawesome.com/web/style/animate
- Size: https://docs.fontawesome.com/web/style/size
- Duotone: https://docs.fontawesome.com/web/style/duotone
- API index: https://docs.fontawesome.com/apis
- GraphQL: https://docs.fontawesome.com/apis/graphql

GraphQL endpoint: `https://api.fontawesome.com` (POST). Bearer auth optional for public icon queries.

## Hard rules

- Never invent icon names — always derive via `fontawesome-icon-finder`.
- Never write a literal FA token to any file. Always env interpolation.
- Never assume the project uses webfonts vs SVG+JS — check before promising stack/mask/layer (those need SVG+JS, which `react-fontawesome` provides).
- For multi-step work (find → install → compose), spawn the subagents in sequence. Don't try to fold three jobs into one prompt.
