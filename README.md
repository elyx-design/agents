# Elyx for Agents

Agent guidance for working with Elyx design files. This repository packages the
same Elyx skill for the official Claude Code and Codex plugin systems, as well
as other agents that support the open Agent Skills format.

The skill teaches an agent how to inspect, author, validate, and render `.elyx`
files. It uses the public
[`@elyx-design/cli`](https://www.npmjs.com/package/@elyx-design/cli) package as
its runtime.

## Claude Code

```sh
claude plugin marketplace add elyx-design/agents
claude plugin install elyx@elyx
```

## Codex

```sh
codex plugin marketplace add elyx-design/agents
codex plugin add elyx@elyx
```

## Other agents

The Elyx skill can also be installed through the third-party
[`skills`](https://github.com/vercel-labs/skills) CLI:

```sh
npx skills add elyx-design/agents --skill elyx
```

## CLI

Install the CLI globally and confirm that `elyx` is available on your `PATH`:

```sh
npm install --global @elyx-design/cli
elyx --help
```

The installer will detect supported agents or let you choose a target. See the
`skills` documentation for global and non-interactive installation options.

## How it works

The Elyx skill teaches agents how to use `elyx man`, `elyx diagnostics`, `elyx
inspect`, `elyx format`, and `elyx render`. Installing the skill provides agent
guidance; installing `@elyx-design/cli` provides the binary that guidance
uses.

The skill deliberately avoids project-specific conventions. Projects should
provide those through their own `README.md`, `AGENTS.md`, local skills, or user
prompts.

## Repository layout

```text
.agents/plugins/marketplace.json
.claude-plugin/marketplace.json
plugins/elyx/
  .claude-plugin/plugin.json
  .codex-plugin/plugin.json
  skills/elyx/SKILL.md
```

The shared public guidance lives in `plugins/elyx/skills/elyx/SKILL.md`.
