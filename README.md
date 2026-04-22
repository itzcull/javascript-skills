# Frontend Agent Skills

This repository is a scaffold for an [Agent Skills](https://agentskills.io/) library focused on frontend development.

It does not ship any concrete skills yet. Instead, it gives you a clean structure for adding them over time.

## Repo Layout

```text
.
├── README.md
├── LICENSE
├── docs/
│   └── authoring.md
├── templates/
│   └── skill-template/
│       └── SKILL.md
└── skills/
```

## Add a New Skill

1. Create a new directory inside `skills/` using a lowercase kebab-case name.
2. Copy `templates/skill-template/SKILL.md` into that directory.
3. Update the `name` field so it exactly matches the directory name.
4. Replace the placeholder description with a specific trigger-oriented description.
5. Fill in the instructions with the workflow you want agents to follow.
6. Add optional `references/`, `scripts/`, or `assets/` folders only when needed.

Example:

```text
skills/
└── your-skill-name/
    ├── SKILL.md
    ├── references/
    ├── scripts/
    └── assets/
```

## Authoring Rules

- Keep skill names lowercase and hyphenated.
- Keep `SKILL.md` focused and practical.
- Put long reference material in `references/` instead of bloating `SKILL.md`.
- Write descriptions so an agent can tell when to activate the skill.
- Prefer reusable workflows over product-specific prompts.

See `docs/authoring.md` for a fuller checklist.

## Validation

The Agent Skills spec recommends validating each skill with `skills-ref`:

```bash
skills-ref validate ./skills/your-skill-name
```

Spec reference: https://agentskills.io/specification
