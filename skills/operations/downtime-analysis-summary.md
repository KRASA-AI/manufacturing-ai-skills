---
name: "Downtime Analysis Summary"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/analysis"
version: 3.0
last_eval_score: 8.9
---

# Downtime Analysis Summary

## Purpose

Turn a period's raw downtime events — pulled from the plant's OEE / MES / historian / andon stack — into a ranked, categorized, root-cause-aware summary that tells a plant manager which two or three countermeasures will move the needle, with bottleneck-line losses framed as the dominant lever, planned and unplanned losses cut separately, chronic and assignable patterns named, and an honest line between what the data shows and what still needs floor verification.

This is the availability deep-dive that pairs with the OEE Analysis Report. The OEE skill is the headline; this skill is the drill-down. A plant that runs both reports each week converges fastest on its top-3 chronic Pareto.

## When to Use

Use when you have downtime events to analyze across a shift, day, week, or month and need a report that answers four questions in order:

1. How much availability loss did we take, and against what baseline — and how much of it landed on the bottleneck line, where every minute lost is a minute the plant cannot recover?
2. Which equipment and which reason codes drove the loss (Pareto by line, by reason, by 4M, by SEMI E10 state)?
3. What are the most credible root-cause hypotheses (planned vs. unplanned, assignable vs. chronic, single-point-of-failure flag, TPM-pillar mapping)?
4. What are the 2–3 highest-leverage countermeasures, and what's the evidence behind each — including a verification metric, threshold, and window?

Good triggers: weekly OEE / MDI / tier-2 review prep, chronic-loss war-room kickoff, post-audit finding on availability (IATF §6.1.2.3 contingency plans, AS9100 §8.1.2 risk to product realization), any line missing its availability target by >2 points for two weeks running, a customer-CSR escalation citing OTD risk, a PdM platform alarm trail that didn't get acted on, or a spare-parts stock-out that took the line down.

## Required Input

1. **Period and scope** — Date range, shift coverage, lines / cells / assets in scope. Mark which line is the bottleneck (the constraint that sets plant throughput); if the bottleneck shifts during the period, list both.
2. **Scheduled production time** — Total available minutes after planned non-production time is removed, per the plant's convention. State the convention used (TEEP vs. classic OEE, breaks-in vs. breaks-out).
3. **Downtime event list** — Each event with: equipment ID, line, start time, duration, reason code, sub-code, free-text note, responding function (operator / maintenance / quality / materials / changeover / engineering / IT-OT). For OEE-platform exports, the SEMI E10 / ISA-95 state is preferred; for andon-only logs, accept the local taxonomy and note it.
4. **OEE / MES / historian source** — Name the system (Aveva PI / OSIsoft, GE Proficy / Plant Applications, Siemens Insights Hub / Industrial Edge, AspenTech IP.21, Ignition with Sepasoft, MachineMetrics, Tulip, Plex Asset Performance, FactoryTalk Metrics / Historian, Maximo APM, Fiix, eMaint, Sight Machine, Litmus Edge, Augury, Senseye / Siemens). Note whether reason-code capture is auto (PLC fault → category map) or operator-attested (andon push) — accuracy and bias differ.
5. **Baseline** — Prior 4-week rolling availability per line, prior month, year-over-year, target by line, world-class anchor (≥90% availability for discrete; differs for continuous / batch / cell / job-shop).
6. **Asset criticality and SPOF list** — Single-point-of-failure assets on the bottleneck line, redundancy where it exists, and assets currently under PdM watch.
7. **Active issues** — Open CAPAs touching availability, PM-schedule gaps, pending spare-parts orders with ETAs, new-product / new-tooling ramps, recent operator-roster turnover, any IT / OT / cyber event that took capacity offline.
8. **Operator and maintenance notes** — Tribal-knowledge commentary that wouldn't be in the event log ("the fault clears when we reseat the photoeye, happens after lunch"; "fault code 2317 only on second-shift after the changeover at 18:00"; "PM was deferred two weeks because spare on order").

## Instructions

You are a manufacturing continuous-improvement engineer (or TPM / reliability engineer) writing a downtime review for a plant manager and line supervisors. Your job is to produce a report that can be read in five minutes, is honest about what the data does and does not support, and ends with 2–3 countermeasures that someone on the floor can actually start on Monday — with a verification metric on each.

**Before you start:**

- Load `config.yml` for plant, line / cell / asset inventory, bottleneck-line designation, OEE / availability targets per line (`operations.availability_targets`), shift pattern, planned-stop taxonomy, OEE-platform name, reason-code taxonomy in use, asset-criticality and SPOF list, PM cadence per asset, and customer-CSR OTD commitments that are at risk if the line slips.
- Reference `knowledge-base/terminology/` for OEE, availability, MTBF, MTTR, MTTF, MTTA, SEMI E10, ISA-95 production-state model, 4M, Ishikawa, 5-Why, TPM eight pillars, FMEA / RPN, Pareto, world-class anchor.
- Confirm the baseline window the plant uses (typical: prior 4-week rolling, or prior month). Use the same convention every cycle.

**Process:**

1. **Compute availability and losses by line.** Availability = (Scheduled time − Total downtime) / Scheduled time. Show the math. Compute delta vs. baseline and vs. target. Compute it separately for the bottleneck line and the non-bottleneck lines — these are different problems with different leverage.
2. **Lead with the bottleneck.** The headline number is bottleneck-line availability and bottleneck-line minutes lost. A non-bottleneck line at 78% availability is usually fine; a bottleneck line at 88% is a plant-throughput emergency.
3. **Separate planned from unplanned.** Changeovers, tool changes, PMs, scheduled breaks, scheduled startup / shutdown — not the same as unplanned faults. Show both cuts. Flag planned-time creep separately (changeover duration trending up, PM overruns, ballooning startup losses).
4. **Build a Pareto.** Rank reason codes by total minutes lost (descending) for the bottleneck line specifically, then for the plant overall. Call out the top 3 and the 80% cutoff line — the vital few.
5. **Categorize by 4M and by SEMI E10 state.** A second Pareto by 4M (Man, Machine, Material, Method) often reveals a pattern the reason-code Pareto hides ("material" events look small individually but aggregate to the #1 category). For continuous / discrete systems already running E10, also cut by E10 state (Productive, Standby, Engineering, Scheduled-Downtime, Unscheduled-Downtime, Non-Scheduled).
6. **Compute MTBF and MTTR for the top contributors.** A reason code with few events and long duration (high MTTR) requires a different countermeasure (faster response, better spares, better diagnostics) from one with many events and short duration (low MTBF, structural fix to the failure mechanism).
7. **Chronic vs. assignable.** Flag each top contributor as either *chronic* (recurring across weeks, Pareto-stable) or *assignable* (a spike tied to a specific event — a PM miss, a new operator, a bad lot, a contaminated coolant batch, a cyber-OT incident, a spare-stockout). Assignable gets a one-time fix; chronic gets a structural countermeasure.
8. **SPOF and asset-criticality call-out.** Cross-reference each top contributor against the SPOF list. A chronic loss on a single-point-of-failure asset on the bottleneck line is the highest-leverage finding the report can produce. Name it as such.
9. **Map top contributors to TPM pillars.** Autonomous Maintenance, Planned Maintenance, Focused Improvement (Kobetsu Kaizen), Early Equipment Management, Quality Maintenance, Education and Training, Safety / Health / Environment, Office TPM. The pillar mapping tells the plant which standing process owns the countermeasure — not just which engineer.
10. **Run a short 5-Why for the #1 contributor.** Not a full RCA — a 5-Why stub that points toward the most credible hypothesis and explicitly notes which "why" is verified (event log / device data / PM record / video) and which is still inference.
11. **Reconcile against the prior period's action register.** If the plant has been running this report for a while, every prior-period countermeasure must be marked Closed / In-Progress / Slipped, with the verification metric pulled forward. A countermeasure that didn't move its metric is not closed — it's failed and needs a different hypothesis.
12. **Recommend 2–3 countermeasures.** Each must be concrete (who, what, when, how verified), tied to a hypothesized cause, scoped to effort tier (*quick win* this week / *kaizen* this month / *engineering change* next quarter / *capital* next budget cycle), and accompanied by a verification metric / threshold / window — e.g., "this reason code under 30 min/week for 3 consecutive weeks." Do not recommend more than 3 — optionality kills execution on the floor.
13. **Be honest about uncertainty and data quality.** Where the data is ambiguous, say so. Where reason-code capture is operator-attested and likely biased (the convenient code, the politically safe code, the one that doesn't trigger andon escalation), say that too. A report that claims confident root cause from thin event-log data will burn credibility fast.

## Required Output Structure

```
DOWNTIME ANALYSIS — [Plant / Bottleneck Line] — [Period]                  Source: [OEE platform]
Baseline window: [prior 4-week rolling]   Convention: [TEEP / classic / breaks-in or out]

HEADLINE
- Bottleneck-line availability: [%] (baseline: [%], target: [%], world-class: 90%, delta: [±pts])
- Plant availability (weighted): [%] (baseline / target / delta)
- Total downtime: [hours] across [N] events on [N] assets
- Planned: [hrs] ([%])   Unplanned: [hrs] ([%])
- Top driver on bottleneck line: [reason code — min — % of bottleneck-line unplanned]
- Bottom line in one sentence: [what the plant manager needs to do this week]

LOSS BREAKDOWN — BOTTLENECK LINE FIRST

By reason code (bottleneck line, unplanned only, top 8 + "other"):
| Rank | Reason code | Events | Total min | % of unplanned | MTBF | MTTR | C / A | SPOF | TPM pillar |
|------|-------------|--------|-----------|----------------|------|------|-------|------|-----------|
| 1    | [code]      | [n]    | [min]     | [%]            | [min]| [min]| [C/A] | [Y/N]| [pillar]  |
| 2    | ...         |        |           |                |      |      |       |      |           |

Pareto cutoff (80% of bottleneck unplanned): rank [N] — the vital few.

By 4M category (bottleneck line):
| Category | Events | Total min | % of unplanned | Notes |
|----------|--------|-----------|----------------|-------|
| Man      |        |           |                |       |
| Machine  |        |           |                |       |
| Material |        |           |                |       |
| Method   |        |           |                |       |

By asset (top 5 plant-wide):
| Asset | Line | Events | Total min | Top reason code | MTBF | MTTR | SPOF on bottleneck? |
| ...   |      |        |           |                 |      |      |                      |

PLANNED vs UNPLANNED VIEW
- Planned downtime was [hrs]: [list of major categories — changeovers, PMs, breaks, startup, shutdown]. Flag any planned-time creep (changeover duration trending up vs. baseline, PM overruns, ballooning startup losses).
- Unplanned downtime was [hrs] — focus of countermeasures.
- Asynchronous-loss check: any unplanned loss on a non-bottleneck line that did NOT translate to bottleneck starvation? Those are real losses; they may not be plant-throughput losses.

ROOT-CAUSE HYPOTHESIS (top contributor on bottleneck line only)

5-Why stub for: [reason code]
1. Why did the line stop?           → [direct trigger — verified from event log / PLC fault]
2. Why did that trigger?            → [mechanism — cite source: event detail / device data / operator note / maintenance note]
3. Why did that happen?             → [underlying condition — flag as inference if not verified on the floor]
4. Why has it persisted?            → [system level — missing PM, drifted spec, training gap, no PdM signal]
5. Why does the system allow it?    → [governance / process gap — TPM pillar level]

Confidence: [high / medium / low] — [what evidence would raise it]
Floor-verification gaps: [what would need to be checked on the floor to harden this]
TPM pillar that owns the countermeasure: [pillar]

PRIOR-PERIOD ACTION-REGISTER RECONCILIATION
| # | Countermeasure (prior period) | Status | Verification metric | Result | Conclusion |
|---|-------------------------------|--------|---------------------|--------|-----------|
| 1 | [item]                        | Closed / In-Progress / Slipped / Failed | [metric and threshold] | [number] | [keep / iterate / new hypothesis] |

COUNTERMEASURES (max 3, ranked by leverage)

1. [Title — e.g., "Reseat photoeye after lunch break, add clean-on-PM step"]
   - Effort tier: quick win / kaizen / engineering change / capital
   - Owner: [function or name + TPM pillar]
   - Target date: [date]
   - Hypothesis it addresses: [which 4M / which 5-Why step / which top-RPN failure mode]
   - Hypothesized minutes / availability point recovery: [number — model assumption]
   - How we'll know it worked: [metric, threshold, window — e.g., "this reason code under 30 min/week for 3 consecutive weeks on the bottleneck line"]
   - Cost-vs-benefit sketch: [order-of-magnitude only — e.g., "$0 / 2 hours" vs. "$15K capital / 6 weeks"]

2. [Title]
   - ...

3. [Title] (optional — only if leverage is clear)
   - ...

WATCHLIST (not countermeasures yet — need more data)
- [Suspicious patterns that aren't yet actionable — e.g., "fault 2317 clusters between 18:00 and 19:00 on second-shift only; need a week of cleaner data"]

DATA-QUALITY NOTES
- Reason-code capture: [auto from PLC / operator-attested via andon / mixed]
- Events missing reason codes: [count and %]
- Operator short-cycling to avoid the log: [observed / not observed]
- PM overruns miscategorized as production: [yes / no]
- E10-state mapping completeness: [% of events mapped]
- Cyber / OT-event-window check: [confirm no events fall inside an OT cyber-incident window — coordinate with OT Cyber Incident Response skill if any did]
```

## Anti-Patterns to Avoid

- **Do not** lead with plant-weighted availability when the bottleneck line is the constraint. The bottleneck is the headline; the average dilutes urgency.
- **Do not** blend planned and unplanned downtime in the Pareto. Hides the real levers and lets changeover creep / PM overrun hide in the "planned" bucket.
- **Do not** present a reason-code Pareto without also cutting by 4M and (where available) SEMI E10 state. Reason codes are symptoms; 4M and E10 are often the pattern.
- **Do not** recommend more than 3 countermeasures. Optionality kills execution on the floor; an unranked list of 7 is a list nobody works.
- **Do not** claim root cause from event-log data alone. The log gives you hypotheses; the floor gives you causes. Mark which 5-Why steps are verified vs. inference.
- **Do not** ignore SPOF on the bottleneck line. A chronic loss on a single-point-of-failure asset on the constraint is the single highest-leverage finding any downtime report can produce — name it as such.
- **Do not** cite "operator error" as a root cause. That is a symptom of a method, training, or error-proofing gap. Route to Work Instruction Generator and Training Plan & Skill Matrix.
- **Do not** characterize a named operator's performance in the report. Reasoning about the system, not the worker, is the rule.
- **Do not** close out a prior-period countermeasure without checking its verification metric. A countermeasure that didn't move its metric is not closed; it's failed.
- **Do not** model "expected availability-point recovery" as confident. Mark it as a hypothesis; the verification metric is what closes the loop.
- **Do not** ignore the OT / cyber timeline. If the plant had a confirmed OT cyber event in the period, downtime around the event window is not a maintenance Pareto entry — coordinate with the OT Cyber Incident Response skill.
- **Do not** write the 5-Why as five confident sentences. Mark verified vs. inference and explicitly state what would raise confidence.

## Integration Notes

- **Pairs with OEE Analysis Report** — downtime is the availability component; this skill drills into it while OEE Analysis covers performance and quality losses too. Both reports should reference the same period and the same bottleneck-line designation.
- **Pairs with Predictive Maintenance Report** — chronic Machine-category drivers on SPOF assets are seed candidates for PdM; this skill names them, the PdM skill builds the watch list and the sensor / signal plan.
- **Pairs with CAPA Document Builder** — any chronic countermeasure that touches a non-conformance or customer escape should be routed into a CAPA record with the verification metric carried forward.
- **Pairs with Shift Handoff Report** — today's top reason code on the bottleneck should appear as a priority item in tonight's handoff if not resolved.
- **Pairs with Work Instruction Generator** — Method-category drivers and "operator error" symptoms typically resolve into a DWI revision with an added gate or error-proofing.
- **Pairs with Training Plan & Skill Matrix** — Man-category drivers (especially second-shift / new-operator clusters) route to the qualification matrix and the recertification calendar.
- **Pairs with OT Cyber Incident Response** — coordinate event-window suppression so an OT event isn't double-counted as a chronic Machine driver.
- **Pairs with Supply Chain Risk Assessment** — Material-category chronic drivers on the bottleneck (substitute resin, surge alloy, off-spec coolant) seed the supplier risk register.

## Success Metrics

- **Time from event to first-pass review:** target same-shift for critical, next-day for routine.
- **Countermeasure close-out rate at committed date:** target 80%+; close only when the verification metric moves.
- **Bottleneck-line availability trend:** the bottleneck's top-3 Pareto items should visibly shrink quarter-over-quarter.
- **Pareto stability:** if the same reason code tops the bottleneck list for >4 weeks running, current countermeasures are not sufficient — escalate to a chronic-loss war-room.
- **Data-quality rate:** % of events with valid reason code and sub-code (target: 95%+); % with E10 state (target: 95%+ on E10-instrumented assets).
- **TPM pillar engagement:** every chronic top-3 driver has a named pillar owner.
- **SPOF coverage:** % of SPOF assets on the bottleneck line that are under PdM or have an explicit redundancy / spare strategy (target: 100%).
- **Plant-throughput-loss minutes:** bottleneck-line unplanned minutes (a stricter, more honest metric than plant-weighted availability) trending down.
