# Manufacturing AI Skills

**Free, open-source AI prompts and workflows built for manufacturers.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for manufacturing. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Downtime Analysis Summary | Turn a period's raw downtime events — pulled from the plant's OEE / MES / historian / andon stack — into a ranked, categorized, root-cause-aware summary that tells a plant manager which two or three countermeasures will move the needle, with bottleneck-line losses framed as the dominant lever, planned and unplanned losses cut separately, chronic and assignable patterns named, and an honest line between what the data shows and what still needs floor verification. | ~45 min/analysis |
| OEE Analysis Report | Take a period's Overall Equipment Effectiveness (OEE) data — availability, performance, quality, and the underlying loss events from the OEE platform of record (Aveva PI / OSIsoft, GE Proficy, Siemens Insights Hub, AspenTech IP.21, Ignition, Tulip, MachineMetrics, Plex Asset Performance, Fiix, Maximo APM, FactoryTalk Metrics) — and produce an honest, prioritized analysis that names the bottleneck-line OEE before the plant-average OEE, separates the Six Big Losses correctly, anchors against the per-line target the plant actually owns rather than a generic "world-class 85%" headline, ties each top-3 driver to a TPM pillar so the countermeasure has a home, and lands two or three actions someone on the floor can start on Monday. | ~75 min/report |
| OT Cybersecurity Incident Response Playbook | Turn a suspected or confirmed cyber event on the plant floor into a structured, time-boxed response that protects life-safety, contains spread between IT and OT, preserves forensic evidence, restarts production from a known-good baseline, and meets every external notification clock the event triggers. | ~6 hr/incident (first 24 h) |
| Predictive Maintenance Report | Turn raw condition-monitoring inputs — vibration, oil, infrared, motor current and stator flux, ultrasonic, partial-discharge, run-hours, and CMMS work-order history, augmented by vendor AI/ML classified alerts from Augury, SKF, Emerson, Senseye, Petasense, ABB, Schaeffler, IBM Maximo Predict, AVEVA Predictive Analytics, GE Digital APM, Bently Nevada System 1, Aspen Mtell, and AT&T Connected AI for Manufacturing — into a defensible predictive maintenance (PdM) report that (a) ranks assets by risk of functional failure, (b) assigns a remaining-useful-life (RUL) band and a P-F interval interpretation, (c) recommends a specific disposition per asset (run-to-failure / inspect / schedule / near-term / emergency), (d) updates the PM program with add / tighten / relax / retire decisions, (e) produces the spare-parts pull list the storeroom needs to have staged before the work order hits the floor, (f) explicitly delineates physics-grounded vs ML-classified alert provenance and runs the adversarial agreement check, (g) hands off the alarm row to the Shift Handoff Report's predictive-maintenance section, and (h) tracks the reliability program's own maturity KPIs distinct from individual-asset health. | ~45 min/report + avoided-breakdown hours + shift-handoff alarm-row pull-through |
| Production Scheduling Optimizer | Take a set of production orders, machine and labor capacities, material constraints, and ranked objectives and produce an improved short-horizon production schedule with the bottleneck scheduled first, family changeovers grouped, material readiness verified, schedule stability protected, and every at-risk order named with the single biggest constraint that put it at risk. | ~90 min/schedule |
| Quality Report Generator | Turn inspection data (incoming, in-process, final, and customer returns) into a structured weekly / monthly quality report with defect Pareto, SPC trend read-out, Cp / Cpk interpretation, escape analysis, customer-CSR scorecard alignment (Ford Q1, GM BIQS, Stellantis CQI, Boeing AS13100, Lockheed AQS, Honeywell HOS, Caterpillar QPN, JLG, John Deere AQS, Toyota STR, GE Aviation S-1000D, FDA-listed program, etc.), APQP / PPAP gate framing where applicable, and prioritized corrective actions ranked by expected COPQ recovery — ready for a weekly quality review, a customer scorecard submission, or a management scorecard. | ~25 min/report |
| SOP Writer | Turn process notes, operator interviews, or tribal knowledge into a controlled, audit-ready Standard Operating Procedure that functions as the governing document for a manufacturing process — complete with document-control header, safety callouts, PFMEA-linked quality checkpoints, KPC/KCC designations, PPE requirements, regulated-industry framing, QMS-platform export structure, and an ECN-triggered requalification handoff. | ~45 min/SOP |
| Shift Handoff Report | Turn the outgoing shift's raw notes, MES/SCADA/Andon data, and surrounding-skill outputs into a structured, scannable handoff the incoming supervisor can absorb in under 90 seconds — surfacing safety incidents (OSHA recordable status from the Safety Incident Report), production counts vs. | ~25 min/handoff |
| Supply Chain Risk Assessment | Turn a tiered supplier list, material criticality map, and current disruption inputs into a defensible supply-chain risk assessment that (a) scores every critical supplier and material on a seven-dimension risk taxonomy, (b) assigns a risk tier using an explicit Likelihood × Severity matrix, (c) flags CBAM-covered goods and forced-labour jurisdiction exposure before customers or regulators find them, (d) produces mitigation playbook recommendations tiered from dual-sourcing to contract clauses, and (e) defines leading-indicator contingency triggers so disruption response is pre-decided rather than improvised. | ~45 min/assessment + avoided line-down hours |
| Vision Inspection Summary | Take the output of an automated computer-vision inspection system — pass / fail counts, defect classifications, model confidence scores, flagged images, calibration events, human-review resolutions, and model-lineage metadata — and produce a structured shift-end summary that quality and operations teams can act on within 30 minutes. | ~45 min/report |
| Work Instruction Generator | Turn a controlled SOP, an engineer's walk-through, a process video transcript, or a paper traveler into a station-specific digital work instruction (DWI) — operator-facing, tablet/HMI-ready, hard to do wrong, traced back to a controlled-document revision, and aligned to whatever connected-worker platform the plant runs (Tulip, Augmentir / SymphonyAI, Zaptic, Apprentice, Poka, Azumuta, Opcenter Connect Quality, Plex MES Workbench, FactoryTalk Operation Suite, SAP DMC, Auros KS). | ~2 hrs/instruction set |
| CAPA Document Builder | Build an audit-ready Corrective and Preventive Action record that satisfies ISO 9001 §10.2, IATF 16949 §10.2.3–10.2.6 (problem-solving / error-proofing / lessons-learned), AS9100D §10.2, FDA 21 CFR 820.100 (medical devices), 21 CFR 211.192 (pharma deviation/CAPA), and the customer-specific 8D requirements of every major automotive and aerospace OEM CSR — with an IS / IS-NOT problem statement, KPC/KCC-aware root-cause analysis tied to the control plan and PFMEA, separated containment / corrective / preventive actions, structured agentic-RCA-pattern output, a defined effectiveness-verification plan with sampling-plan rigor, an explicit controlled-document update list, and a QMS-platform-native record ready for upload to MasterControl / ETQ Reliance / Plex / Greenlight Guru / Sparta TrackWise / IQS / Veeva Vault QMS / AssurX / SAP QM. | ~75 min/CAPA |
| CMMC Level 2 Self-Assessment & SPRS Score Prep | Turn a manufacturer's current cybersecurity posture into a CMMC 2.0 Level 2 self-assessment package: a scoped System Security Plan (SSP), a control-by-control implementation status against the 110 NIST SP 800-171 Rev. | ~3 day/cycle (gap analysis + evidence map + scoring + SPRS submission package) |
| Compliance Audit Prep | Turn a plant's controlled-document set, training records, prior-audit findings, and forward-look regulatory horizon into an audit-readiness report that (a) maps existing documentation to the specific standard clauses in scope, (b) flags gaps by severity (Major NC / Minor NC / Observation / OFI), (c) closes prior-cycle findings with effectiveness-verification evidence, (d) produces the objective-evidence pull list the auditor will actually request, (e) runs a mock-audit interview script set on the highest-risk clauses, (f) scripts the plant's response to "auditor catnip" questions that trip up most sites, (g) runs an explicit FDA QMSR / CMMC 2.0 Phase 2 / ISO 9001:2026 transition-readiness pre-pass for sites in those regulatory horizons, and (h) integrates ISO 27001 + ISO 42001 + CSRD / IFRS S1 / IFRS S2 overlap for the integrated-management-system audit. | ~90 min/audit prep + reduced finding volume + ISO 9001:2026 / FDA QMSR / CMMC 2.0 Phase 2 transition coverage |
| Safety Incident & Near-Miss Report | Turn a raw account of a plant-floor safety event — whether an actual injury, a property-damage incident, a near-miss, or a SIF precursor — into a complete, audit-ready incident report. | ~40 min/incident |
| Supplier Communication Drafter | Draft professional, contractually-aware supplier communications across the full PO-to-payment lifecycle — PO confirmations and expedites, Supplier Corrective Action Requests (SCARs / 8D requests), delivery escalations, quality holds, customer-driven controlled-shipping cascades (Ford CSL-1 / CSL-2, GM CS-I / CS-II, Stellantis NCT, Toyota CL2 / CL3, Honda QAS escalation), Section 232 / Section 301 / Section 232 pharmaceutical tariff cost pass-through requests, RFQ follow-ups, scorecard feedback, PPAP submission requests, end-of-life and last-time-buy notices, and supplier-audit notices — in the plant's voice, with the right level of firmness for the situation, the supplier-tier relationship, and the governing quality-agreement clause. | ~20 min/email |
| Sustainability & Emissions Report | Turn raw plant utility, production, and material data into a defensible sustainability and emissions report for customers, regulators, and internal leadership. | ~3 hr/reporting cycle |
| Tariff Impact Analysis | Turn a product line, BOM, or inbound shipment schedule into a defensible duty-exposure analysis and a response plan. | ~4 hr/product family |
| Training Plan & Skill Matrix Generator | Turn a roster of operators, a list of stations or tasks, and a set of regulatory and customer competence requirements into three artifacts the plant actually uses: a current-state skill matrix scored on a four-tier proficiency scale, a per-operator training plan with time-to-qualified targets and explicit gate checks, and a recertification calendar that closes the audit loop on competence. | ~6 hr/operator (onboarding plan + qualification card + recert schedule) |
| Warranty Claim Analyzer | Turn raw warranty claim inputs — customer descriptions, dealer write-ups, failed-part RMAs, field-service notes, social-listening signals — into structured, auditable claim records that (a) classify the issue, (b) assign a dynamic risk score with named contributors, (c) flag fraud and anomaly patterns through a multi-signal taxonomy routed to a fraud review queue rather than denial, (d) roll up across claims to surface emerging defect clusters before they become a field campaign, (e) hand confirmed clusters off to the CAPA Document Builder's agentic-RCA workflow with a controlled-shipping-trigger evaluation, and (f) drive a six-stage forward-risk reserve model the CFO can defend at quarter close. | ~25 min/claim + ~2 hr/weekly pattern review + ~3 hr/quarterly reserve review |
| Email Drafter | Draft a professional manufacturing email that uses the right industry vocabulary, separates facts from asks, names a clear next step with an owner and a date, and never fabricates an identifier (PO, NCMR, SCAR, RMA, lot number, drawing revision, certification clause). | ~15 min/email |
| Meeting Summarizer | Turn raw meeting notes — transcript, bullet list, scribe pad, paper sign-in roster — into the structured summary the meeting type calls for, with the right header fields, the right action / decision separation, the right safety / quality / compliance escalation block, and (for management reviews) the right ISO 9001 §9.3 / IATF §9.3.2 / AS9100 §9.3 / ISO 13485 §5.6 evidence cross-reference so the summary can stand as the audit record. | ~20 min/meeting |
| Review Responder | Craft public review responses for a manufacturing company that protect reputation, do not create legal or contractual exposure, and reinforce the credentials a B2B buyer or potential employee actually cares about. | ~15 min/use |

**Total time saved per use: ~740+ minutes across all skills.**

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
