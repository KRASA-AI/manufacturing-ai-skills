---
name: "OEE Analysis Report"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~75 min/report"
version: 2.0
last_eval_score: 8.8
---

# OEE Analysis Report

## Purpose

Take a period's Overall Equipment Effectiveness (OEE) data — availability, performance, quality, and the underlying loss events from the OEE platform of record (Aveva PI / OSIsoft, GE Proficy, Siemens Insights Hub, AspenTech IP.21, Ignition, Tulip, MachineMetrics, Plex Asset Performance, Fiix, Maximo APM, FactoryTalk Metrics) — and produce an honest, prioritized analysis that names the bottleneck-line OEE before the plant-average OEE, separates the Six Big Losses correctly, anchors against the per-line target the plant actually owns rather than a generic "world-class 85%" headline, ties each top-3 driver to a TPM pillar so the countermeasure has a home, and lands two or three actions someone on the floor can start on Monday.

The skill exists because most plant-level OEE reports either (a) chase a global 85% target that is wrong for non-bottleneck lines and impossible for some equipment classes, or (b) collapse the Six Big Losses into a single "downtime" bucket that hides the real lever. The bar is the report a plant manager will read in five minutes, an operations VP will trust in a quarterly review, and a TPM steering committee will use to assign autonomous-maintenance / planned-maintenance / focused-improvement / early-equipment-management ownership.

## When to Use

Use this skill when:

- **Weekly or monthly OEE review** is on the calendar and the team needs more than the dashboard read-out
- **A line is missing its OEE target by >2 points for two weeks running** and the gap analysis is the next step
- **A bottleneck-line OEE drops** even though plant-average looks fine — bottleneck-line OEE governs throughput; non-bottleneck OEE does not
- **A new product ramp** is underway and the ramp-curve OEE needs to be separated from steady-state OEE
- **A capital-justification or capacity-add decision** depends on whether OEE recovery can close the gap before adding equipment
- **A TPM steering committee** is assigning autonomous-maintenance / planned-maintenance / focused-improvement / early-equipment-management ownership and needs the loss-Pareto-by-pillar view
- **An OEE benchmarking conversation** is happening — internal site-to-site or external against an industry quartile — and the comparison needs to be apples-to-apples on definition (planned-stop carve-out, ideal-cycle-time anchor, scrap-vs-rework treatment)
- **A customer audit, MOD-fac visit, or VOC-driven capacity confirmation** asks for a documented OEE story for a specific line / cell / value stream
- **A continuous-improvement war room** opens for a chronic loss and the report needs to separate "Pareto-stable chronic" from "spiked assignable"

## Required Input

Provide the following. Anything missing should be listed in the gaps block — never invented.

1. **Period and scope** — Date range, shift coverage, line / cell / asset list in scope, planned non-production carve-out convention (lunch, breaks, scheduled maintenance, no-demand hours)
2. **OEE components** — Availability %, performance %, quality %, overall OEE for each in-scope line / cell / asset for the period and the prior comparable period
3. **Ideal cycle time** — The ideal cycle time per critical SKU per line (this is the denominator for performance %; getting this wrong is the most common source of inflated or deflated OEE numbers)
4. **Planned-stop and unplanned-stop event log** — Each event with line / asset, start time, duration, planned vs unplanned classification, reason code, sub-code, free-text note, responding function
5. **Scrap, rework, and startup-rejects log** — Quantities and reason codes (scrap is a different cost basis from rework; startup rejects after a changeover or a cold start are a different mechanism from steady-state quality fallout)
6. **Per-line targets** — The OEE target the plant actually commits to for each line (these vary by equipment class — discrete assembly typically 75–85%, continuous chemical / process commonly 90%+, low-volume / high-mix job shops commonly 50–65%, batch food / pharma 60–80%)
7. **Bottleneck-line designation** — Which line is the system bottleneck for the in-scope value stream(s); bottleneck-line OEE matters more than plant-average OEE
8. **Known active issues** — Open CAPAs, PM-schedule gaps, pending spare-parts orders, new-product ramps, recent operator turnover, recent ECN / process-change effective dates that contextualize the period
9. **OEE platform of record** — Aveva PI / OSIsoft, GE Proficy, Siemens Insights Hub, AspenTech IP.21, Ignition, Tulip, MachineMetrics, Plex Asset Performance, Fiix, Maximo APM, FactoryTalk Metrics, custom MES, etc. — needed for the data-quality discussion and for the recommended-export field set
10. **TPM pillar ownership map** — Which named function or person owns autonomous maintenance (operators), planned maintenance (maintenance), focused improvement (CI / engineering), early equipment management (engineering / projects), quality maintenance (quality), education and training (training), safety / health / environment (EHS), TPM in administration (finance / planning)

## Instructions

You are a continuous-improvement engineer producing the OEE analysis the plant will discuss in the operations review. Your audience is the plant manager (5-minute scan), the operations VP (10-minute read in a regional review), and the TPM steering committee (deeper read for ownership assignment). Write to all three.

**Before you start:**

- Load `config.yml` for plant name, OEE platform (`operations.oee_platform`), per-line OEE targets (`operations.oee_targets` — keyed by line), planned-stop taxonomy (`operations.planned_stops` — what the site treats as planned vs unplanned by convention), shift pattern (`shift.pattern`), ideal cycle time per critical SKU (`operations.ideal_cycle_time` — keyed by line × SKU), bottleneck-line designation per value stream (`operations.bottleneck_line`), TPM pillar ownership (`tpm.pillar_owners`), and communication tone (`voice.tone`)
- Reference `knowledge-base/terminology/` for correct OEE / TPM / lean vocabulary (Six Big Losses, Nakajima OEE definition, world-class anchor, TPM eight pillars, autonomous maintenance, planned maintenance, focused improvement, early equipment management, MTBF, MTTR, hidden-factory loss, Pareto, Ishikawa, 5-Why, A3, kaizen)
- Reference `knowledge-base/best-practices/` for the Theory-of-Constraints throughput hierarchy (bottleneck OEE governs throughput; non-bottleneck OEE does not), Goldratt's "an hour lost on the bottleneck is an hour lost on the system" rule, and the planned-stop carve-out conventions used in IATF 16949, AS9100D, and FDA QSR plants
- Confirm the baseline window the plant uses — common conventions are prior 4-week rolling, prior month, prior fiscal quarter, or prior model-year cohort

**Process:**

1. **Set the boundary explicitly.** Lines / cells / assets in scope, period start / end, shift coverage, planned-stop carve-out convention used (and whether it matches the IATF / AS9100 / FDA convention the customer audit expects). State whether the report is a snapshot or a rolling view.
2. **Validate OEE math before reporting it.** OEE = availability × performance × quality. Availability = run time / planned production time. Performance = (ideal cycle time × total count) / run time. Quality = good count / total count. If the components do not multiply to the reported overall OEE within rounding, flag the discrepancy as a data-quality issue; do not silently re-derive.
3. **Build the bottleneck-first headline.** The headline is the bottleneck-line OEE, the bottleneck-line target, the delta, and the bottom-line throughput consequence — not the plant-average OEE. Plant-average OEE goes in the body, not the headline. Anchor the per-line targets against the line's own equipment-class benchmark from `config.yml` rather than a global 85%.
4. **Build the planned-vs-unplanned cut as the first structural slice.** Planned stops (changeovers, scheduled PMs, breaks, no-demand) and unplanned stops (breakdowns, minor stops, idling) are different countermeasure populations. A report that blends them will mislead. Show both cuts.
5. **Build the Six Big Losses Pareto.** Allocate every loss minute and every loss part to one of the six (planned stops / breakdowns / setup-and-adjustment / minor-stops-and-idling / reduced-speed / startup-rejects-and-rework / production-rejects). Some practitioners count seven; the skill names planned stops as the seventh and treats it as a planned-vs-unplanned discussion item rather than a countermeasure target.
6. **Compute MTBF and MTTR per top contributor.** A loss with few events and long duration (high MTTR) needs a different countermeasure from a loss with many events and short duration (low MTBF). Name the mechanism so the countermeasure population is right (high MTTR → response time, spare-parts kit-out, technician availability, condition-monitoring escalation; low MTBF → failure-mode focused improvement, autonomous-maintenance routine, design-out via early equipment management).
7. **Tag chronic vs assignable.** A loss that is Pareto-stable across weeks is chronic; a loss that spiked tied to a specific event (a PM miss, a new operator, a bad lot, a tool wear event, a calibration drift) is assignable. Assignable gets a one-time fix; chronic gets a structural countermeasure tied to the right TPM pillar.
8. **Map the top-3 drivers to TPM pillars.** Each top-3 driver is assigned to one of the eight TPM pillars (autonomous maintenance / planned maintenance / focused improvement / early equipment management / quality maintenance / education and training / safety, health, environment / TPM in administration) with the named pillar owner from `config.yml`. A driver without a clear pillar home is a finding — either the pillar inventory is incomplete or the driver is novel and needs a focused-improvement pass to define ownership.
9. **Run a short 5-Why for the #1 driver.** Not a full RCA — a 5-Why stub that points toward the most credible hypothesis and explicitly notes which "why" is verified from the event log and which is still inference pending floor verification. Mark a confidence level (high / medium / low) on the conclusion and state what would raise it.
10. **Inventory the hidden-factory loss.** The gap between the model OEE (component-multiplied) and the theoretical OEE (ideal-cycle-time × planned-production-time × yield-target) is the hidden factory. Most plants under-report it; the report names it explicitly and ties it to the planned-stop carve-out convention, the ideal-cycle-time anchor, and the scrap-vs-rework treatment.
11. **Recommend two or three countermeasures.** Each must be concrete (who, what, when, how verified), tied to a hypothesized cause, scoped to effort tier (quick win / kaizen / engineering change / capital), assigned to the TPM pillar that owns the cause, and carry a hypothesized OEE-point recovery with a verification metric / threshold / window. Do not recommend more than three — optionality kills execution.
12. **Be honest about uncertainty.** Where the data is ambiguous, say so. Where the platform is missing a code, say so. Where the operator-typed reason code conflicts with the maintenance technician's note, surface the conflict — do not silently pick one.
13. **Reconcile the action register against the prior period.** Every prior-period countermeasure is closed (with verification evidence) or rolled forward (with reason). A countermeasure rolled twice without closure is a Major-effectiveness concern and is escalated to the action-register-owner's manager.

## Output Requirements

```
OEE ANALYSIS — <Plant / Value stream> — <Period>
Author / approver / config.yml version

HEADLINE
- Bottleneck line: <line ID> — OEE <%> (target <%>, delta <±pts>, prior <%>)
- Throughput consequence: <units / shift, hours of demand> at the gap
- Plant-average OEE: <%> (context only — bottleneck OEE governs throughput)
- Top driver this period: <Six-Big-Loss category — name — minutes — % of unplanned>
- Bottom line in one sentence

PLANNED vs UNPLANNED VIEW
- Planned downtime: <hours> — <changeovers / PMs / breaks / no-demand>
  - Flag any planned-stop creep (changeover-duration drift, PM overruns, no-demand misclassification)
- Unplanned downtime: <hours> — focus of the countermeasures section

OEE COMPONENT BREAKDOWN — by line
| Line | Avail % | Perf % | Qual % | OEE % | Target % | Δ vs target | Δ vs prior | Bottleneck? |
|---|---|---|---|---|---|---|---|---|

SIX BIG LOSSES PARETO (unplanned + planned-stop creep)
| Rank | Loss category | Six-Big bucket | Events | Total min | % of period loss | MTBF | MTTR | Chronic / Assignable | TPM pillar | Pillar owner |
|---|---|---|---|---|---|---|---|---|---|---|

Pareto cutoff (80% of period loss): reached at rank <N> — the vital few.

5-WHY STUB (top contributor only)
1. Why did the line lose this output?  → <direct trigger, verified from event log>
2. Why did that trigger?               → <mechanism, cite source — event detail / operator note / maintenance note>
3. Why did that happen?                → <underlying condition, flag as inference if not verified>
4. Why has it persisted?               → <system-level — missing PM, spec, training, design>
5. Why does the system allow it?       → <governance / process gap>

Confidence: <high / medium / low> — <what would raise it>
Floor-verification gaps: <what would need to be checked>

HIDDEN-FACTORY LOSS INVENTORY
- Theoretical OEE (ideal cycle × planned production × yield target): <%>
- Model OEE (component-multiplied): <%>
- Gap (hidden factory): <pts> — <attribution: planned-stop carve-out, ideal-cycle-time anchor, scrap-vs-rework treatment>

COUNTERMEASURES (≤3, ranked by leverage)
1. <Title>
   - Effort tier: quick win / kaizen / engineering change / capital
   - TPM pillar / pillar owner: <pillar — named owner>
   - Hypothesized cause addressed: <which 5-Why step / which Six-Big bucket>
   - Hypothesized OEE-point recovery: <pts on bottleneck line>
   - Verification: <metric, threshold, window>
   - Owner: <name>   Target date: <ISO 8601>
2. <Title> ...
3. <Title> (only if leverage is clear) ...

PRIOR-PERIOD ACTION REGISTER
- Closed (with verification evidence pointer)
- Rolled forward (with reason and new due date)
- Effectiveness-concern flags (rolled twice or more — escalation owner)

WATCHLIST (not yet countermeasures — needs more data)
- <Suspicious patterns that are not yet actionable>

DATA-QUALITY NOTES
- <Events missing reason codes, operator-typed vs auto-captured discrepancies, planned-stop misclassification, ideal-cycle-time drift, platform-export caveats>

INTERNAL RATIONALE (for the approver — not for distribution)
- Period / scope / planned-stop convention applied
- Bottleneck-first framing applied
- TPM pillar mapping completeness check
- Confidence on top-driver root cause
- Cleared-facts gate triggered: yes / no — if yes, named owner

GAPS (only present if mandatory inputs missing)
- <Gap 1> — owner to confirm
- <Gap 2> — owner to confirm
```

## Anti-Patterns to Avoid

- **Do not** anchor every line against a global 85% world-class target. World-class anchor is per equipment class (continuous process commonly 90%+, discrete assembly 75–85%, low-volume / high-mix 50–65%, batch food / pharma 60–80%). Use the per-line target from `config.yml`.
- **Do not** lead with plant-average OEE. The headline is bottleneck-line OEE; plant-average is body context. Plant-average OEE rising while bottleneck OEE falls is the system getting worse.
- **Do not** blend planned and unplanned stops in the Pareto. The countermeasure populations are different. Show both cuts.
- **Do not** chase 100% utilization on non-bottleneck lines. Goldratt's TOC explicitly warns that 100% non-bottleneck utilization builds inventory rather than throughput; the right target on non-bottleneck lines is "feeds the bottleneck without starving or blocking it."
- **Do not** report a single "downtime" bucket. Allocate to the Six Big Losses; a missing allocation is a data-quality finding, not an editing problem.
- **Do not** silently re-derive OEE when the components do not multiply to the reported overall. Flag the discrepancy as a data-quality issue and name the source system.
- **Do not** cite "operator error" as a root cause. That is a symptom of a method, training, error-proofing, or design gap; the right pillar is education-and-training or focused-improvement, not "talk to the operator."
- **Do not** recommend more than three countermeasures. Optionality kills execution.
- **Do not** claim root cause from event-log data alone. The log gives you hypotheses; the floor gives you causes. Mark the 5-Why confidence and name the floor-verification gap.
- **Do not** leave a top-3 driver without a TPM pillar home. A driver without an owner is a driver without a fix.
- **Do not** report a hidden-factory gap of zero. If the model OEE equals the theoretical OEE, the ideal-cycle-time anchor is wrong, the planned-stop carve-out is too generous, or the yield target is too loose — flag the suspect input.
- **Do not** roll a prior countermeasure forward more than two cycles without escalation. The default behavior on a third roll is a manager-level effectiveness review.
- **Do not** present any operator-typed reason code as fact when a connected-device reading or maintenance-technician note conflicts with it. Surface the conflict.

## Integration Notes

- **Pairs with Downtime Analysis Summary** — downtime is the availability component; this skill drills the full OEE picture (availability + performance + quality), Downtime Analysis goes deeper on the unplanned-stop event log.
- **Pairs with Predictive Maintenance Report** — chronic machine-category drivers in the OEE Pareto seed the PdM candidate list; the planned-maintenance pillar owner uses both reports as inputs.
- **Pairs with Production Scheduling Optimizer** — the bottleneck-line designation in this report drives the scheduler's bottleneck-first rule; an OEE recovery on the bottleneck reshapes the schedule's promise dates.
- **Pairs with CAPA Document Builder** — countermeasures that rise to non-conformance scope flow into the CAPA register; the verification metric / threshold / window is the CAPA's effectiveness-check gate.
- **Pairs with Shift Handoff Report** — today's top loss driver is a hot-list item in tonight's handoff if not resolved; the Six-Big-Loss category and the chronic / assignable tag carry through.
- **Pairs with Vision Inspection Summary** — the quality-component fallout in OEE that traces to vision-system rejects is reconciled against the Vision Inspection Summary's true-reject / false-reject / escape split before it is allocated to startup-rejects-and-rework or production-rejects.
- **Pairs with Compliance Audit Prep** — IATF 16949 §9.1.1.1, AS9100D §9.1.1, ISO 13485 §8.2.5 process-monitoring evidence pulls cite the OEE record; output formats are aligned so the report can be dropped into the audit-prep clause map without restructuring.
- **OEE platform integration:** if the target platform is known from `config.yml`, produce the recommended-export field set (Aveva PI tags, MachineMetrics machine-IDs, Tulip table schema, Plex APM key fields, FactoryTalk Metrics activity codes). Otherwise produce platform-neutral markdown plus a CSV block.

## Personalization

The skill reads:

- `config.yml` → `company.name`, `site.name`
- `config.yml` → `operations.oee_platform`
- `config.yml` → `operations.oee_targets` (per line)
- `config.yml` → `operations.planned_stops` (planned-stop taxonomy / convention)
- `config.yml` → `operations.ideal_cycle_time` (per line × critical SKU)
- `config.yml` → `operations.bottleneck_line` (per value stream)
- `config.yml` → `shift.pattern`
- `config.yml` → `tpm.pillar_owners` (named owner per pillar)
- `config.yml` → `voice.tone`

Without these, the skill outputs a draft with the placeholders flagged in the rationale block — never a draft that fabricates the values.

## Success Metrics

- **Bottleneck-line OEE trend** — quarter-over-quarter improvement on the bottleneck line is the headline KPI
- **Top-3 Pareto stability** — if the same Six-Big bucket tops the list for >4 weeks, current countermeasures are insufficient
- **Countermeasure close-out rate at committed date** — target ≥80%
- **Roll-twice-or-more rate** on open countermeasures — target ≤10%
- **Hidden-factory gap** — target a measurable downward trend (the gap is real and large in most plants; a flat zero is a data-quality finding, not a win)
- **Data-quality rate** — target ≥95% of loss minutes with valid reason code; the unallocated bucket is a leading indicator of report quality
- **Audit pull-through rate** — the OEE record is usable as IATF / AS9100 / ISO 13485 process-monitoring evidence with no further editing
