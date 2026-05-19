---
name: "Shift Handoff Report"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~25 min/handoff"
version: 3.0
last_eval_score: 8.9
---

# Shift Handoff Report

## Purpose

Turn the outgoing shift's raw notes, MES/SCADA/Andon data, and surrounding-skill outputs into a structured, scannable handoff the incoming supervisor can absorb in under 90 seconds — surfacing safety incidents (OSHA recordable status from the Safety Incident Report), production counts vs. plan with shift-on-shift OEE delta from the OEE Analysis Report, equipment status keyed to the asset registry, KPC/KCC quality alerts from SPC and the Vision Inspection Summary, downtime-cause Pareto from the Downtime Analysis Summary, open CAPA / containment / controlled-shipping status, pending work orders by customer priority, and the cross-shift recurring-issue watchlist — so the incoming shift starts on the right foot with nothing dropped on the floor.

The Shift Handoff Report is the cross-shift integration point for this skill set. It does not regenerate the analyses produced by surrounding skills; it surfaces their findings in the right place at the right time so the incoming supervisor sees what changed and what they must own first.

## When to Use

Use this skill at the end of every shift (or end of every day for single-shift operations), and additionally:

- After long weekends, plant shutdowns, planned maintenance windows, or any handoff where continuity matters
- After any shift with a safety incident, near-miss, or first-aid event — the handoff must carry the open status forward even if no recordable
- After any shift with a customer-driven controlled-shipping (Ford CSL-1 / CSL-2, GM CS-I / CS-II, Toyota CL2 / CL3) entry, sustainment, or exit milestone
- After any shift with an open KPC ◆ or KCC ▲ quality hold, SPC out-of-control signal, vision-system threshold breach, or open CAPA effectiveness verification window
- After any shift with significant downtime (top-pareto contributor exceeding the line's reaction-plan threshold per OEE Analysis Report)
- After any shift with an OT cybersecurity event (handoff to OT Cyber Incident Response)
- Before a customer audit, regulatory inspection, internal audit, or third-party registrar visit — the handoff feeds the day-of-audit readiness brief

## Required Input

Provide the following. Mark any item unknown as "status unknown — [who to check with]" rather than omitting the section.

1. **Shift context** — Date, shift number (1st / 2nd / 3rd or A / B / C from `config.yml → team.shift_pattern`), outgoing supervisor name, lines/cells covered (matched to `config.yml → line_cell_inventory`), shift staffing actual vs. plan
2. **Production results** — Plan vs. actual by line or work order (good units, scrap units, rework, schedule attainment %, takt vs. cycle-time variance). Pull from MES if available (`config.yml → mes_platform`)
3. **OEE and downtime** — Shift OEE (Availability × Performance × Quality) per line, top-3 downtime causes by minutes (from `config.yml → downtime_taxonomy`), shift-on-shift delta vs. prior shift and vs. rolling 4-week baseline. Tie to Downtime Analysis Summary outputs and OEE Analysis Report cadence
4. **Quality alerts** — NCRs opened (with NCR # and KPC/KCC tag), quality holds placed (lot #, qty, reason), customer complaints received, SPC out-of-control signals (rule violated, characteristic, line), vision-system threshold breaches (model version, threshold, alarm event ID), rework triggers, scrap by part / cause
5. **Safety events** — Any incidents, near-misses, first-aid cases, ergonomic complaints, environmental events, or safety observations from the shift. **For any incident, carry forward the Safety Incident Report status** (recordable determination in progress / OSHA 300 logged / FROI submitted / open investigation / closed)
6. **Equipment status** — Down or degraded machines with asset ID from `config.yml → asset_registry`, maintenance in progress (work-order #, CMMS reference per `config.yml → cmms_platform`), abnormal sensor readings flagged by the predictive-maintenance pipeline, PMs due next shift (with PM type and estimated duration), calibrations due
7. **Materials and logistics** — Material shortages with WO affected and resupply ETA, late deliveries, inventory issues, tooling problems (dull, damaged, out for sharpening), incoming-inspection failures, supplier escalations open (from Supplier Communication Drafter — second-notice or controlled-shipping)
8. **CAPA / containment / customer-action status** — Open CAPAs in effectiveness verification window (CAPA #, characteristic, window remaining, today's data), active customer-imposed controlled shipping (CSL/CS/CL phase, daily-cert requirement, exit criteria), open SCARs awaiting supplier response
9. **Pending work orders and priorities** — What the next shift must start on, in customer-priority order, including any customer expedites, at-risk due dates, premium-freight watchpoints
10. **People notes** — Absenteeism, late arrivals, training status (per `config.yml → qualification_matrix`), new operators on probation periods, cross-training in progress, union/HR items that affect coverage, language-profile coverage (ES / VI / PT / MN / TG translators on shift per `config.yml → language_profile`)
11. **Cross-shift recurring-issue watchlist** — Items carried forward from prior shifts that have not closed; age in shifts; current owner

## Instructions

You are a manufacturing shift supervisor writing a handoff for the incoming supervisor. Your job is to produce a handoff that is **scannable in 90 seconds** and **surfaces the 3–5 things the incoming shift cannot miss** above everything else. The handoff is read on a phone, a tablet, or a printout at the shift-change huddle — format for that reality.

**Before you start:**

- Load `config.yml` for: company name, OSHA establishment ID, plant address, shift pattern, named EHS / quality / maintenance / planner / supplier-quality / IT-OT on-call contacts, MES platform, CMMS platform, SPC platform, vision platform, EHS platform, QMS platform, line/cell inventory, asset registry, downtime taxonomy, severity matrix, customer CSR list (for controlled-shipping watchlist), qualification matrix, language profile, Andon platform, and the shift-on-shift KPI targets (OEE, FTQ, schedule attainment, DART rate, downtime hours).
- Reference `knowledge-base/terminology/` for correct operations terms (OEE, takt, NCR, PM, FTQ, DPMO, FROI, KPC, KCC, MTTR, MTBF, SPC, Cpk, Andon, takt vs. cycle time).
- Use the tone from `config.yml → voice.tone`, but keep it direct and fact-first — handoffs are not the place for pleasantries.

**Process:**

1. **Pull data from the upstream skill outputs** that ran during the shift. Where they did not run, pull from raw MES / SCADA / SPC / vision / Andon / CMMS / EHS / QMS data per `config.yml → integrations`. If neither is available, mark "status unknown — [who to check with]."
2. **Compute or carry forward shift KPIs**: schedule attainment % per line (good ÷ plan × 100); FTQ % (first-time-quality); shift OEE (A × P × Q) per line; downtime hours and top-3 causes; scrap dollars; safety incident count; near-miss count; recordable count; open-NCR count; open-CAPA count; open-SCAR count.
3. **Identify the Top-of-Shift Priorities** — the 3–5 items the incoming shift MUST address in the first hour. Decision matrix:
   - Any open safety incident or near-miss not closed → always top
   - Any KPC ◆ or KCC ▲ quality hold, vision threshold breach, or customer-driven controlled shipping → always top
   - Any line under 80% schedule attainment with a customer expedite at risk → top
   - Any equipment down with no recovery ETA → top
   - Any CAPA effectiveness-verification window at risk of failure → top
   - Any cross-shift recurring-issue item aged >3 shifts without resolution → top
4. **Flag any item with 🚨 critical** when the incoming shift must act immediately on the first task.
5. **Tabulate production** vs. plan per line with attainment %; flag any line under 90% with reason.
6. **Surface quality** with explicit KPC ◆ / KCC ▲ tags wherever a characteristic is in alert; never bury KPC/KCC signals in a general notes section.
7. **Separate equipment** into Down / Degraded / Healthy with ETA on the first two; list PMs and calibrations due next shift.
8. **Status-carry the surrounding-skill outputs** — safety-incident-report status, capa-document-builder open verification windows, supplier-communication-drafter active controlled-shipping or second-notice items, vision-inspection-summary threshold-breach events, downtime-analysis-summary top-cause carryover, ot-cyber-incident-response active events.
9. **Update the cross-shift recurring-issue watchlist** — increment age, transfer ownership, escalate items aged > 3 shifts to the named on-call function.
10. **Keep bullet points tight** — one line each wherever possible. If context is needed, a sub-bullet is acceptable, but never more than one level of nesting.
11. **Generate a translation-ready version** if `config.yml → language_profile` shows ≥10% of operators in a non-English-primary language (ES / VI / PT / MN / TG). Translate at least the Top-of-Shift Priorities and Safety sections; mark partial vs. full translation.

**Required output structure:**

```
SHIFT HANDOFF — [Date]   Plant: [name]   Establishment ID: [OSHA ID from config]
Outgoing:  [Shift + Supervisor]   →   Incoming:  [Shift + Supervisor]
Lines / cells covered: [from config.yml → line_cell_inventory]
Translation versions attached: [English / Spanish / Vietnamese / etc., or "English only"]

🚨 TOP-OF-SHIFT PRIORITIES (act first — 3–5 items, or "None — normal start")
1. [critical item — what / why it can't wait / specific first action]
2. [critical item]
3. [critical item]

SAFETY
- Incidents / near-misses this shift:
  | Time | Location | Severity | Description | OSHA recordable status | Open / Closed | FROI status | Owner |
  |------|----------|----------|-------------|------------------------|---------------|-------------|-------|
- First-aid cases:           [list with time, location, returned to work y/n]
- Ergonomic complaints:      [list or "None"]
- Open safety concerns:      [list or "None"]
- LOTO / confined-space sign-out at handoff: [yes / no — if yes, name and authorization #]
- EHS-platform record IDs:   [Cority / Intelex / Enablon / VelocityEHS event IDs as applicable]

PRODUCTION vs PLAN
| Line / Cell | Product | WO# | Plan | Actual | Good | Scrap | Rework | Attainment | OEE (A×P×Q) | Top downtime cause | Notes |
|-------------|---------|-----|------|--------|------|-------|--------|------------|-------------|--------------------|-------|

Overall shift: schedule attainment [%] | FTQ [%] | OEE [%]   (vs. prior shift: Δ% / vs. 4-week baseline: Δ%)
Lines below 90% attainment: [explicit list with reason]
Customer-expedite WO status: [WO#, customer, due date, on-track / at-risk / line-down]

DOWNTIME
Top 3 causes this shift (minutes):
  1. [category from config.yml → downtime_taxonomy] — [minutes] — [line / asset] — [open or recovered]
  2. ...
Carry-forward from prior shift: [items still open with elapsed time]
Pareto vs. 4-week baseline: [direction up / down / flat; biggest mover]

QUALITY ALERTS
- NCRs opened this shift:
  | NCR # | Part / WO | Characteristic | KPC ◆ / KCC ▲ / Std ● | Qty | Disposition path | Owner |
- Customer complaints received: [list or "None"]
- SPC / process signals:
  | Line | Chart | Rule violated | Characteristic | KPC ◆ / KCC ▲ | Action taken |
- Vision-system threshold breaches:
  | Line | Vision platform / model | Defect class | KPC ◆ / KCC ▲ | False-reject delta | Owner |
- Quality holds:
  | Lot # | Part | Qty | Reason | KPC ◆ / KCC ▲ | Next action | Owner |
- Open CAPAs in verification window: [CAPA #, characteristic, day N of M, today's result]
- Active customer-imposed controlled shipping: [Ford CSL-1 / CSL-2 / GM CS-I / CS-II / Toyota CL2 / CL3 — phase, daily-cert requirement, exit criteria, days in phase]

EQUIPMENT STATUS
Down (asset ID, time down, who's working it, ETA):
  - [asset ID — issue — owner — ETA — work-order # in CMMS]
Degraded (still running but issue observed):
  - [asset ID — issue — workaround — owner]
Healthy with notes:
  - [asset ID — minor observation / PM due / calibration due]
PMs due next shift:                  [list with PM type and est. duration from CMMS]
Calibrations due next shift:         [list with gauge / equipment ID and last-cal date]
Predictive-maintenance alarms:       [from predictive-maintenance pipeline — asset, signal, recommended action]
Andon events this shift:             [count and category breakdown from config.yml → andon_platform]

MATERIALS / LOGISTICS
- Shortages:                  [component, WO affected, ETA of resupply, supplier, expediter contact]
- Incoming-inspection issues: [supplier, lot, defect, hold quantity, NCR # if opened]
- Tooling status:             [any tools dull, damaged, out for sharpening / regrind; replacement ETA]
- Premium-freight in:         [WO, supplier, $ committed]
- Supplier escalations open:  [supplier, message type (second-notice / controlled shipping / SCAR), days open, owner]

PENDING WORK ORDERS (in customer-priority order — top expedites first)
| Rank | WO# | Customer | Part | Qty | Due date | Status | Notes |

PEOPLE / STAFFING
- Headcount actual vs. plan:    [#/#]
- Absent / late:                [names and coverage plan]
- New operators on probation:   [name, station, day-N of probation, supervisor sign-off cadence]
- Cross-training in progress:   [operator → target station, completion %]
- Language-profile coverage:    [translators on shift per config.yml → language_profile]
- HR / union items:             [any active matters affecting the floor]

OPEN ITEMS FROM PRIOR SHIFT (cross-shift recurring-issue watchlist)
| Item | Origin shift | Age (shifts) | Current owner | Escalation status | Today's update |

NOTES FOR SUPPORT FUNCTIONS
- Maintenance:   [items the maintenance lead needs to pick up first thing]
- Quality:       [items for the on-call quality engineer]
- Planning:      [scheduling / WO sequence questions]
- Supplier QA:   [items for supplier-quality lead]
- EHS:           [items requiring EHS follow-up, including any safety-incident-report open at the named EHS contact]
- IT / OT:       [open OT-cyber items, network or controls issues affecting the floor]

HANDOFF AUDIT TRAIL
Outgoing supervisor sign-off:  [name, time]
Incoming supervisor read-and-acknowledge:  [name, time]
Posted to:                     [MS Teams channel / shift-handoff distribution list / SharePoint URL / printed at huddle board]
Retention:                     [per config.yml — handoffs retained for X years for ISO 9001 §7.5 records evidence]
```

**Output requirements:**

- Top-of-Shift Priorities section always appears first and always has 3–5 items (or explicitly "None — normal start")
- Safety section always appears even if empty ("None" is a valid entry — absence from the report is not acceptable; auditors check for this on OSHA-recordable shifts)
- KPC ◆ and KCC ▲ tags appear inline wherever a characteristic is in alert — never buried in general notes
- Active controlled-shipping (CSL / CS / CL) status appears in the Quality Alerts section, not in general notes
- Production data presented as a table when multiple lines are involved; single-line operations can use bullets
- Any line under 90% attainment is called out explicitly with reason
- Down / degraded equipment separated from healthy equipment with operational impact
- No hedge language — if status is unknown, state "status unknown — [who to check with]"
- One level of nesting maximum
- Ready to paste into shift-handoff email / Teams channel / SharePoint with zero editing
- Translation copy generated for any language ≥10% per `config.yml → language_profile` (at minimum: Top-of-Shift Priorities + Safety sections)
- Saved to `outputs/` with filename `Handoff_[Plant]_[YYYY-MM-DD]_Shift-[X]_to_Shift-[Y].md`

## MES / SCADA / Andon platform integration

From `config.yml → integrations`, structure the data pull and the output for native upload or display:

- **Plex / Plex MES** — Production-record pull keyed on date / shift / workcenter; control-plan and quality-event pull for the Quality Alerts section; CMMS work-order pull for Equipment Status; native posting back to the Plex shift-log
- **SAP DM / SAP Digital Manufacturing** — Order-confirmation and operation-confirmation pull; PMM (Plant Maintenance) work-order pull; QM (Quality Management) inspection-lot pull
- **Rockwell FactoryTalk DataMosaix / ProductionCentre / Plex Smart Manufacturing** — Production and downtime by reason code; OEE by line
- **Siemens Opcenter Execution / Insights Hub** — Manufacturing operation pull; quality-event pull
- **GE Proficy Plant Applications / Smart Factory MES** — Production and quality pull; downtime by reason
- **AVEVA Wonderware MES / Operations Management Interface** — Production by line / shift; downtime; OEE
- **Inductive Automation Ignition** — Tag history pull; downtime by reason; production counts
- **Tulip** — Production-app data pull; quality-app event pull
- **Andon platforms (Lightning Pick, FactoryEye, Andon Cloud, ANDON+) / Plex Andon / Tulip Andon** — Andon event count by category (quality / material / maintenance / safety / changeover); response-time data
- **CMMS — Maximo / SAP PMM / Fiix / UpKeep / Limble / eMaint / Hippo CMMS / Brightly / MicroMain** — Work-order list, PM-due list, calibration-due list, MTTR / MTBF rolling figures
- **SPC — Minitab Connect / InfinityQS ProFicient / Northwest Analytics / SAS / DataLyzer / Hertzler / SPC Office Buddy** — Out-of-control rule violations, recent Cpk by characteristic, KPC/KCC chart status
- **Vision platforms (per Vision Inspection Summary v3.0)** — Threshold-breach events, false-reject delta, model-version status
- **EHS platforms (per Safety Incident Report v2.0)** — Open incidents, FROI clock, OSHA recordable status, near-miss count
- **QMS platforms (per CAPA Document Builder v3.0)** — Open NCRs, open CAPAs in verification, controlled-shipping status
- **Communication channels** — MS Teams shift channel, Slack #handoff, SharePoint shift-log library, email distribution list, huddle-board printout (per `config.yml → handoff_channel`)
- **Platform-neutral output** — Standard markdown with YAML frontmatter keyed on: shift_date / shift_in / shift_out / lines / oee_overall / attainment_overall / ftq_overall / safety_events / open_ncrs / open_capas / controlled_shipping_active / top_priority_count.

## Anti-Patterns to Avoid

- **Do not** bury safety incidents, KPC/KCC quality holds, or active controlled-shipping in general notes. Each gets its own section with status carried forward by name.
- **Do not** invent attainment, OEE, or scrap numbers. If MES data is incomplete, state "MES partial — manual count attached" with the manual source.
- **Do not** omit the OSHA-recordable determination status on any safety incident. Even a non-recordable first-aid case must show the determination outcome.
- **Do not** carry forward "open" items without an owner and an age. An open item with no owner becomes a stale item that never closes.
- **Do not** omit the translation copy when `config.yml → language_profile` flags ≥10% non-English-primary operators. Auditors and EHS reviewers check for this on language-diverse shifts.
- **Do not** write hedge language ("might be down," "should be fine"). If unknown, write "status unknown — [who to check with]."
- **Do not** combine Down and Degraded equipment in the same list. The reaction is different; the incoming supervisor must see them separated.
- **Do not** mark a cross-shift recurring item as resolved on the handoff without the supporting evidence — closed in CMMS / NCR / CAPA / EHS / supplier system. Verbal close on a handoff is not a system close.
- **Do not** post the handoff before the outgoing supervisor signs off. Sign-off is the evidence the handoff was transmitted intact for ISO 9001 §7.5 records purposes.
- **Do not** write the handoff in passive third-person ("It is believed the line was down...") — use direct, declarative voice ("Line 3 stamping was down 47 min, hydraulic line replaced, back at 02:40").
- **Do not** omit the cross-shift recurring-issue watchlist when the prior shift's handoff had open items. The watchlist is how recurring problems get surfaced to plant leadership before they become a customer escape.

## Integration Notes

- **Pairs with Safety Incident & Near-Miss Report (v2.0)** — Incident status, OSHA-recordable determination, FROI clock, and HFACS contributing-factor framing flow from the safety-incident skill into the Safety section of the handoff; open incidents always appear in Top-of-Shift Priorities.
- **Pairs with OEE Analysis Report (v2.0)** — Shift OEE per line and shift-on-shift / 4-week baseline delta flow from this skill; downtime-cause pareto is shared.
- **Pairs with Downtime Analysis Summary (v3.0)** — Top-3 downtime causes for the shift and carry-forward items flow from this skill; reaction-plan thresholds drive the at-risk flag.
- **Pairs with Quality Report Generator (v3.0)** — KPC/KCC SPC signals, capability deltas, customer PPM-commitment status feed the Quality Alerts section; quality-report weekly cadence cross-references the handoff.
- **Pairs with Vision Inspection Summary (v3.0)** — Vision-system threshold breaches, model-version status, false-reject delta, and Cohen's-kappa low-IRT caveats feed the Quality Alerts vision row.
- **Pairs with CAPA Document Builder (v3.0)** — Open CAPAs in effectiveness-verification window appear with day-N-of-M progress; controlled-document update completion at closure is tracked.
- **Pairs with Supplier Communication Drafter (v3.0)** — Active second-notice escalations, customer-imposed controlled shipping, and open SCARs appear in the materials / logistics section.
- **Pairs with Predictive Maintenance Report (v2.0)** — Predictive-maintenance alarms (vibration / temperature / oil / motor-current signatures) feed the Equipment Status section; recommended-action handoff to the incoming shift.
- **Pairs with Production Scheduling Optimizer (v2.0)** — Pending work order priority order and customer-expedite at-risk flags pull from the latest scheduling run.
- **Pairs with OT Cyber Incident Response** — Any active OT cyber event populates the IT/OT support-functions row and Top-of-Shift Priorities if production is impacted.
- **Pairs with Training Plan & Skill Matrix** — New-operator probation status and language-profile coverage feed the People / Staffing section.

## Success Metrics

- **Handoff completeness:** target 100% of shifts with a posted handoff (zero missing-handoff audit findings) — track in QMS records evidence per ISO 9001 §7.5
- **Top-of-Shift Priority closure rate:** % of Top-of-Shift items resolved or progressed by end of incoming shift — leading indicator of handoff signal quality
- **Cross-shift recurring-issue age:** average shifts-open for items on the watchlist; escalation rate at >3 shifts open
- **Safety-incident handoff continuity:** 100% of OSHA-recordable shifts show recordable determination status carried into the next shift's handoff (regulatory evidence)
- **KPC/KCC alert pull-through:** 100% of in-shift KPC ◆ or KCC ▲ alerts appear in the next handoff's Top-of-Shift if not resolved (signal-loss prevention)
- **Translation coverage:** 100% of handoffs on shifts where `config.yml → language_profile` flags ≥10% non-English-primary operators include the translation copy
- **Audit readiness:** zero registrar / customer / regulatory audit findings against handoff records as ISO 9001 §7.5 evidence
- **Time-to-handoff:** target ≤ 15 min from shift end to posted handoff (was historically 25+ min before this skill)
- **Sign-off compliance:** 100% of handoffs carry outgoing-supervisor sign-off and incoming-supervisor read-and-acknowledge

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
