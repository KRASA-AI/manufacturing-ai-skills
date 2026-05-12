---
name: "Shift Handoff Report"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/handoff"
version: 2.0
last_eval_score: 8.6
---

# Shift Handoff Report

## Purpose

Turn the outgoing shift's raw notes into a structured, scannable handoff the incoming supervisor can absorb in under 90 seconds — surfacing safety incidents, production counts vs. plan, equipment status, quality alerts, and pending work orders so the incoming shift starts on the right foot with nothing dropped on the floor.

## When to Use

Use this skill at the end of every shift (or end of every day for single-shift operations) whenever production status, open issues, and priorities need to be communicated to the incoming team. Also useful after long weekends, plant shutdowns, or any handoff where continuity matters.

## Required Input

Provide the following:

1. **Shift context** — Date, shift number (1st/2nd/3rd or A/B/C), outgoing supervisor name, lines/cells covered
2. **Production results** — Plan vs. actual by line or work order (good units, scrap units, rework), schedule attainment %
3. **Safety events** — Any incidents, near-misses, first-aid cases, ergonomic complaints, or safety observations from the shift
4. **Quality alerts** — NCRs opened, quality holds placed, customer complaints received, SPC out-of-control signals, rework triggers
5. **Equipment status** — Any machine down or degraded, maintenance in progress, abnormal sensor readings, PMs due next shift
6. **Materials and logistics** — Material shortages, late deliveries, inventory issues, tooling problems, incoming lot issues
7. **Pending work orders / priorities** — What the next shift must start on, in priority order, including any customer expedites or at-risk due dates
8. **People notes** — Absenteeism, late arrivals, training status, union/HR items that affect coverage

## Instructions

You are a manufacturing shift supervisor writing a handoff for the incoming supervisor. Your job is to produce a handoff that is **scannable in 90 seconds** and **surfaces the 3–5 things the incoming shift cannot miss** above everything else.

**Before you start:**
- Load `config.yml` for company name, shift pattern (`team.shift_pattern` if defined), and production KPI targets
- Reference `knowledge-base/terminology/` for correct operations terms (OEE, takt, NCR, PM, FTQ, DPMO)
- Use the tone from `config.yml` → `voice` → `tone`, but keep it direct and fact-first — handoffs are not the place for pleasantries

**Process:**

1. Parse input and route each item to the correct section of the output template.
2. Identify the **Top-of-Shift Priorities** — the 3–5 items the incoming shift MUST address in the first hour. Pull these from safety incidents still open, quality holds, at-risk due dates, and equipment issues still in progress.
3. For production: compute schedule attainment % if raw numbers are provided (good units ÷ plan units × 100). Flag any line under 90% attainment.
4. For quality: escalate any open NCR, customer complaint, or SPC signal — never bury these in a general notes section.
5. For equipment: list down and degraded machines separately from healthy ones. Include ETA to recovery if known.
6. Flag anything marked with 🚨 (critical — incoming shift must act immediately).
7. Keep bullet points tight — one line each wherever possible. If context is needed, a sub-bullet is acceptable, but never more than one level of nesting.

**Required output structure:**

```
SHIFT HANDOFF — [Date]
Outgoing: [Shift + Supervisor]   →   Incoming: [Shift + Supervisor]

🚨 TOP-OF-SHIFT PRIORITIES (act first)
1. [critical item — what and why it can't wait]
2. [critical item]
3. [critical item]

SAFETY
- Incidents / near-misses: [list with time, location, brief description, status — or "None"]
- Open safety concerns: [list — or "None"]

PRODUCTION vs PLAN
| Line / Cell | Product | Plan | Actual | Scrap | Attainment | Notes |
|-------------|---------|------|--------|-------|------------|-------|
| [line]      | [WO#]   | [qty]| [qty]  | [qty] | [%]        | [brief] |

Overall schedule attainment: [%]   FTQ: [%]   Downtime: [hours, top cause]

QUALITY ALERTS
- NCRs opened this shift: [list with NCR#, part, issue, quantity, status]
- Customer complaints: [list or "None"]
- SPC / process signals: [any out-of-control points, drifts, or tool-wear flags]
- Quality holds: [lots on hold with lot #, reason, next action]

EQUIPMENT STATUS
Down / degraded:
- [Machine ID]: [issue, who's working it, ETA]
Healthy with notes:
- [Machine ID]: [PM due, minor issue observed, etc.]
PMs due next shift: [list]

MATERIALS / LOGISTICS
- Shortages: [component, WO affected, ETA of resupply]
- Incoming issues: [late trucks, failed incoming inspection, etc.]
- Tooling: [any tools dull, damaged, or out for sharpening]

PENDING WORK ORDERS (priority order)
1. [WO# — product — qty — due date — at-risk? why]
2. [WO#]
3. [WO#]

PEOPLE / STAFFING
- Absent: [names and coverage plan]
- Cross-training / new operators: [who's on what equipment]
- HR / union items: [any active matters affecting the floor]

OPEN ITEMS FROM PRIOR SHIFT
- [carry-forward items still not resolved]

NOTES FOR MAINTENANCE / QUALITY / PLANNING
- [anything the support functions need to pick up]
```

**Output requirements:**
- Top-of-Shift Priorities section always appears first and always has 3–5 items (or explicitly state "None — normal start")
- Safety section always appears even if empty ("None" is a valid entry — absence from the report is not acceptable)
- Production data presented as a table when multiple lines are involved; single-line operations can use bullets
- Any line under 90% attainment is called out explicitly with reason
- NCRs and quality holds never buried in general notes — they get their own section
- Down/degraded equipment separated from healthy equipment with operational impact
- No hedge language — if status is unknown, state "status unknown — [who to check with]"
- Ready to paste into shift-handoff email/Teams channel with zero editing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
