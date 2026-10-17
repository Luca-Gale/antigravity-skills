# Antigravity Custom Skills Repository

This repository tracks custom [Google Antigravity](https://antigravity.google/docs/skills) skills across machines and workspaces.

## Included Skills

| Skill | Slash Command | Description |
| :--- | :--- | :--- |
| **[redteam](./redteam/SKILL.md)** | `/redteam` | Adversarial architectural validation and ideation workflow (Blue Team Architect vs. Red Team Skeptic live 2-round debate, Cross-Examination Chamber, and `TECH_SPEC.md` / `RED_TEAM_AUDIT.md` generation). |

## How It Is Linked Globally

Antigravity automatically discovers all skills in this repository via the global manifest at `~/.gemini/config/skills.json`:

```json
{
  "entries": [
    {
      "path": "~/.gemini/antigravity/scratch/antigravity-skills"
    }
  ]
}
```

## Adding a New Skill

Create a new subfolder `<skill-name>/SKILL.md` at the root of this repository with standard YAML frontmatter (`name` and `description`). Antigravity scans this directory one level deep and registers new skills automatically.
