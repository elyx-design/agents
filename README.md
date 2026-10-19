# Elyx for external agents

This repository holds the Elyx skills for the Claude Code, Codex, and Cursor
plugin systems, and for other agents that support the
[Agent Skills](https://github.com/vercel-labs/skills) format.

The base `elyx` skill teaches an agent how to inspect, author, validate, and
render `.elyx` files. The `elyx-code` skill adds guidance for translating
between Elyx designs and web code. Both skills have the agent run the `elyx`
command from the public
[`@elyx-design/cli`](https://www.npmjs.com/package/@elyx-design/cli) package.
Every integration needs the CLI, so install it first.

## Install the Elyx CLI

Install the CLI globally with npm or another package manager that uses the npm
registry. The `elyx` command is then available to your agent and to you:

```sh
npm install --global @elyx-design/cli
elyx --help
```

## Integrations

### Codex

Codex installs the Elyx plugin from npm, as
[`@elyx-design/codex-plugin`](https://www.npmjs.com/package/@elyx-design/codex-plugin),
and runs its canvas with `node`. Before you install the plugin, make sure that
both `npm` and [Node.js](https://nodejs.org) 22 or later are on your login
shell's `PATH`.

Install the Elyx [plugin](https://learn.chatgpt.com/docs/plugins?surface=app)
through the desktop app:

1. Go the Plugins Tab
2. Click Add > Add a marketplace
3. Enter `elyx-design/agents` in the first field, leave the rest blank
4. Then a "Personal" tab should appear from which you can install the Elyx plugin

or from the command line:

```sh
codex plugin marketplace add elyx-design/agents
codex plugin add elyx@elyx
```

After you install the plugin, restart Codex.

The Codex plugin also opens `.elyx` files as an Elyx canvas in the desktop app.
To use it, open a `.elyx` file, open its **Open** menu, and choose **Elyx
canvas** under **Open in ChatGPT**.

### Claude Code

```sh
claude plugin marketplace add elyx-design/agents
claude plugin install elyx@elyx
```

### Cursor

In Cursor, open **Customize**, choose **From GitHub Repository**, and import
`https://github.com/elyx-design/agents`. Install the Elyx plugin from the
imported marketplace, and choose a project or user scope.

The plugin contains the same skills as the Claude Code and Codex plugins. See
the [Cursor plugin documentation](https://cursor.com/docs/plugins) for more.

### Other agents

Install the Elyx skills with the third-party
[Agent Skills](https://github.com/vercel-labs/skills) CLI. It detects the agents
on your machine and asks which agent and scope to install into:

```sh
npx skills add elyx-design/agents --skill '*'
```

`--skill '*'` installs both `elyx` and `elyx-code`. To install only the base
skill, use `--skill elyx`. The `elyx-code` skill relies on the base Elyx
guidance, so don't install it without `elyx`.

## How it works

The base `elyx` skill teaches agents how to use `elyx man`, `elyx diagnostics`,
`elyx find`, `elyx tree`, `elyx normalize`, and `elyx render`. The `elyx-code`
skill adds translation guidance. The skills give your agent the guidance, and
`@elyx-design/cli` gives it the command that the guidance uses.

The skills leave out project-specific conventions. Give your agent those in your
project's `README.md`, `AGENTS.md`, local skills, or prompts.
