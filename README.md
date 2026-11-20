# claude-marketplace

Claude Code plugin marketplace.

## Usage

```
/plugin marketplace add evilbrettdesigns/claude-marketplace
/plugin install example-plugin@claude-marketplace
```

## Layout

```
.claude-plugin/marketplace.json   # marketplace catalog
plugins/<plugin-name>/            # one directory per plugin
  .claude-plugin/plugin.json
  commands/ agents/ skills/ hooks/
```

## Adding a plugin

1. Create `plugins/<name>/.claude-plugin/plugin.json`.
2. Add an entry to `plugins[]` in `.claude-plugin/marketplace.json` with `"source": "./plugins/<name>"`.
