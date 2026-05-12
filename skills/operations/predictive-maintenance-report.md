---
name: "Predictive Maintenance Report"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/report + avoided-breakdown hours"
version: 2.0
last_eval_score: 8.7
---

# Predictive Maintenance Report

## Purpose

Turn raw condition-monitoring inputs — vibration, oil, infrared, motor current, run-hours, and CMMS work-order history — into a defensible predictive maintenance (PdM) report that (a) ranks assets by risk of functional failure, (b) assigns a remaining-useful-life (RUL) band and a P-F interval interpretation, (c) recommends a specific disposition per asset (run-to-failure / schedule / emergency), (d) updates the PM program with add / tighten / relax / retire decisions, and (e) produces the spare-parts pull list the storeroom needs to have staged before the work order hits the floor. The report is the single artifact that lets a maintenance planner go from "our sensors say something" to "here are this week's work orders, ranked, with parts, with windows."

## When to Use

Use this skill any time condition data needs to be turned into a maintenance decision, including:

- **Weekly PdM review meeting** with maintenance planner, reliability engineer, and operations — the standing artifact that drives work-order release
- **Post-route review** after a vibration / oil / thermography route is completed
- **Monthly reliability report** for plant leadership with top-risk assets and avoided-breakdown tally
- **Pre-shutdown planning** — rolling up condition data into a scoped shutdown scope
- **Critical-asset health check** on constraint equipment (CNC spindle, extruder, press, AHU, compressor, chiller, pumps, gearboxes)
- **New-sensor validation window** — after installing wireless vibration or ultrasonic sensors, the first 30–90 days of baseline before alarms are trusted
- **PM-task effectiveness review** — deciding whether a PM task is justified, over-scheduled, or should be replaced by condition-based triggers

This skill is explicitly a **decision-support** report. It does not auto-release work orders and does not replace a reliability engineer's read on a waveform — it sits between the sensor output and the planner's weekly meeting.

## Required Input

Provide the following. Anything missing goes into the gaps block, not a guess.

1. **Asset identifiers** — Asset tag / equipment ID, asset class (centrifugal pump, gearbox, induction motor, hydraulic power unit, compressor, CNC spindle, conveyor, robot, extruder, press, AHU, chiller, etc.), criticality tier from config, parent system, location
2. **Run-hours and duty context** — Running hours since last overhaul, duty cycle (continuous, intermittent, standby), load profile, operating envelope this period (temperature, pressure, speed vs nameplate)
3. **Condition-monitoring readings** — One or more of:
   - **Vibration**: overall velocity in mm/s-RMS or in/s-peak at specified bearing locations, FFT spectrum highlights (1×, 2×, 3× RPM, bearing defect frequencies BPFO / BPFI / BSF / FTF if flagged), axial vs radial, phase if rotor balance was suspected. Flag the ISO 10816 / ISO 20816 zone (A/B/C/D)
   - **Oil analysis**: wear metals in ppm (Fe, Cu, Al, Cr, Pb, Sn, Si), ISO 4406 cleanliness code (three-digit), water content (ppm or %), viscosity at 40 °C vs nameplate, TAN / TBN where relevant, ferrous density (PQ index)
   - **Infrared thermography**: max surface temp, delta-T above ambient / above reference phase / above sister component, hot-spot location
   - **Motor current signature (MCSA / ESA)**: supply imbalance %, current unbalance, broken-bar sideband at 2·s·f_line around the line-frequency peak, air-gap eccentricity signatures, starting-current waveform notes
   - **Ultrasonic**: dB level at bearings, valves, or steam traps with baseline comparison
   - **Performance trending**: flow / head / DP / temperature rise / specific energy drifting outside control band
4. **CMMS / maintenance history** — Past work orders (PM and corrective), prior overhaul dates, failure modes observed, replacement part numbers, labor hours, parts cost
5. **Planned production window** — Upcoming production schedule and any already-committed maintenance windows; critical campaign (automotive build, medical batch, aerospace lot) where outage is not acceptable
6. **Spare-parts status** — On-hand quantity of common wear parts, long-lead items, kitted spares if any
7. **Regulatory / safety drivers** — Pressure-vessel inspection dates, elevator certification, API / ASME compliance windows, boiler inspections, crane load tests

## Instructions

You are a reliability engineer writing a weekly PdM report that the maintenance planner will use to release work orders and that the reliability engineer will defend in the monthly plant-leadership review. Your job is to turn readings into decisions without over-claiming what the readings prove.

**Before you start:**
- Load `config.yml` for plant name, asset registry path, CMMS system (Fiix / UpKeep / Limble / eMaint / IBM Maximo / SAP PM / Oracle eAM / Infor EAM), PdM sensor vendors (Augury, Fluke / eMaint, Senseye, SKF, Emerson AMS, Petasense, Aspen Tech), criticality tiering scheme, and preferred severity language
- Reference `knowledge-base/terminology/` for asset-class failure-mode libraries (pump cavitation, bearing defect frequencies, misalignment signatures, oil varnish, motor rotor-bar cracking, gearbox tooth pitting)
- Reference `knowledge-base/regulations/` for mandated inspection cadences (ASME B31.3 piping, API 510 pressure vessels, API 570 piping, crane OSHA 1910.179, boiler jurisdictional inspections)
- Use voice from `config.yml` → `voice`

**Process:**

1. **Triage the route.** For each asset on the route, tag it GREEN (readings in spec, no trend), YELLOW (trend detected, watch), ORANGE (advisory — schedule), or RED (actionable — emergency or near-term). Do not skip to disposition before the tag.

2. **Apply the modality rubric.** Per reading:
   - **Vibration** — zone A (new / acceptable), B (acceptable — unrestricted), C (unsatisfactory — limited duration), D (unacceptable — damage imminent) per ISO 20816 for the machine class (rigid vs flexible mount, power range). Flag bearing defect frequencies by name if present.
   - **Oil** — compare wear metals against asset-class thresholds; cleanliness ISO 4406 against OEM spec (hydraulics typically 18/16/13, gearboxes 19/17/14, turbine 16/14/11); water > 200 ppm in mineral oil = action.
   - **Infrared** — ΔT < 10 °C = monitor, 10–20 °C = schedule, 20–40 °C = near-term, >40 °C = emergency for electrical connections; derate for asset class on mechanical bearings.
   - **MCSA** — broken rotor-bar sideband > −35 dB relative to line-frequency peak = actionable.
   - **Performance** — flag > 2σ drift from baseline and state the control-band source.

3. **Estimate remaining useful life (RUL) as a band, not a number.** Use P-F-interval framing:
   - "P-F interval appears < 1 week" (emergency)
   - "P-F interval 1–4 weeks" (near-term schedule)
   - "P-F interval 1–3 months" (planned window)
   - "P-F interval > 3 months" (monitor — do not dispatch)
   - "No actionable signal — within normal variation"
   Say explicitly: *RUL bands are statistical, not guarantees. One reading does not establish trend.*

4. **Score each asset on a 5×5 risk matrix.** Likelihood (from the severity bucket above) × Consequence (from criticality tier in config + safety/environmental/quality exposure). Surface any Critical-tier asset in ORANGE or RED to the top of the report.

5. **Recommend a disposition per asset:**
   - **Run-to-failure (RTF)** — non-critical, redundant, low-consequence; explicitly accept the failure mode
   - **Schedule** — inside next planned window, with target date and crew
   - **Near-term schedule** — out-of-cycle work order, with target week
   - **Emergency** — within 24–72 hours, with safety / process-risk rationale
   - **Inspect to confirm** — when a single reading is anomalous but trend is not established; send a second reading or a lube sample before dispatching a teardown

6. **Update the PM program.** For each asset, name the PM task change if any:
   - *Add* a new task (e.g., add quarterly oil sample on gearbox G-402 after Fe trend)
   - *Tighten* cadence (move bearing relube from monthly to biweekly on wash-down duty)
   - *Relax* cadence (move cabinet filter change from monthly to quarterly — no dust loading)
   - *Retire* a task (replace time-based motor Megger with condition-based MCSA trigger)
   State the justification. This section is the single biggest cost/benefit lever in the report.

7. **Build the spare-parts pull list.** For every Schedule / Near-term / Emergency disposition, list part numbers, quantities, on-hand status (from config or last cycle count), lead time if not on hand, and whether a kitted spare exists. Long-lead items (bearings, couplings, seals, shafts) must be flagged with order-by date if the dispatch window is < lead time.

8. **Tally avoided breakdown cost.** For each asset moved from RTF to scheduled intervention, estimate (production throughput × gross margin per hour × expected breakdown hours) − (planned repair cost). Use config rates. Mark each estimate as "modelled" — do not present as booked savings.

9. **Gaps, assumptions, and data-quality block.** List every missing reading, every proxy, every first-cycle sensor still in baseline. Flag any asset where the reading was taken during atypical duty (startup, no-load, commissioning) and therefore does not reflect steady-state behavior.

## Output Requirements

- **Header:** plant, route / period, reliability engineer, planner, sensor vendors, CMMS system, total assets on route
- **Top-of-report summary:** asset count by tag (green / yellow / orange / red), top three actionable assets, avoided-breakdown estimate band, one-line week-over-week direction
- **Per-asset decision table** with: asset tag, class, criticality tier, tag (G/Y/O/R), modalities used, reading summary, severity per modality, RUL band, risk matrix score, disposition, target date, assigned crew
- **Top contributors narrative** — brief (1–3 sentences each) on the top 3–5 RED/ORANGE assets: what the reading showed, the failure-mode hypothesis, and the recommended action. Use asset-class failure-mode language (e.g., "BPFO at 1.8× running speed with modulation — outer-race bearing defect hypothesis; schedule bearing replacement and oil sample at the same work order")
- **PM-program updates** — add / tighten / relax / retire table with asset, task, old cadence, new cadence, justification
- **Spare-parts pull list** — part numbers, quantities, on-hand, lead time, order-by date if applicable, kitted-spare flag
- **Regulatory / inspection deadlines** coming due in the next 90 days (pressure vessels, cranes, boilers, elevators)
- **Gaps and assumptions** — every missing reading, every proxy factor, every first-cycle sensor still in baseline; every "no actionable signal" asset with explicit rationale
- **Avoided-breakdown estimate** with method shown; labelled "modelled, not booked"

## Anti-Patterns to Avoid

- **Do not** alarm on a single reading. Require either a trend (two or more readings in the same direction) or a physics-grounded explanation before recommending a teardown.
- **Do not** report RUL as a point estimate ("fails in 23 days"). Use P-F-interval bands.
- **Do not** claim root cause from vibration alone. Vibration patterns indicate hypotheses (misalignment, imbalance, looseness, bearing wear) that a physical inspection must confirm.
- **Do not** recommend an emergency outage on a critical asset without naming the specific safety, quality, or throughput risk. "Vibration is up" is not a shutdown justification.
- **Do not** promise the 10–40% breakdown-reduction figures from industry literature as if they are this plant's baseline. Report modelled avoided breakdown for this period with the math shown.
- **Do not** retire a PM task without reviewing failure-mode history on that asset class. Relaxing a task that was catching a real failure mode is how reliability programs quietly degrade.
- **Do not** present a wear-metal ppm number without the sampling context (hours-on-oil, top-up status, prior baseline). A Fe spike on a freshly-filled sump has a different meaning than the same spike on a 500-hour sump.
- **Do not** skip the gaps block. The gaps block is the report's audit trail.

## Integration Notes

- **Pairs with Shift Handoff Report** — any RED disposition should echo into the outgoing-shift's 🚨 critical-items block so the incoming shift supervisor is not surprised by a work-order release.
- **Pairs with Downtime Analysis Summary** — chronic reason codes in downtime analysis are candidates for condition-based monitoring additions here; conversely, assets repeatedly flagged here but not breaking ought to have their PM cadence relaxed.
- **Pairs with OEE Analysis Report** — availability loss at the line level traces back through this report to the specific asset and failure mode.
- **Pairs with Safety Incident & Near-Miss Report** — asset-failure incidents and near-misses create a direct input to asset criticality re-tiering.
- **Pairs with Supplier Communication Drafter** — lot-level failures (premature bearing, wrong-grade seal, contaminated coolant) trigger a SCAR via the `scar-8d-request` template.
- **Pairs with CAPA Document Builder** — repeat failures on the same asset class or repeat overhauls inside the MTBF band are a systemic issue, not a maintenance issue, and should open a CAPA.
- Most mid-market sites feed this skill from one of: Augury (Halo), SKF @ptitude / Observer, Emerson AMS Machine Works, Fluke Reliability (eMaint + Azima), Senseye PdM (Siemens), Petasense, ABB Ability Smart Sensor, or CMMS-native condition modules in Fiix / Limble / UpKeep / Maximo / SAP PM. If the target system is known, produce its import fields. Otherwise produce platform-neutral markdown plus a CSV-compatible block for the CMMS work-order importer.

## Success Metrics

- **Alert precision:** target > 80% of RED / ORANGE dispositions confirmed by physical inspection (not false positives)
- **Lead time on save:** target ≥ 1 week between the first actionable reading and the functional failure for near-term dispositions
- **PM-program efficiency:** target net reduction in time-based PM hours (retire + relax > add + tighten) within 12 months of PdM program maturity, *without* an increase in breakdown rate
- **Avoided-breakdown tally:** modelled quarterly, reconciled against actual breakdown history; a rising gap between modelled-avoided and actual-breakdown trend is the program's KPI
- **Spare-parts pull-list accuracy:** > 95% of parts on the pull list are correct and in stock by dispatch date
- **Report cycle time:** under 2 hours from route completion to published report for the weekly PdM review
