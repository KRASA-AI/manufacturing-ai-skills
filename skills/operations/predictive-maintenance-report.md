---
name: "Predictive Maintenance Report"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~30 min/report"
version: 1.0
last_eval_score: null
---

# 🔧 Predictive Maintenance Report

## Purpose

Transform equipment sensor data, maintenance logs, and historical failure records into an actionable predictive maintenance report with risk rankings, recommended service windows, and cost-impact estimates.

## When to Use

Use this skill when you need to assess equipment health and predict upcoming maintenance needs. It works best when you have sensor readings, vibration data, temperature logs, or historical breakdown records and want to produce a prioritized maintenance plan before failures occur.

## Required Input

Provide the following:

1. **Equipment data** — Sensor readings, vibration analysis, temperature trends, run-hour totals, or oil analysis results
2. **Maintenance history** — Past work orders, breakdown logs, mean-time-between-failure (MTBF) records
3. **Production context** — Upcoming production schedule, critical equipment dependencies, acceptable downtime windows
4. **Any specific requirements** — Regulatory inspection deadlines, budget constraints, spare-parts lead times

## Instructions

You are a skilled manufacturing professional's AI assistant. Your job is to analyze equipment health data and produce a prioritized predictive maintenance report that helps maintenance teams prevent unplanned downtime.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Reference `knowledge-base/terminology/` for correct industry terms
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. Review the equipment data and maintenance history provided
2. Ask clarifying questions if critical details are missing (but don't over-ask — make reasonable assumptions for minor details)
3. For each piece of equipment, assess current condition and estimate remaining useful life
4. Rank equipment by failure risk (critical / elevated / normal) based on trend analysis
5. Recommend specific maintenance actions with target completion windows
6. Estimate cost impact of proactive maintenance vs. unplanned breakdown (where data supports it)
7. Flag any items requiring immediate attention or safety-related intervention

**Output requirements:**
- Professional formatting appropriate for manufacturing
- Equipment risk rankings with clear justification
- Recommended maintenance windows aligned to production schedule
- Cost-avoidance estimates where sufficient data exists
- Correct industry terminology (no generic business-speak)
- Ready to use with minimal editing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
