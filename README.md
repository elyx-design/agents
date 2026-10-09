# Elyx for External Agents

This repository houses Elyx skills for use with the Claude, Codex and Cursor
plugin systems, as well as other agents that support the
[Agent Skills](https://github.com/vercel-labs/skills) format.

The base `elyx` skill teaches an agent how to inspect, author, validate and render
`.elyx` files. The `elyx-code` skill adds guidance for translating between Elyx
designs and web code. Both use the public
[`@elyx-design/cli`](https://www.npmjs.com/package/@elyx-design/cli) package as
their runtime.

## Integrations

### ChatGPT/Codex

The Elyx skills can be installed as a
[plugin](https://learn.chatgpt.com/docs/plugins?surface=app) through the desktop
app or from the command line:

```sh
codex plugin marketplace add elyx-design/agents
codex plugin add elyx@elyx
```

### Claude Code

```sh
claude plugin marketplace add elyx-design/agents
claude plugin install elyx@elyx
```

### Cursor

In Cursor, open **Customize**, choose **From GitHub Repository**, and import
`https://github.com/elyx-design/agents`. Install the Elyx plugin from the imported
marketplace, choosing a project or user scope.

The plugin bundles the same skills as the Claude and Codex integrations. Install the
Elyx CLI separately using the [CLI instructions](#cli) below.

See the [Cursor plugin documentation](https://cursor.com/docs/plugins) for more.

### Others

The Elyx skills can also be installed through the third-party
[Agent Skills](https://github.com/vercel-labs/skills) CLI:

```sh
npx skills add elyx-design/agents --skill '*'
```

This selects every skill in the repository, including both `elyx` and
`elyx-code`, while the installer lets you choose the target agent and scope.
For just the base skill, use `--skill elyx`. Install `elyx` alongside
`elyx-code`, since the translation skill relies on the base Elyx guidance.

> The installer detects supported agents and provides an installation wizard.
> See the [Agent Skills](https://github.com/vercel-labs/skills) documentation
> for more.

## CLI

Install the CLI globally with npm or another npm registry-compatible package
manager. This makes `elyx` available to your agent and for direct use:

```sh
npm install --global @elyx-design/cli
elyx --help
```

## How it works

The base `elyx` skill teaches agents how to use `elyx man`, `elyx diagnostics`,
`elyx inspect`, `elyx normalize`, and `elyx render`. The `elyx-code`
skill adds translation guidance. Installing the skills provides agent guidance;
installing `@elyx-design/cli` provides the binary that guidance uses.

The skills deliberately avoid project-specific conventions. Projects should
provide those through their own `README.md`, `AGENTS.md`, local skills, or user
prompts.
