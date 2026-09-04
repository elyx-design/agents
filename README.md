# Elyx for External Agents

This repository houses the Elyx skill for use with the Claude and Codex plugin
systems, as well as other agents that support the
[Agent Skills](https://github.com/vercel-labs/skills) format.

The skill teaches an agent how to inspect, author, validate and render `.elyx`
files. It uses the public
[`@elyx-design/cli`](https://www.npmjs.com/package/@elyx-design/cli) package as
its runtime.

## Integrations

### ChatGPT/Codex

The Elyx skill can be installed as a
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

### Others

The Elyx skill can also be installed through the third-party
[Agent Skills](https://github.com/vercel-labs/skills) CLI:

```sh
npx skills add elyx-design/agents --skill elyx
```

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

The Elyx skill teaches agents how to use `elyx man`, `elyx diagnostics`, `elyx
inspect`, `elyx format`, and `elyx render`. Installing the skill provides agent
guidance; installing `@elyx-design/cli` provides the binary that guidance
uses.

The skill deliberately avoids project-specific conventions. Projects should
provide those through their own `README.md`, `AGENTS.md`, local skills, or user
prompts.
