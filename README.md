# BPMN Modeling Light

A Claude Code skill that creates and modifies BPMN process models
in Markdown table format based on the BPMN Database Schema v3.0.

## What it does

This skill enables Claude Code to generate complete, schema-compliant
BPMN models as structured Markdown tables. It supports:

- **New model creation** from natural language process descriptions
- **Model modification** of existing BPMN Markdown models
- Full referential integrity (PK/FK validation)
- BPMN 2.0 conformance checks

## Usage

Install as a Claude Code skill by adding the path to this repository
in your Claude Code configuration.

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
