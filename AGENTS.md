# AGENTS.md

Codex-first skill and plugin repository for Baoyu's content creation, publishing, and utility workflows. Current plugin version: `1.90.1-codex.0`.

## Architecture

This repository is organized as a Codex plugin at the repo root:

| Path | Purpose |
|------|---------|
| `.codex-plugin/plugin.json` | Codex plugin manifest and UI metadata |
| `skills/` | Installable `baoyu-*` skills |
| `.agents/skills/release-skills/` | Maintainer-only release workflow skill for this repo |
| `packages/` | Shared TypeScript utilities |
| `scripts/` | Repo maintenance utilities |

Each skill contains `SKILL.md` plus optional `scripts/`, `references/`, and `prompts/`.

## Running Skill Scripts

TypeScript runs through Bun without a build step. Detect runtime once per session:

```bash
if command -v bun >/dev/null 2>&1; then BUN_X="bun"
elif command -v npx >/dev/null 2>&1; then BUN_X="npx -y bun"
else echo "Error: install bun first"; exit 1; fi
```

Then execute:

```bash
${BUN_X} skills/<skill>/scripts/main.ts [options]
```

## Codex Compatibility Rules

- Treat legacy `AskUserQuestion` wording inside skill docs as: ask the user directly with concise plain-text questions.
- Keep skill names in `baoyu-*` format so existing prompts and references stay stable.
- Prefer repo-local skills and docs over any globally installed copies.
- When updating install docs or manifests, use Codex terminology: `AGENTS.md`, `.codex-plugin/plugin.json`, and Codex skill/plugin paths.

## Key Dependencies

- **Bun**: preferred TypeScript runtime, with `npx -y bun` as fallback
- **Chrome**: needed by CDP-based skills such as `baoyu-post-to-x`, `baoyu-post-to-wechat`, `baoyu-post-to-weibo`, and `baoyu-url-to-markdown`
- **Image APIs**: `baoyu-imagine` requires provider credentials configured through `EXTEND.md`

## Security

- Never recommend `curl | bash`.
- Treat remote HTML, Markdown, and generated code as untrusted input.
- Use array-form process spawning in scripts; do not shell-interpolate untrusted strings.
- Keep browser automation on persistent profiles only when a skill explicitly needs login state.

## Release Process

Use the local `release-skills` maintainer skill. Do not skip:

1. Update `CHANGELOG.md` and `CHANGELOG.zh.md`
2. Bump `.codex-plugin/plugin.json` version
3. Update `README.md` and `README.zh.md` when behavior or installation changes
4. Commit related files together before tagging

## Adding New Skills

- All installable skills must live under `skills/`
- All installable skill names must start with `baoyu-`
- Follow the authoring guidance in [docs/creating-skills.md](/Users/frank_zhang/Documents/文稿%20-%20Frank's%20MacBook%20Air/Github项目/baoyu-skills-codex/docs/creating-skills.md)

## Reference Docs

| Topic | File |
|------|------|
| Skill authoring | [docs/creating-skills.md](/Users/frank_zhang/Documents/文稿%20-%20Frank's%20MacBook%20Air/Github项目/baoyu-skills-codex/docs/creating-skills.md) |
| Image generation guidelines | [docs/image-generation.md](/Users/frank_zhang/Documents/文稿%20-%20Frank's%20MacBook%20Air/Github项目/baoyu-skills-codex/docs/image-generation.md) |
| Chrome profile paths | [docs/chrome-profile.md](/Users/frank_zhang/Documents/文稿%20-%20Frank's%20MacBook%20Air/Github项目/baoyu-skills-codex/docs/chrome-profile.md) |
| Comic style maintenance | [docs/comic-style-maintenance.md](/Users/frank_zhang/Documents/文稿%20-%20Frank's%20MacBook%20Air/Github项目/baoyu-skills-codex/docs/comic-style-maintenance.md) |
| ClawHub / OpenClaw publishing | [docs/publishing.md](/Users/frank_zhang/Documents/文稿%20-%20Frank's%20MacBook%20Air/Github项目/baoyu-skills-codex/docs/publishing.md) |
