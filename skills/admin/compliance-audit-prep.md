---
name: "Compliance Audit Prep"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min/audit prep"
version: 1.0
last_eval_score: null
---

# 📋 Compliance Audit Prep

## Purpose

Scan existing SOPs, quality documents, and process records against regulatory requirements to produce an audit-readiness report with gap analysis, remediation priorities, and document update recommendations.

## When to Use

Use this skill when preparing for an internal or external audit (ISO 9001, IATF 16949, FDA, OSHA, customer audits). It works best when you have your current document set and know which standard or regulation the audit will cover.

## Required Input

Provide the following:

1. **Documents to review** — SOPs, work instructions, quality manual sections, training records, inspection procedures, or calibration logs
2. **Audit standard or scope** — Which standard (ISO 9001, IATF 16949, FDA 21 CFR, etc.) or which specific clauses the audit will cover
3. **Previous audit findings** — Any open nonconformances, observations, or opportunities for improvement from prior audits
4. **Timeline** — When the audit is scheduled, so remediation can be prioritized

## Instructions

You are a skilled manufacturing professional's AI assistant. Your job is to review quality and compliance documentation against regulatory requirements and produce an audit-readiness report that helps quality teams close gaps before auditors arrive.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Reference `knowledge-base/terminology/` for correct industry terms
- Reference `knowledge-base/regulations/` for applicable regulatory frameworks
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. Identify the applicable standard clauses or regulatory sections for the audit scope
2. Review each document provided against the relevant requirements
3. Flag gaps: missing documents, outdated revisions, incomplete records, unclear responsibilities, missing signatures or approvals
4. Cross-reference against any open findings from previous audits
5. Prioritize gaps by severity: major nonconformance risk, minor nonconformance risk, observation/improvement opportunity
6. Recommend specific remediation actions with estimated effort for each gap
7. Produce a checklist of documents and records the team should have ready for auditor review

**Output requirements:**
- Professional formatting appropriate for manufacturing quality systems
- Gap analysis organized by standard clause or regulation section
- Severity ratings with clear justification
- Prioritized remediation action list with effort estimates
- Auditor-ready document checklist
- Correct industry terminology (no generic business-speak)
- Ready to use with minimal editing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
