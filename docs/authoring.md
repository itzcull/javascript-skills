# Authoring Guide

This repo follows the Agent Skills format described at https://agentskills.io/specification.

## Minimum Skill Structure

Every skill must contain a `SKILL.md` file:

```text
your-skill-name/
└── SKILL.md
```

Optional directories:

```text
your-skill-name/
├── SKILL.md
├── references/
├── scripts/
└── assets/
```

## Frontmatter Rules

Required fields:

- `name`
- `description`

Optional fields:

- `license`
- `compatibility`
- `metadata`
- `allowed-tools`

## Naming Rules

- Directory names must be lowercase kebab-case.
- `name` must exactly match the directory name.
- Avoid broad names like `frontend` or `helpers`.
- Prefer task-shaped names like `x-y-z` that describe one job well.

## Description Rules

Good descriptions do two things:

1. Say what the skill does.
2. Say when the agent should use it.

Strong descriptions usually include:

- problem type
- likely user wording
- relevant technologies or contexts
- activation cues like "Use when..."

## Writing the Body

Good `SKILL.md` files are operational, not aspirational.

Include:

- purpose
- when to use
- expected inputs
- step-by-step workflow
- output format
- constraints or escalation rules
- references to local files when needed

Avoid:

- vague advice
- giant walls of text
- hidden assumptions about tools or stack
- burying critical instructions in reference files

## Progressive Disclosure

- Keep the frontmatter specific.
- Keep `SKILL.md` concise.
- Move detailed background material into `references/`.
- Add scripts only when the workflow genuinely benefits from executable helpers.

## Suggested Review Checklist

Before adding a skill, check:

- the directory name matches `name`
- the description is specific enough for activation
- the workflow is concrete and ordered
- references use relative paths
- the skill does one job clearly
- optional files are only added when they earn their cost

## Validation Example

```bash
skills-ref validate ./skills/your-skill-name
```
