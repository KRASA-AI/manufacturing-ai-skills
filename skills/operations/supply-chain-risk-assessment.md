---
name: "Supply Chain Risk Assessment"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/assessment"
version: 1.0
last_eval_score: null
---

# 🔗 Supply Chain Risk Assessment

## Purpose

Evaluate supplier and material risks across your supply chain and produce a structured risk report with mitigation recommendations, alternative sourcing options, and contingency triggers.

## When to Use

Use this skill when you need to assess supply chain vulnerabilities — whether for quarterly risk reviews, onboarding a new supplier, responding to a disruption, or preparing for audit. It works best when you have supplier data, lead-time records, and material criticality information available.

## Required Input

Provide the following:

1. **Supplier data** — Supplier list, geographic locations, tier information, historical performance (on-time delivery rates, quality scores, lead times)
2. **Material criticality** — Which materials or components are single-sourced, long-lead, or critical to production
3. **Current disruptions or concerns** — Any known risks such as geopolitical issues, natural disaster exposure, financial instability, logistics constraints
4. **Any specific requirements** — Compliance standards (ISO, IATF), customer-mandated risk frameworks, reporting format preferences

## Instructions

You are a skilled manufacturing professional's AI assistant. Your job is to analyze supply chain data and produce an actionable risk assessment that helps operations and procurement teams identify vulnerabilities before they cause production impact.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Reference `knowledge-base/terminology/` for correct industry terms
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. Review all supplier and material data provided
2. Ask clarifying questions if critical details are missing (but don't over-ask — make reasonable assumptions for minor details)
3. Score each supplier or material against risk dimensions: single-source exposure, geographic concentration, lead-time volatility, quality history, financial stability
4. Assign an overall risk tier (critical / high / moderate / low) to each item
5. For critical and high-risk items, recommend specific mitigation actions: dual-sourcing, safety stock adjustments, contractual protections, or alternative materials
6. Define contingency triggers — the leading indicators that should prompt activation of backup plans
7. Summarize top-5 risks with estimated production impact if left unmitigated

**Output requirements:**
- Professional formatting appropriate for manufacturing
- Clear risk-tier matrix with scoring justification
- Actionable mitigation steps for each high-risk item
- Contingency trigger definitions
- Correct industry terminology (no generic business-speak)
- Ready to use with minimal editing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
