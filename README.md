# Manufacturing AI Skills

**Free, open-source AI prompts and workflows built for manufacturers.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for manufacturing. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Downtime Analysis Summary | Compile downtime events into a root-cause summary with recommended countermeasures. | ~20 min/analysis |
| OEE Analysis Report | Turn raw Overall Equipment Effectiveness (OEE) data — availability, performance, and quality metrics — into a clear analysis report that identifies the biggest loss categories, highlights trends, and recommends targeted improvement actions. | ~45 min/report |
| Predictive Maintenance Report | Transform equipment sensor data, maintenance logs, and historical failure records into an actionable predictive maintenance report with risk rankings, recommended service windows, and cost-impact estimates. | ~30 min/report |
| Production Scheduling Optimizer | Take a set of production orders, machine capacities, labor availability, and material constraints and produce an improved short-horizon production schedule that balances due dates, changeover cost, and capacity utilization. | ~60 min/schedule |
| Quality Report Generator | Turn inspection data (incoming, in-process, final, and customer returns) into a structured quality report with defect Pareto, SPC trend read-outs, Cp/Cpk interpretation where applicable, and prioritized corrective actions — ready for a weekly quality review or management scorecard. | ~25 min/report |
| SOP Writer | Turn process notes, operator interviews, or tribal knowledge into a controlled, ISO-compliant Standard Operating Procedure that an operator can actually follow on the shop floor — complete with safety callouts, quality checkpoints, PPE requirements, and revision metadata. | ~45 min/SOP |
| Shift Handoff Report | Turn the outgoing shift's raw notes into a structured, scannable handoff the incoming supervisor can absorb in under 90 seconds — surfacing safety incidents, production counts vs. | ~15 min/handoff |
| Supply Chain Risk Assessment | Evaluate supplier and material risks across your supply chain and produce a structured risk report with mitigation recommendations, alternative sourcing options, and contingency triggers. | ~45 min/assessment |
| Vision Inspection Summary | Take the output of an automated computer-vision inspection system — pass/fail counts, defect classifications, confidence scores, and flagged images — and produce a structured summary that quality and operations teams can act on within the same shift. | ~25 min/report |
| CAPA Document Builder | Structure corrective and preventive action reports with investigation findings and timelines. | ~30 min/CAPA |
| Compliance Audit Prep | Scan existing SOPs, quality documents, and process records against regulatory requirements to produce an audit-readiness report with gap analysis, remediation priorities, and document update recommendations. | ~60 min/audit prep |
| Supplier Communication Drafter | Write professional emails for PO confirmations, quality issues, and delivery follow-ups. | ~10 min/email |
| Email Drafter | Turn rough notes into a professional, ready-to-send email that matches your company's voice and uses correct manufacturing terminology. | ~10 min/use |
| Meeting Summarizer | Transform raw meeting notes into a structured summary with decisions, action items, owners, and deadlines — formatted for manufacturing teams who need clarity, not fluff. | ~10 min/use |
| Review Responder | Craft professional, personalized responses to online reviews that reinforce your manufacturing company's reputation for quality, reliability, and expertise. | ~10 min/use |

**Total time saved per use: ~440+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/manufacturing-ai-skills.git
cd manufacturing-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
manufacturing-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **Manufacturing AI guide**: [krasa.ai/industries/manufacturing](https://krasa.ai/industries/manufacturing)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
