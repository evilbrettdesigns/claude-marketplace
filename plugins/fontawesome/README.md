# FontAwesome Plugin

End-to-end FontAwesome automation for React projects. One orchestrator skill routes work to six specialized subagents (haiku/sonnet) so main-thread context stays lean.

## What it does

- **Find icons** by intent ("a download arrow", "social: github") via live FontAwesome GraphQL API.
- **Install** FA into any React project across all three tiers (Free, Pro Kit, Pro npm) — package manager + framework auto-detected, `.npmrc` written with safe env interpolation.
- **Style/transform** single icons (size, rotate, flip, animate, duotone, power-transform basics).
- **Compose** icons via stack / mask / layer / power-transform (SVG+JS features).
- **Subset** the bundle via tree-shake — audit imports, flag wildcards, optionally rewrite to per-icon imports.
- **Audit** the project against FontAwesome best practices (tree-shake, SSR pinning, accessibility, version pinning, token security).

## Components

### Skill

| Skill | Trigger | Routes to |
|-------|---------|-----------|
| `fontawesome` | "find an icon", "install FontAwesome", "style icon", "stack icons", "shrink FA bundle", "FA best practices" | one of the agents below |

### Agents

| Agent | Job | Model |
|-------|-----|-------|
| `fontawesome-icon-finder` | Live GraphQL search → ranked icon table | haiku |
| `fontawesome-installer` | Add FA to React project end-to-end | sonnet |
| `fontawesome-stylist` | Sizing, rotate, animate, duotone JSX | haiku |
| `fontawesome-compositor` | Stack/mask/layer/power-transform JSX | sonnet |
| `fontawesome-subsetter` | Tree-shake audit + import rewrites | sonnet |
| `fontawesome-best-practices` | Tailored 🔴🟡🟢 checklist | haiku |

## Tier handling

| Tier | Auth needed |
|------|-------------|
| **Free** | none |
| **Pro Kit** | kit code + `FONTAWESOME_PACKAGE_TOKEN` (npm install of `@awesome.me/kit-<CODE>`) |
| **Pro npm** | `FONTAWESOME_PACKAGE_TOKEN` (per-style `@fortawesome/pro-*-svg-icons`) |

Optional: `FONTAWESOME_API_TOKEN` — Bearer auth for GraphQL. Only needed for kit/account queries; icon search is public.

`.npmrc` template (env interpolation, never literal token):

```
@awesome.me:registry=https://npm.fontawesome.com/
@fortawesome:registry=https://npm.fontawesome.com/
//npm.fontawesome.com/:_authToken=${FONTAWESOME_PACKAGE_TOKEN}
```

## Subset strategy

This plugin uses **tree-shake via per-icon imports**, not the FA hosted Kit subsetter. Reason: works for Free + Pro npm, no build tool config required, plays well with Vite/Next.js/CRA out of the box.

```tsx
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faCoffee } from '@fortawesome/free-solid-svg-icons';

<FontAwesomeIcon icon={faCoffee} />
```

For icons used in 3+ places, the installer/subsetter optionally generates a `src/lib/icons.ts` barrel with `library.add(...)`.

## Install

Plugin lives in this marketplace. From a Claude Code project:

```bash
/plugin install fontawesome@evilbrettdesigns
```

## Use

Trigger phrases:
- "Find me a download icon"
- "Set up FontAwesome Free in this Vite project"
- "Use FontAwesome Pro Kit, kit code abc12def34"
- "How do I make this icon spin"
- "Stack a checkmark over a green circle"
- "Audit my FontAwesome usage"
- "Shrink my FA bundle"

## Hard rules baked in

- Never writes literal tokens to `.npmrc` — always `${FONTAWESOME_PACKAGE_TOKEN}` interpolation.
- Never installs Pro packages without confirming `FONTAWESOME_PACKAGE_TOKEN` is set.
- Stack/mask/layer require SVG+JS — agent refuses if project is webfont-only.
- All icon names derived from live GraphQL — never invented.

## References

- FontAwesome docs: https://docs.fontawesome.com/
- React integration: https://docs.fontawesome.com/web/use-with/react
- Packages: https://docs.fontawesome.com/web/setup/packages
- GraphQL API: https://docs.fontawesome.com/apis/graphql (endpoint: `https://api.fontawesome.com`)
