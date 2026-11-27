# claude-marketplace

Open source Claude Code plugins from Evil Brett Designs, released under the MIT License.

## Install

```bash
/plugin marketplace add evilbrettdesigns/claude-marketplace
/plugin install fontawesome@evilbrettdesigns
```

Or add it to a project's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "evilbrettdesigns": {
      "source": {
        "source": "github",
        "repo": "evilbrettdesigns/claude-marketplace"
      }
    }
  }
}
```

## Plugins

| Plugin | Description |
|--------|-------------|
| [**fontawesome**](plugins/fontawesome) | FontAwesome toolkit for React: live icon search, install (Free/Pro Kit/Pro npm), styling, stack/mask/layer compositing, tree-shake subsetting and best-practice audits. |

## Adding a plugin

Each plugin lives in `plugins/<name>/` with a `.claude-plugin/plugin.json` manifest and at least one skill or command. Add an entry for it to `.claude-plugin/marketplace.json`.
