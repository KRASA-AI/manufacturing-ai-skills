---
name: "Predictive Maintenance Report"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/report + avoided-breakdown hours + shift-handoff alarm-row pull-through"
version: 4.0
last_eval_score: 8.7
---

# Predictive Maintenance Report

## Purpose

Turn raw condition-monitoring inputs — vibration, oil, infrared, motor current and stator flux, ultrasonic, partial-discharge, run-hours, and CMMS work-order history, augmented by vendor AI/ML classified alerts from Augury, SKF, Emerson, Senseye, Petasense, ABB, Schaeffler, IBM Maximo Predict, AVEVA Predictive Analytics, GE Digital APM, Bently Nevada System 1, Aspen Mtell, and AT&T Connected AI for Manufacturing — into a defensible predictive maintenance (PdM) report that (a) ranks assets by risk of functional failure, (b) assigns a remaining-useful-life (RUL) band and a P-F interval interpretation, (c) recommends a specific disposition per asset (run-to-failure / inspect / schedule / near-term / emergency), (d) updates the PM program with add / tighten / relax / retire decisions, (e) produces the spare-parts pull list the storeroom needs to have staged before the work order hits the floor, (f) explicitly delineates physics-grounded vs ML-classified alert provenance and runs the adversarial agreement check, (g) hands off the alarm row to the Shift Handoff Report's predictive-maintenance section, and (h) tracks the reliability program's own maturity KPIs distinct from individual-asset health.

## When to Use

Use this skill any time condition data needs to be turned into a maintenance decision, including:

- **Weekly PdM review meeting** with maintenance planner, reliability engineer, and operations — the standing artifact that drives work-order release
- **Post-route review** after a vibration / oil / thermography / ultrasonic / partial-discharge route is completed
- **Monthly reliability report** for plant leadership with top-risk assets and avoided-breakdown tally
- **Pre-shutdown planning** — rolling up condition data into a scoped shutdown scope
- **Critical-asset health check** on constraint equipment (CNC spindle, extruder, press, AHU, compressor, chiller, pumps, gearboxes, induction motors, robots, switchgear, transformers)
- **New-sensor validation window** — after installing wireless vibration, ultrasonic, or AT&T Connected AI for Manufacturing edge-AI sensors, the first 30–90 days of baseline before alarms are trusted
- **PM-task effectiveness review** — deciding whether a PM task is justified, over-scheduled, or should be replaced by condition-based triggers
- **AI/ML alert review** — when an Augury Halo, SKF @ptitude, Senseye PdM, Aspen Mtell, or AT&T Connected AI alert fires and the reliability engineer wants the physics-grounded second read before dispatching a work order
- **Reliability-program maturity assessment** — quarterly or semi-annual review of the program's own KPI set (sensor coverage, alert precision, save-event documentation, PM-task retirement, breakdown-rate trending)

This skill is explicitly a **decision-support** report. It does not auto-release work orders and does not replace a reliability engineer's read on a waveform — it sits between the sensor output (and the vendor AI/ML classifier output) and the planner's weekly meeting.

## Two-Pass Model

The full report below is a 13-step, seven-modality artifact that assumes a complete condition-monitoring route is already in hand. That is the right depth for the weekly PdM review — but it is too heavy when someone needs a fast read on "which assets should I worry about this week" before the full vibration/oil/IR/MCSA route data is collected. This skill therefore runs in two passes.

- **Pass 1 — Quick Failure-Risk Screen.** Minimal input: a coarse asset list (tag + class + criticality), last-PM / last-overhaul dates, and **one** available signal per asset (a single vibration overall, an oil flag, a thermography ΔT, a run-hours-since-overhaul number, *or* a vendor AI/ML alert). Returns a G/Y/O/R triage, the top-3 actionable assets, a provisional disposition band, and the explicit list of which assets must go to the full Pass 2. Built to run on whatever data exists right now.
- **Pass 2 — Full PdM Report.** The complete seven-modality decision artifact below: modality rubric, AI/ML adversarial agreement check, RUL P-F bands, 5×5 risk scoring, PM-program add/tighten/relax/retire, spare-parts pull list, reliability-program maturity KPIs, and the shift-handoff alarm rows. Run it on the assets Pass 1 escalated, not the whole plant.

**Use Pass 1 when** you have a coarse fleet view but not a full route — a Monday triage, a new-route first look, or a "is anything about to bite us before the shutdown" check. **Skip to Pass 2 when** the full multi-modality route is already collected on the assets of concern. Pass 1 never recommends a teardown on one reading — its strongest output is "inspect to confirm" or "collect the full route" (see escalation triggers).

### Pass 1 — Quick Failure-Risk Screen (run on three coarse inputs)

Required to start: (1) **Asset list** — tag, class, criticality tier (from `config.operations.line_cell_inventory` / asset registry); (2) **Last-PM / last-overhaul date** (or run-hours since overhaul); (3) **One signal per asset** — any single modality reading *or* a vendor AI/ML alert class.

Pass 1 returns:

```
QUICK FAILURE-RISK SCREEN — [date / route]

Asset triage (G/Y/O/R):
  | Asset tag | Class | Crit | One signal (what)      | Tag | Provisional disposition     |
  | WELD-1    | robot weld cell | critical | "Augury class-2 bearing wear" | O | Inspect to confirm — collect full route |
  | MILL-1    | 5-axis spindle  | critical | "vib overall 4.1 mm/s, was 2.8" | O | Trend up — full route Pass 2 |
  | LASER-1   | fiber laser     | major    | "last PM 95 days, cadence 90" | Y | PM overdue — schedule |

Top-3 actionable (criticality × signal):
  1. [asset] — [one-line why]
  2. ...
Provisional avoided-breakdown note: [modelled, not booked — coarse]

ESCALATE TO FULL PASS 2 (these assets need the seven-modality read):
  - [asset] — [trigger that fired]
```

Five escalation triggers that force a full Pass 2 (never resolve a teardown at the screen): (a) any **critical-tier** asset tagged O or R; (b) a **trend** signal (this reading worse than the last) on any asset; (c) a vendor **AI/ML alert** that has not had the physics-grounded second read; (d) a single reading already in **ISO 20816 zone C/D** or an IR ΔT > 20 °C / oil water > 200 ppm; (e) any asset inside a **committed production campaign** where an outage is unacceptable. An asset with none of the five and a clean single signal may legitimately stay at "monitor — no dispatch" on the screen alone.

Pass 1 inherits every anti-pattern below — most importantly: **do not alarm on a single reading**, and **do not accept an AI/ML alert without the physics check** (in Pass 1 an un-checked AI/ML alert is an automatic "inspect to confirm / escalate," never a dispatch).

## Config Pre-Population

Bind these `config.yml` keys so the standing fleet and platform facts are not re-asked each route:

| Output field | Config key |
|---|---|
| Asset list + criticality tiering | `operations.line_cell_inventory[]` (id, criticality) + asset registry |
| OEE / availability targets (consequence weighting) | `operations.oee_targets` |
| CMMS reorder payload target | `tools.cmms` (e.g. Fiix) |
| Historian / data source | `tools.historian` (e.g. Ignition) |
| AI/ML alert provenance vendors | PdM sensor-vendor list (config / asset registry) |
| Spare-parts ABC + OEM lead-time | `spare_parts_ABC` / `OEM_vendor_matrix` (if present) |
| Shift-handoff alarm-row routing | `operations.line_cell_inventory` tag match → Shift Handoff Report |
| Maintenance-tech-facing summary language | `voice` / `language_profile` |
| High-hazard assets (consequence escalation) | `ehs.high_hazard_processes` |

For the example config (`Summit Precision`), the critical-tier assets that auto-populate the screen are **WELD-1 (robotic weld cell)** and **MILL-1 (5-axis mill cell A)**; CMMS reorder payloads target **Fiix**; condition data sources resolve to **Ignition** historian; and any O/R disposition on WELD-1 or MILL-1 auto-routes a shift-handoff alarm row — none re-entered each cycle.

## Required Input

Provide the following. Anything missing goes into the gaps block, not a guess.

1. **Asset identifiers** — Asset tag / equipment ID, asset class (centrifugal pump, gearbox, induction motor, hydraulic power unit, compressor, CNC spindle, conveyor, robot, extruder, press, AHU, chiller, switchgear, transformer, etc.), criticality tier from `config.yml → asset_criticality_tiers`, parent system, location, ISO 20816 machine class (Part 1 general, Part 2 large gas turbines, Part 3 industrial-coupled machines, Part 4 wind turbine — pick the correct class for the asset)
2. **Run-hours and duty context** — Running hours since last overhaul, duty cycle (continuous, intermittent, standby), load profile, operating envelope this period (temperature, pressure, speed vs nameplate)
3. **Condition-monitoring readings** — One or more of:
   - **Vibration** — overall velocity in mm/s-RMS or in/s-peak at specified bearing locations, FFT spectrum highlights (1×, 2×, 3× RPM, bearing defect frequencies BPFO / BPFI / BSF / FTF if flagged), axial vs radial, phase if rotor balance was suspected. Flag the ISO 20816 zone (A new / B acceptable / C unsatisfactory limited duration / D damage imminent) for the correct machine class. Apply EWMA / CUSUM SPC overlay if available — trend acceleration distinct from level alarms
   - **Oil analysis** — wear metals in ppm (Fe, Cu, Al, Cr, Pb, Sn, Si, Ni, Mo), ISO 4406 cleanliness code (three-digit), ISO 11171 particle counting with gravimetric millipore-patch backup if the sample is contaminated, water content (ppm or %), viscosity at 40 °C and 100 °C vs nameplate, TAN / TBN where relevant, ferrous density (PQ index), Karl Fischer water if mineral oil, varnish potential (MPC)
   - **Infrared thermography** — max surface temp, delta-T above ambient / above reference phase / above sister component, hot-spot location, emissivity correction notes
   - **Motor current signature (MCSA / ESA)** — supply imbalance %, current unbalance, broken-bar sideband at 2·s·f_line around the line-frequency peak, air-gap eccentricity signatures, starting-current waveform notes
   - **Motor stator-side flux signature analysis (FSA)** — supplementary to MCSA for stator-winding-internal faults
   - **Ultrasonic** — dB level at bearings, valves, steam traps, with baseline comparison; partial-discharge ultrasonic for switchgear and motor windings
   - **Partial-discharge EMI** — for switchgear and motor windings — pC level and pulse-pattern interpretation
   - **Performance trending** — flow / head / DP / temperature rise / specific energy drifting outside control band; pump-curve drift; gearbox heat-rejection drift
4. **AI/ML alert payload** (if any) — vendor (Augury / SKF / Emerson / Senseye / Petasense / ABB / Schaeffler / IBM Maximo Predict / AVEVA / GE Digital APM / Bently Nevada / Aspen Mtell / AT&T Connected AI), classification (e.g., "bearing wear class 2", "imbalance moderate"), confidence band, training-data window on this asset class, drift status, last reliability-engineer override
5. **CMMS / maintenance history** — Past work orders (PM and corrective), prior overhaul dates, failure modes observed, replacement part numbers, labor hours, parts cost
6. **Planned production window** — Upcoming production schedule and any already-committed maintenance windows; critical campaign (automotive build, medical batch, aerospace lot) where outage is not acceptable
7. **Spare-parts status** — On-hand quantity of common wear parts, long-lead items, kitted spares if any, ABC-classified consumption forecast per `config.yml → spare_parts_ABC`, OEM-vendor lead-time band per `config.yml → OEM_vendor_matrix`
8. **Regulatory / safety drivers** — Pressure-vessel inspection dates (API 510 / ASME PCC), piping (API 570 / B31.3), risk-based-inspection driver (API 580 / 581), atmospheric storage tank (API 653), machinery (API 670 vibration), crane (OSHA 1910.179) and slings (1910.184), PSM (29 CFR 1910.119) for covered processes, NBIC boiler inspection (jurisdictional), IECEx and Class I Div 2 area-classification re-verification
9. **Reliability-program maturity context** — sensor-network coverage on this asset class, baseline-establishment maturity, alert-precision-tracking history, save-event-documentation cadence, PM-task-retirement-tracking record, breakdown-rate-trending-vs-modelled-avoided history

## Instructions

You are a reliability engineer writing a weekly PdM report that the maintenance planner will use to release work orders, that the reliability engineer will defend in the monthly plant-leadership review, and that the outgoing shift supervisor will pull into the Shift Handoff Report's predictive-maintenance alarm row. Your job is to turn readings — and vendor AI/ML classified alerts — into decisions without over-claiming what either source proves.

**Before you start:**
- Load `config.yml` for plant name, asset registry, asset criticality tiering, CMMS platform, PdM sensor vendors, AI/ML alert sources, oil-analysis lab, alignment-shop lab, OEM-vendor matrix, spare-parts ABC classification, breakdown-rate baseline, planned-maintenance window calendar, regulatory-inspection calendar, voice, and `language_profile` for the maintenance-tech-facing alert summary section
- Reference `knowledge-base/terminology/` for asset-class failure-mode libraries (pump cavitation, bearing defect frequencies, misalignment signatures, oil varnish potential MPC, motor rotor-bar cracking, gearbox tooth pitting, stator winding short-turn signatures, partial-discharge pulse-pattern signatures)
- Reference `knowledge-base/regulations/` for mandated inspection cadences (ASME B31.3 piping, API 510 / 570 / 580 / 581 / 653 / 670, OSHA 1910.179 cranes, 1910.184 slings, 1910.119 PSM, NBIC boiler jurisdictional inspections, IECEx and Class I Div 2 re-verification windows)
- Use voice from `config.yml → voice`; use `config.yml → language_profile` for the maintenance-tech-facing alert summary section translation when a language ≥ 10% of the maintenance crew triggers it

**Process:**

1. **Triage the route.** For each asset on the route, tag it GREEN (readings in spec, no trend), YELLOW (trend detected, watch), ORANGE (advisory — schedule), or RED (actionable — emergency or near-term). Do not skip to disposition before the tag.

2. **Apply the modality rubric.** Per reading:
   - **Vibration** — zone A (new / acceptable), B (acceptable — unrestricted), C (unsatisfactory — limited duration), D (unacceptable — damage imminent) per the correct ISO 20816 Part for the machine class (rigid vs flexible mount, power range; Part 1 general, Part 2 large gas turbines, Part 3 industrial-coupled machines, Part 4 wind turbine). Flag bearing defect frequencies by name if present. Apply EWMA / CUSUM SPC overlay to surface trend acceleration distinct from level alarms
   - **Oil** — compare wear metals against asset-class thresholds; cleanliness ISO 4406 against OEM spec (hydraulics typically 18/16/13, gearboxes 19/17/14, turbine 16/14/11); water > 200 ppm in mineral oil = action; ISO 11171 particle counting with gravimetric millipore-patch backup if the sample is contaminated; MPC varnish potential vs threshold; viscosity drift > 10% vs nameplate at 40 °C
   - **Infrared** — ΔT < 10 °C = monitor, 10–20 °C = schedule, 20–40 °C = near-term, > 40 °C = emergency for electrical connections; derate for asset class on mechanical bearings; emissivity-corrected only
   - **MCSA / ESA** — broken rotor-bar sideband > −35 dB relative to line-frequency peak = actionable; air-gap eccentricity threshold per `config.yml`
   - **FSA (flux signature)** — supplementary to MCSA for stator-winding-internal faults; threshold per asset class
   - **Ultrasonic** — dB at bearings vs baseline; valve-passing dB threshold; steam-trap pass/fail by waveform
   - **Partial-discharge EMI** — pC level vs threshold per switchgear / motor-winding class; pulse-pattern interpretation against discharge taxonomy
   - **Performance** — flag > 2σ drift from baseline and state the control-band source

3. **Apply the AI/ML alert adversarial review.** For every vendor AI/ML alert on the route (Augury Halo, SKF @ptitude, Senseye PdM, Aspen Mtell, AT&T Connected AI, etc.):
   - **State the provenance** — "Augury Halo classified bearing wear class 2 with N=12 weeks of training data on this asset class, drift status: stable, confidence band: 0.78"
   - **Run the physics-grounded second read** — "Do the frequencies and the oil sample agree with the bearing-wear hypothesis?"
   - **State the agreement** — physics-grounded ✓ AGREES with ML, ✗ DOES NOT AGREE with ML, or ? INSUFFICIENT DATA TO AGREE
   - **Honor the human-in-the-loop rule** — AI-classified alerts do not replace reliability-engineer review; an AI alert that disagrees with the physics check is a candidate for an inspect-to-confirm work order, not a teardown dispatch
   - **Track the alert precision / recall** — feed the disposition outcome (confirmed by inspection / not confirmed / inspection deferred) into the AI/ML alert-precision tracker for vendor calibration

4. **Estimate remaining useful life (RUL) as a band, not a number.** Use P-F-interval framing:
   - "P-F interval appears < 1 week" (emergency)
   - "P-F interval 1–4 weeks" (near-term schedule)
   - "P-F interval 1–3 months" (planned window)
   - "P-F interval > 3 months" (monitor — do not dispatch)
   - "No actionable signal — within normal variation"
   Say explicitly: *RUL bands are statistical, not guarantees. One reading does not establish trend. AI/ML RUL outputs are inputs, not adjudications.*

5. **Score each asset on a 5×5 risk matrix.** Likelihood (from the severity bucket above, refined by AI/ML alert if available and physics-agreed) × Consequence (from criticality tier in config + safety / environmental / quality / throughput exposure + regulatory deadline if applicable). Surface any Critical-tier asset in ORANGE or RED to the top of the report.

6. **Recommend a disposition per asset:**
   - **Run-to-failure (RTF)** — non-critical, redundant, low-consequence; explicitly accept the failure mode
   - **Schedule** — inside next planned window, with target date and crew
   - **Near-term schedule** — out-of-cycle work order, with target week
   - **Emergency** — within 24–72 hours, with safety / process-risk rationale
   - **Inspect to confirm** — when a single reading is anomalous, when the AI/ML alert disagrees with the physics check, or when trend is not established; send a second reading or a lube sample before dispatching a teardown
   - **Defer-pending-baseline** — for new sensors still in the 30–90 day baseline window

7. **Generate the Shift Handoff Report predictive-maintenance alarm-row payload.** For every ORANGE / RED disposition, generate the explicit handoff row the Shift Handoff Report v3.0 pulls into its Equipment Status section:
   - Asset tag
   - Modality (or modalities)
   - Alert class (level vs trend)
   - RUL band
   - Recommended disposition
   - Target work-order window
   - Crew assignment if known
   - One-line plain-English summary in the operations supervisor's language for the outgoing-shift's top-of-shift priorities scan
   The handoff row pulls into the Shift Handoff Report's Equipment Status section automatically when the asset tag matches `config.yml → asset_registry`.

8. **Update the PM program.** For each asset, name the PM task change if any:
   - *Add* a new task (e.g., add quarterly oil sample on gearbox G-402 after Fe trend)
   - *Tighten* cadence (move bearing relube from monthly to biweekly on wash-down duty)
   - *Relax* cadence (move cabinet filter change from monthly to quarterly — no dust loading)
   - *Retire* a task (replace time-based motor Megger with condition-based MCSA trigger)
   State the justification. This section is the single biggest cost/benefit lever in the report.

9. **Build the spare-parts pull list with CMMS reorder integration.** For every Schedule / Near-term / Emergency disposition, list part numbers, quantities, on-hand status (from config or last cycle count), lead time if not on hand, ABC consumption-forecast class, and whether a kitted spare exists. Long-lead items (bearings, couplings, seals, shafts, OEM-only parts) must be flagged with order-by date if the dispatch window is < lead time. Generate the CMMS-importable reorder payload (Maximo / SAP PM / Oracle eAM / Infor EAM / Fiix / Limble / UpKeep / eMaint / MicroMain / Hippo / Brightly / MVP One) per `config.yml → CMMS_platform`.

10. **Tally avoided breakdown cost.** For each asset moved from RTF to scheduled intervention, estimate (production throughput × gross margin per hour × expected breakdown hours) − (planned repair cost). Use config rates. Mark each estimate as "**modelled, not booked**" — do not present as booked savings.

11. **Run the six-stage reliability-program maturity assessment.** Track and report on the program's own KPI set:
    - **Stage 1 — Sensor-network coverage** — what fraction of the critical-asset list is instrumented and at what modality density
    - **Stage 2 — Baseline-establishment maturity** — how many sensors are out of the 30–90 day baseline window
    - **Stage 3 — Alert precision** — confirmed-by-inspection rate on ORANGE / RED alerts, tracked per modality and per AI/ML vendor
    - **Stage 4 — Save-event documentation** — count of save events documented this period with cost-avoidance estimates
    - **Stage 5 — PM-task retirement** — running tally of time-based PM hours retired vs added, with the net change
    - **Stage 6 — Breakdown-rate trending vs modelled-avoided** — rolling 12-month breakdown rate vs modelled avoided breakdown; a rising gap is the program's leading KPI
    Surface this as a standing section so the program's own maturity is visible distinct from individual-asset health.

12. **Add the regulatory-inspection deadline table.** Surface the 90-day-forward window for ASME B31.3 / API 510 / 570 / 580 / 581 / 653 / 670 / OSHA 1910.179 (cranes) / 1910.184 (slings) / 29 CFR 1910.119 PSM (covered processes) / NBIC boiler inspection (jurisdictional) / IECEx and Class I Div 2 area-classification re-verification. Each deadline carries an owner, target date, and current readiness flag.

13. **Gaps, assumptions, and data-quality block.** List every missing reading, every proxy, every first-cycle sensor still in baseline, every AI/ML alert that has not received the physics-grounded second read. Flag any asset where the reading was taken during atypical duty (startup, no-load, commissioning) and therefore does not reflect steady-state behavior.

## Output Requirements

- **Header** — plant, route / period, reliability engineer, planner, sensor vendors, AI/ML alert sources, CMMS platform, total assets on route
- **Top-of-report summary** — asset count by tag (green / yellow / orange / red), top three actionable assets, avoided-breakdown estimate band, one-line week-over-week direction, count of AI/ML alerts agreeing-with-physics vs disagreeing
- **Per-asset decision table** with: asset tag, class, criticality tier, tag (G/Y/O/R), modalities used, reading summary, severity per modality, AI/ML alert provenance and agreement status, RUL band, risk matrix score, disposition, target date, assigned crew
- **Top contributors narrative** — brief (1–3 sentences each) on the top 3–5 RED / ORANGE assets: what the reading showed, what the AI/ML alert classified, the physics-vs-ML agreement, the failure-mode hypothesis, and the recommended action. Use asset-class failure-mode language (e.g., "BPFO at 1.8× running speed with modulation — outer-race bearing defect hypothesis; Augury Halo classified bearing wear class 2, oil sample shows Fe rising — physics ✓ AGREES; schedule bearing replacement and oil sample at the same work order")
- **Shift Handoff Report alarm-row payload** — the per-asset handoff rows pulled into the Shift Handoff Report Equipment Status section
- **PM-program updates** — add / tighten / relax / retire table with asset, task, old cadence, new cadence, justification
- **Spare-parts pull list with CMMS reorder payload** — part numbers, quantities, on-hand, lead time, ABC class, order-by date if applicable, kitted-spare flag, CMMS-importable format per `config.yml → CMMS_platform`
- **Regulatory / inspection deadlines** — coming due in the next 90 days, with owner, target date, current readiness flag
- **Six-stage reliability-program maturity assessment** — sensor coverage, baseline maturity, alert precision per modality and per AI/ML vendor, save-event tally, PM-retirement net change, breakdown-rate-vs-modelled-avoided rolling 12-month
- **Gaps and assumptions** — every missing reading, every proxy factor, every first-cycle sensor still in baseline; every "no actionable signal" asset with explicit rationale; every AI/ML alert without physics-grounded second read
- **Avoided-breakdown estimate** with method shown; labelled "**modelled, not booked**"

## Anti-Patterns to Avoid

- **Do not** alarm on a single reading. Require either a trend (two or more readings in the same direction) or a physics-grounded explanation before recommending a teardown.
- **Do not** report RUL as a point estimate ("fails in 23 days"). Use P-F-interval bands.
- **Do not** claim root cause from vibration alone. Vibration patterns indicate hypotheses (misalignment, imbalance, looseness, bearing wear) that a physical inspection must confirm.
- **Do not** recommend an emergency outage on a critical asset without naming the specific safety, quality, or throughput risk. "Vibration is up" is not a shutdown justification.
- **Do not** promise the 10–40% breakdown-reduction figures from industry literature as if they are this plant's baseline. Report modelled avoided breakdown for this period with the math shown. Do not promise the AT&T Connected AI for Manufacturing 70% waste reduction or 2.5–4 hour pre-failure detection lead-time pilot figures as this plant's baseline — those are vendor pilot stats under controlled-test conditions, not steady-state plant baselines.
- **Do not** retire a PM task without reviewing failure-mode history on that asset class. Relaxing a task that was catching a real failure mode is how reliability programs quietly degrade.
- **Do not** present a wear-metal ppm number without the sampling context (hours-on-oil, top-up status, prior baseline). A Fe spike on a freshly-filled sump has a different meaning than the same spike on a 500-hour sump.
- **Do not** accept the vendor AI/ML alert without running the physics-grounded second read. Do not skip the AI/ML provenance disclosure — "the model said so" is not a defensible work-order justification. The human-in-the-loop sign-off rule is mandatory.
- **Do not** push a shift-handoff alarm row without a target work-order window. The shift supervisor needs to know when the work hits the floor.
- **Do not** skip the gaps block. The gaps block is the report's audit trail.

## Integration Notes

- **Pairs with Shift Handoff Report** — every RED / ORANGE disposition generates the explicit handoff row that the Shift Handoff Report v3.0 pulls into its Equipment Status section's predictive-maintenance alarm row. The handoff is automatic when the asset tag matches `config.yml → asset_registry`.
- **Pairs with Downtime Analysis Summary** — chronic reason codes in downtime analysis are candidates for condition-based monitoring additions here; conversely, assets repeatedly flagged here but not breaking ought to have their PM cadence relaxed.
- **Pairs with OEE Analysis Report** — availability loss at the line level traces back through this report to the specific asset and failure mode.
- **Pairs with Safety Incident & Near-Miss Report** — asset-failure incidents and near-misses create a direct input to asset criticality re-tiering.
- **Pairs with Supplier Communication Drafter** — lot-level failures (premature bearing, wrong-grade seal, contaminated coolant) trigger a SCAR via the `scar-8d-request` template.
- **Pairs with CAPA Document Builder** — repeat failures on the same asset class or repeat overhauls inside the MTBF band are a systemic issue, not a maintenance issue, and should open a CAPA. The Section 5.4 agentic-RCA workflow can absorb the predictive-maintenance physics + AI/ML evidence package.
- **Pairs with OT Cyber Incident Response** — sensor-network availability and integrity events (vendor cloud outage, sensor tamper, network anomaly on edge-AI deployments) cross-link to OT cyber incident response.
- **Pairs with Compliance Audit Prep** — regulatory-inspection deadlines (API / ASME / OSHA / NBIC / IECEx) flow into the audit-prep clause-by-clause evidence matrix.
- **PdM platform / AI vendor depth** — produce export and reference fields for: Augury (Halo), SKF @ptitude / Observer / SKF Insight Rail, Emerson AMS Machine Works / Plantweb, Fluke Reliability (eMaint + Azima DLI), Senseye PdM (Siemens), Petasense, ABB Ability Smart Sensor, Schaeffler OPTIME, IBM Maximo Application Suite + Predict, AVEVA Predictive Analytics, GE Digital APM, Bently Nevada System 1, Aspen Mtell, **AT&T Connected AI for Manufacturing** (the latter newly added per the 2026-06-01 landscape-monitor signal on AT&T GA). If the target system is known, produce its import fields. Otherwise produce platform-neutral YAML frontmatter plus a CSV-compatible block for the CMMS work-order importer.
- **CMMS platform depth** — IBM Maximo / Maximo Manage, SAP PM / EAM, Oracle eAM, Infor EAM, Fiix, UpKeep, Limble, eMaint, MicroMain, Hippo, Brightly, MVP One.

## Success Metrics

- **Alert precision** — target > 80% of RED / ORANGE dispositions confirmed by physical inspection (not false positives); track separately per modality and per AI/ML vendor
- **Physics-vs-ML alert agreement rate** — track the rate at which the physics-grounded second read agrees with the AI/ML classifier; a rising disagreement rate signals vendor model drift and feeds back to vendor calibration
- **Lead time on save** — target ≥ 1 week between the first actionable reading and the functional failure for near-term dispositions
- **PM-program efficiency** — target net reduction in time-based PM hours (retire + relax > add + tighten) within 12 months of PdM program maturity, *without* an increase in breakdown rate
- **Avoided-breakdown tally** — modelled quarterly, reconciled against actual breakdown history; a rising gap between modelled-avoided and actual-breakdown trend is the program's KPI
- **Save-event documentation completeness** — target 100% of confirmed saves documented with cost-avoidance estimate, reliability-engineer narrative, and disposition trace
- **Shift-handoff alarm-row pull-through rate** — target 100% of RED / ORANGE dispositions surface in the next Shift Handoff Report's predictive-maintenance alarm row; gaps indicate a handoff failure to investigate
- **Spare-parts pull-list accuracy** — > 95% of parts on the pull list are correct and in stock by dispatch date
- **Regulatory inspection on-time-completion rate** — > 98% of API / ASME / OSHA / NBIC / IECEx deadlines completed on or before the due date; surfaces in the regulatory-inspection-deadline table
- **AI/ML alert calibration** — vendor AI/ML alert precision and recall tracked per asset class per quarter; precision target ≥ 0.75 and recall ≥ 0.7 at the calibrated threshold; results feed back to the vendor as part of the contract review
- **Report cycle time** — under 2 hours from route completion to published report for the weekly PdM review
- **Reliability-program maturity** — six-stage assessment tracked quarter-over-quarter; the program's own KPI distinct from individual-asset health

## Example Output

**Worked Pass-1 Quick Failure-Risk Screen** (example config: Summit Precision; Monday triage, partial data — one signal per asset, full route not yet collected):

```
QUICK FAILURE-RISK SCREEN — 2026-06-29, Plant-1 critical-asset Monday read

Asset triage:
  | Asset tag | Class            | Crit     | One signal (what)                       | Tag | Provisional disposition          |
  | WELD-1    | robotic weld cell| critical | Augury Halo "bearing wear class 2", no physics check yet | O | Inspect to confirm — collect full route |
  | MILL-1    | 5-axis spindle   | critical | vib overall 4.1 mm/s (was 2.8 last route)| O  | Trend up — escalate to Pass 2     |
  | TURN-1    | turning cell     | major    | oil Fe 18 ppm, baseline ~12              | Y  | Watch — resample with hours-on-oil context |
  | LASER-1   | fiber laser      | major    | last PM 95 d (cadence 90 d)              | Y  | PM overdue — schedule, not condition-driven |
  | BRAKE-1   | press brake line | standard | IR delta-T +6 C on motor                 | G  | Monitor — within normal           |

Top-3 actionable (criticality x signal):
  1. MILL-1 — 5-axis spindle vibration up 46% route-over-route; critical-tier constraint cell.
  2. WELD-1 — vendor AI flagged bearing wear class 2; un-verified by physics -> inspect-to-confirm.
  3. LASER-1 — time-based PM overdue (administrative, not a failure signal).

Provisional avoided-breakdown note: MILL-1 unplanned spindle loss ~ constraint stop —
  modelled, not booked; coarse — confirm in Pass 2.

ESCALATE TO FULL PASS 2:
  - MILL-1 — triggers (a) critical-tier O + (b) trend signal. Collect FFT (1x/2x, bearing
    defect freqs), oil sample, and axial/radial vib for the seven-modality read.
  - WELD-1 — trigger (c) un-checked AI/ML alert on a critical asset. Pull the waveform and an
    oil sample to run the physics-vs-ML agreement check before any work order.
```
>
> Note how the screen ran on **one signal per asset** plus criticality and last-PM dates — no full route — yet correctly separated a genuine trend (MILL-1) from an administrative PM-overdue (LASER-1) from a vendor alert that must not be actioned without the physics check (WELD-1). Neither critical asset is dispatched for teardown on the screen; both are escalated to Pass 2. Run the full seven-modality process above on the two escalated assets.
