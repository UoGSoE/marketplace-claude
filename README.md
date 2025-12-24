Claude code plugin to add some helpful coding assistant skills, commands and hooks that follow our conventions.

## Installation

1. Run `claude`, then :
- `/plugin` -> Marketplaces -> Add marketplace -> `https://github.com/UoGSoE/marketplace-claude`

Make sure to enable the plugin - then restart Claude.

Now you should have varous `/teamdev:` commands available and if you run `/skills` there will be some teamdev skills listed.

**Note:** The plugin includes a hook that stops claude doing some of it's "bad habits".  It assumes the path is `~/Documents/code/agent-edit-checker/check.php`.  The repo for that script is at `https://github.com/ohnotnow/agent-edit-checker`.  If you don't want to use it, or have it in a different path, edit the json file in your `~/.claude/plugins/marketplaces/teamdev-plugins/plugins/agent-skills/hooks/` directory.

