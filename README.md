# BPMN Modeling Light

An agent skill that creates and modifies BPMN process models
in Markdown table format based on the BPMN Database Schema v3.0.

This skill follows the open [Agent Skills](https://agentskills.io) standard
and works across multiple AI platforms.

## What it does

This skill enables AI agents to generate complete, schema-compliant
BPMN models as structured Markdown tables. It supports:

- **New model creation** from natural language process descriptions
- **Model modification** of existing BPMN Markdown models
- Full referential integrity (PK/FK validation)
- BPMN 2.0 conformance checks

## Installation

Skills are installed by cloning the repository into the directory where your AI agent looks for skills. There is no separate install command — placing the files in the right location is all it takes. The agent automatically discovers any `SKILL.md` file in its skills directory.

### Claude Code

From your project root, clone the skill into the skills directory:

```bash
# Project-level (shared via git)
git clone https://github.com/hennig-ai/bpmn-modeling-light.git .claude/skills/bpmn-modeling-light

# Personal (available across all projects)
git clone https://github.com/hennig-ai/bpmn-modeling-light.git ~/.claude/skills/bpmn-modeling-light
```

### Claude.ai (Cowork)

Upload the skill via the Claude.ai interface. See [Using skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude) for details.

### OpenAI Codex CLI

```bash
git clone https://github.com/hennig-ai/bpmn-modeling-light.git .agents/skills/bpmn-modeling-light
```

### GitHub Copilot

```bash
git clone https://github.com/hennig-ai/bpmn-modeling-light.git .github/skills/bpmn-modeling-light
```

### Claude.ai Projects (alternative)

You can also use this skill manually via Claude.ai Projects. However, this requires adjustments:

1. Paste the content of `SKILL.md` into the **Project Prompt** (system instructions)
2. Upload the four files from `references/` as **Project Knowledge**
3. **Remove or adapt the "Loading Strategy" section** from the prompt — it references file paths that don't apply in a Projects context, since all knowledge files are already loaded into the conversation

Note that in Projects, all reference files are always in context (no lazy loading), which uses more of the context window.

### Other platforms

Any AI agent that supports the [Agent Skills standard](https://agentskills.io) can use this skill. Clone the repository into the platform's skills directory.

> **Note:** This skill has only been tested on Claude Code and Claude.ai. It should work on any platform that supports the Agent Skills standard, but is not guaranteed.

## Usage

Trigger the skill with BPMN-specific requests, e.g.:
- "Create a BPMN model for an order process"
- "Add an Exclusive Gateway to the model"
- "Modify the existing BPMN model"

## Repository Structure

| File | Purpose |
|------|---------|
| `SKILL.md` | Skill definition and rules |
| `references/bpmn-schema.md` | Full schema documentation |
| `references/bpmn-hierarchy.md` | BPMN element type hierarchy |
| `references/bpmn-instance-example.md` | Reference instance with table format |
| `references/event-detail-tables.md` | Event definition tables |

## License

MIT — see [LICENSE](LICENSE) for details.
