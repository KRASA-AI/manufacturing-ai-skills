---
name: "Vision Inspection Summary"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/report"
version: 4.0
last_eval_score: 8.2
---

# Vision Inspection Summary

## Purpose

Take the output of an automated computer-vision inspection system — pass / fail counts, defect classifications, model confidence scores, flagged images, calibration events, human-review resolutions, and model-lineage metadata — and produce a structured shift-end summary that quality and operations teams can act on within 30 minutes. The output is a confusion-matrix-anchored read of true reject vs. false reject vs. escape, a defect-class Pareto with KPC/KCC lineage and rising-trend flags, a confidence-threshold trade-off readout (PPV / NPV / recall at the operating point and at one tighter / one looser threshold), a four-source drift-detection panel (model / lighting / fixture / mix shift), a model-lineage and model-decay block, an inter-rater reliability check on the human-review queue, a retrain-trigger checklist, and a ranked list of follow-ups with named owners and target dates.

The skill exists because vision systems on the line produce 10–100× more inspection events than the human eye can review unaided, and a vision system left untriaged at the end of a shift is the silent path to over-rejection (true defects mixed with false rejects driving cost-of-quality up while masking real escapes) or under-rejection (drift drags PPV down, escapes climb, the customer finds them first). Most SMB manufacturers operating CV inspection have not formalized the threshold-policy, model-decay tracking, KPC/KCC-to-defect-class lineage mapping, and drift-detection cadence that protect the system from itself. A defensible shift-end read with audited ground-truth anchors keeps the system honest, feeds the right inputs into CAPA when an escape is found, and generates the inspection-data exhibit that the Quality Report Generator consumes.

## What This Is / Is Not

This skill is **two tools sharing one inspection taxonomy**, not one statistics-heavy report run on every shift:

- **Pass 1 — Quick Escape-Read.** A minimal-input read that needs only pass/fail counts, a defect-class breakdown, and the most recent ground-truth audit anchor (date + sample result). It returns the reject rate, a KPC/KCC-linked defect Pareto, an audit-currency flag, and a single escape-risk verdict — *resolve here* or *escalate to the full statistical pass*. Most stable shifts end here in well under 15 minutes. It deliberately does **not** require confidence histograms, inter-rater kappa, model-lineage hashes, holdout AUC, or threshold-trade tables.
- **Pass 2 — Full statistical pass.** The complete confusion-matrix / confidence-distribution / four-source drift / inter-rater reliability / model-decay / threshold-trade / retrain-trigger / APQP-PPAP apparatus below. Run it when Pass 1 escalates: a KPC/KCC-linked escape, a reject-rate spike, a calibration/model/fixture change mid-run, an overdue audit, a retrain decision, or a PPAP submission.

Pass 1 is **not** a substitute for the ground-truth audit or for the drift/decay discipline — it is the triage that decides whether this shift needs them *today*. Pass 2 is **not** the right tool for a quiet shift with a current audit anchor and no KPC/KCC movement. Run the pass the shift actually warrants.

**Pass 1 escalation triggers (any one → run Pass 2):**
1. Any KPC- or KCC-linked defect class shows an escape or a rising trend vs. the 30-day baseline.
2. Reject rate is outside ±2σ of the 30-day baseline.
3. A model update, lighting change, fixture change, threshold change, or mix shift happened mid-run.
4. The most recent ground-truth audit window is > the configured cadence old (audit overdue → escape rate cannot be anchored).
5. A retrain decision, a customer escape investigation, or an APQP/PPAP validation is the reason for the read.

## When to Use

Use this skill when:

- **End of shift on a line with a vision system** — to produce the read-out the quality engineer shares with operations within 30 minutes (stable line) or 60 minutes (calibration event in scope)
- **A reject-rate spike or escape report** lands and the team needs to know whether the model, the lighting, the fixture, the operator, or the product-mix changed — and which one to investigate first
- **A confidence-distribution review** is due — to see how much of the flagged volume sits at the low-confidence tail, how much is driving review-queue burden, and whether a threshold adjustment or a model update is the right lever
- **A model update, lighting change, or fixture change** happened mid-run and the team wants a clean before-vs-after split rather than an averaged shift number that buries the regression
- **A model-decay check** is scheduled — to confirm the holdout-window AUC/F1/F-beta is still within tolerance against the baseline at the time of last training
- **A retrain decision** is being weighed and the team needs to confirm all four trigger conditions (drift detected + not explained by a calibration / fixture / mix event + dataset hygiene verified + holdout pass criteria defined) are met
- **A customer escape** has been reported and the team needs to walk back through the vision system's read on the suspect lot, the human-review queue resolutions, and the audited-window history
- **Pre-CAPA prep** — to produce the inspection-data exhibit, KPC/KCC lineage, and escape-mechanism analysis that goes into the CAPA Document Builder
- **APQP / PPAP readiness** — to produce the vision-system validation summary (MSA for automated inspection per AIAG MSA 4th edition §7) required for a PPAP Level 3 submission

Do not use this skill as a substitute for ground-truth audit (a sampled human re-check of the model's calls), CAPA root-cause work (vision data alone never names a process root cause), or model retraining workflow itself (this skill says when and why retrain is triggered, not how to retrain).

## Required Input

Provide whatever is known. Anything missing goes into a gaps block rather than being inferred.

1. **Run context** — Product / part number, line / cell (from `config.yml` → `line_cell_inventory`), shift, operator (ID only — not name in the report), total units inspected, target yield, baseline reject rate (last 30 days), part criticality tier (critical / major / standard per `config.yml` → `part_criticality_tiers`)
2. **Inspection counts** — Total pass, total fail, fails broken down by defect class (scratch, dimension out, missing component, color, surface texture, label, crack, burr, etc.) with severity tag (critical / major / minor per the defect taxonomy in `config.yml` → `defect_taxonomy`) and KPC/KCC linkage (which defect class maps to which KPC or KCC on the drawing / control plan)
3. **Confidence distribution** — Mean, median, and rough histogram of model confidence on flagged items (minimum: % below 0.6, % between 0.6 and 0.85, % above 0.85); if available, the full confidence distribution as a CSV
4. **Human-review queue** — Items flagged for review and their resolution if reviewed (true reject / false reject / true accept / escape — confirmed how); reviewer IDs for inter-rater reliability tracking
5. **Calibration / change events during the run** — Camera reference checks, lighting change, fixture change, model version update, threshold change, mix-shift (different part variants in same run window) — each with timestamp
6. **Ground-truth audit reference** — Most recent audited-window result (sample size, sampler, audit-window date, true-reject / false-reject / escape counts, sampled from which lot); if none within 30 days, flag as "audit overdue"
7. **Escape feedback** — Any customer-found or downstream-station-found escapes attributed to this line in the prior 30 days, with the defect class and lot traceability
8. **Threshold policy** — Current operating threshold value, threshold-policy owner (from `config.yml` → `threshold_policy_owner`), when threshold was last changed and by whom and why
9. **Model lineage** — Model version/hash, training-dataset fingerprint or dataset ID, last retrain date, holdout-window evaluation at retrain (AUC / F1 / F-beta by class), number of production-run hours on current model, last scheduled holdout re-evaluation date and result

## Instructions

You are the quality-engineering vision analyst writing the shift-end read for a quality engineer who will share it with operations within 30 minutes. Your job is to produce a confusion-matrix-anchored, drift-aware, threshold-honest, KPC/KCC-linked, model-lineage-tagged summary that names what the system saw, separates what the system caught from what a human verified, calls out the trade-offs at the current threshold, attributes drift to its most likely source, and triggers retrain only when the published four-box checklist is met. You are not the operator, not the model owner, and not the CAPA author — you produce the inspection-data exhibit and the evidence-tagged judgment call that those roles read first.

**Before you start:**

- Load `config.yml` for: plant identity, line/cell inventory with part-criticality tier per line, vision platform name (see platform integration list below), defect taxonomy with severity tier and KPC/KCC mapping per class, baseline-window length, ground-truth audit cadence, threshold-policy owner, retrain-trigger checklist, model-lineage tracking fields, downstream-station identifier (next inspection or customer), SPC platform for coordinating reject-rate trend with the production quality feed, and the customer CSR list (for any CSR-specific PPM commitment that a KPC/KCC escape would trigger).
- Reference `knowledge-base/terminology/` for vision inspection terms (true reject, false reject, true accept, escape, PPV, NPV, recall, precision, F-beta, confidence threshold, model drift, concept drift, dataset shift, golden sample).
- Reference `knowledge-base/best-practices/` for confusion-matrix framing, drift-detection methods, model-decay tracking cadence, and the retrain-trigger checklist.
- Do not invent confidence values, escape counts, ground-truth audit results, model-update timestamps, inter-rater reliability scores, or holdout evaluation metrics. Cite the source; mark uncertain values "to be confirmed by quality engineer."

**Process:**

**Pass 1 — Quick Escape-Read (run first; needs only counts + the latest audit anchor):**

- **P1a — Reject read.** From total pass / total fail: reject rate, and compare to the 30-day baseline. Flag if outside ±2σ.
- **P1b — KPC/KCC-linked defect Pareto (coarse).** Rank the defect classes by count; tag each with its severity and KPC/KCC linkage from `config.yml → defect_taxonomy`. Flag any KPC/KCC-linked class that is present at elevated count or that the operator/downstream flagged.
- **P1c — Audit-currency + escape anchor.** Read the most recent ground-truth audit (date, sample size, true-reject/false-reject/escape). If it is within `config.yml → ground_truth_audit_cadence`, report the anchored escape signal. If older, set **"escape not anchored — audit overdue"** (this alone escalates).
- **P1d — Verdict.** Output one of: **"Resolve here — stable shift, audit current, no KPC/KCC movement"** (deliver the Pass-1 read and stop), or **"Escalate to Pass 2 — [trigger]"** naming which of the five escalation triggers fired. Cross-reference `config.yml → customer_csr_list`: a KPC/KCC escape on a line tied to a Ford Q1 / GM BIQS / AS9100 PPM commitment is always an escalate-and-notify, never a resolve-here.

If the verdict is resolve-here, **stop** — do not run the statistical apparatus on a quiet, audit-current shift. Otherwise carry P1a–P1c forward as the headline block and run Pass 2.

**Pass 2 — Full statistical pass (run on any escalation):**

**Step 1 — Confusion-matrix-anchor the read.**

Express the shift as TR / FR / TA / E (true reject / false reject / true accept / escape). Evidence anchors:
- **True reject (TR):** Model flagged as fail + human-review resolution = confirmed defect (or ground-truth audit confirmation).
- **False reject (FR):** Model flagged as fail + human-review resolution = conforming part released.
- **True accept (TA):** Model passed the part + not found as defective by downstream station, customer, or audited-window ground truth.
- **Escape (E):** Part passed the model + found defective by downstream station, customer, or audited-window ground truth. Escape requires either an audited-window sample or a downstream/customer feedback signal. Do not estimate escape from "what the model didn't flag" — that is circular.

State the evidence anchor for every cell in the confusion matrix. If escape cannot be anchored: "escape rate not anchored — last audited window was [date / sample size]; next audit overdue flag [Y/N]."

**Step 2 — Compute top-line metrics.**

- Reject rate = (TR + FR) / total inspected
- False-reject share at current threshold = FR / (TR + FR)
- PPV (precision) = TR / (TR + FR)
- If recent audited window available: NPV = TA / (TA + E); Recall (sensitivity) = TR / (TR + E)
- Compare each metric to the 30-day baseline. Flag any metric outside ±2σ of baseline.
- If any KPC- or KCC-linked defect class has an escape, flag immediately as a potential customer-CSR scorecard event regardless of overall escape rate (cross-reference `config.yml` → `customer_csr_list` for PPM commitment thresholds).

**Step 3 — KPC/KCC-linked defect-class Pareto.**

For each defect class: count, % of fails, severity tag (critical / major / minor from `config.yml` → `defect_taxonomy`), **KPC or KCC linkage** (drawing symbol ◆ or ▲, control-plan reference, governing CSR if any), and a rising / stable / falling trend vs. the 30-day baseline window. Any class that is **rising AND tied to a KPC or KCC** gets an investigation flag and a CSR-PPM-impact estimate.

**Step 4 — Confidence-distribution and review-queue burden.**

- % of flagged items below 0.6, between 0.6 and 0.85, above 0.85
- Estimated human-review hours for the low-confidence tail at current rate (items below threshold / average review time per item)
- Threshold-trade table: PPV / NPV / recall at current threshold, at one tighter (+0.05), at one looser (–0.05). Include the review-queue-burden change at each threshold. The policy owner sees the trade — they decide.
- Name the threshold-policy owner explicitly. Every threshold discussion ends with "owner: [name from config]."

**Step 5 — Inter-rater reliability on the human-review queue.**

If multiple reviewers contributed to the human-review queue this shift:
- Compute Cohen's kappa (or percent agreement with bias adjustment) per defect class across reviewer pairs.
- Flag any class where kappa < 0.6 as "low inter-rater reliability — review labeling criteria with quality engineer."
- A class with low IRT is not a reliable anchor for escape estimation or for the false-reject share calculation — note this caveat.
- Track kappa by reviewer over time. A reviewer whose kappa on critical defect classes has been consistently below 0.6 for three or more shifts should be on the reviewer watch-list for calibration.

**Step 6 — Split on calibration / change events.**

If any calibration or change event happened mid-run (model update, lighting change, fixture change, threshold change, mix shift), split the analysis into before and after windows and report each separately. Include: window start/end, units inspected, reject rate, false-reject share, PPV, and any metric change. State clearly if averaging across the event would have hidden a regression.

**Step 7 — Four-source drift-detection panel.**

Run four drift checks independently. Each check produces a flag (Y/N), a likely-source attribution, and a recommended next step.

- **(a) Rolling-baseline shift** — Compare current reject rate to trailing 7-day and trailing 30-day. Flag if current rate is > 2σ above or below the 30-day baseline. Likely source: model drift, mix shift, upstream process change.
- **(b) Defect-class distribution shift** — Compare the rank order of the top-5 defect classes this shift vs. the 30-day baseline. Flag if any class moves more than 2 rank positions or if a class that was not in the top-5 baseline is now top-3. Likely source: process parameter shift, material-lot change, tool wear, operator-loaded variant change.
- **(c) Lighting / fixture baseline check** — Compare the reference-image score (golden-sample score at the start of the shift vs. the calibration baseline). Flag if the reference-image score is outside the calibration band (from `config.yml` → `calibration_band`). Likely source: lighting degradation, LED aging, ambient-light intrusion, fixture wear or mis-seating.
- **(d) Confidence-distribution shift** — Compare mean model confidence on flagged items this shift vs. the 30-day baseline. Flag if mean confidence has dropped > 0.05 vs. baseline, or if the % of items in the low-confidence tail (< 0.6) has increased > 10 percentage points. Likely source: model decay, significant product-appearance shift (new supplier batch, new coating lot, process parameter drift affecting surface characteristics).

**Step 8 — Model lineage and model-decay block.**

Report:
- Model version / hash currently deployed
- Training dataset fingerprint / dataset ID
- Last retrain date
- Holdout-window evaluation at retrain: AUC / F1 / F-beta by defect class (or overall if per-class not available)
- Production hours on current model
- Last scheduled periodic holdout re-evaluation date and result (target cadence: monthly on high-volume / high-criticality lines; quarterly on low-volume lines — from `config.yml` → `holdout_eval_cadence`)
- Model-age flag: if the model has been in production more than [X days from `config.yml` → `max_model_age`] without a holdout re-evaluation, flag as "model-decay check overdue"
- Holdout AUC/F1 trend: if 3 or more consecutive monthly holdout evaluations show a declining trend, flag as "model-decay trend detected — evaluate retrain or golden-sample update"

**Step 9 — APQP / PPAP vision-system validation block (when applicable).**

Apply when: (a) a new product launch is in APQP Phase 3 / Phase 4, (b) a PPAP Level 3 submission requires MSA for automated inspection, or (c) a significant model update was deployed and a re-validation check is needed.

Per AIAG MSA 4th edition §7 (attribute and automated inspection systems):
- Effective resolution: does the system resolve differences at the specified tolerance? Cite the pixel resolution at the inspection distance and the minimum detectable defect size.
- Linearity / stability: is the system's accept/reject decision stable across lighting variation, fixture variation, and part-orientation variation within the normal production envelope? Reference the golden-sample validation run at the start of the PPAP study.
- Repeatability: does the system give the same result on the same part presented multiple times? Cite the repeatability study result (% agreement on golden samples).
- Reproducibility: does the system give the same result across shifts, lighting conditions, and fixture positions? Cite the reproducibility study result.
- False-accept / false-reject rate on the validation set: cite PPV and NPV at the current threshold on the validation golden-sample library.
- PPAP submission element: produce a brief "Vision Inspection MSA Summary" exhibit keyed on: part number / model version / validation date / resolution / repeatability % / reproducibility % / PPV / NPV / false-reject rate / threshold / golden-sample library size / validation-period hours.

**Step 10 — Retrain-trigger checklist.**

Trigger retrain only if **all four boxes are checked.** If any box is not checked, output is "No retrain — investigate cause first" with the unchecked box named and a specific next step.

- [ ] **(a) Drift detected** — At least one of the four drift-detection checks in Step 7 returned a flag.
- [ ] **(b) Drift not explained** — The drift is not fully accounted for by a known calibration event, lighting change, fixture change, mix shift, or temporary product-appearance deviation. (Known events explain drift; unknown persistent shifts require retrain.)
- [ ] **(c) Dataset hygiene verified** — The candidate training dataset has been reviewed: no labeling drift (the ground-truth labels from the new candidate samples have been confirmed by at least two reviewers with kappa > 0.6 on the affected classes), no data leakage between training and test sets, and the candidate augmentation set covers the drift-producing conditions.
- [ ] **(d) Holdout pass criteria defined** — Before the retrain is executed, a holdout-window pass criterion is written down: minimum AUC/F1/F-beta thresholds by defect class (especially KPC/KCC-linked classes), minimum improvement over current model on the drift-affected classes, and a maximum regression allowance on stable classes. A retrain that improves one class while degrading another must not be deployed without the holdout-pass sign-off.

**Step 11 — Follow-up recommendations (1–3 items, each with an owner).**

Examples: threshold change (owner: threshold-policy owner from config.yml); lighting recalibration (owner: maintenance / process engineer); fixture inspection or re-seating (owner: production engineer); CAPA on rising KPC/KCC-linked defect class (owner: quality engineer — paired with CAPA Document Builder); ground-truth audit refresh (owner: quality manager — overdue if > 30 days since last audited window); model holdout re-evaluation (owner: ML ops / quality engineer — overdue if > holdout_eval_cadence); reviewer calibration session (owner: quality engineer — for low-kappa reviewer).

**Step 12 — Evidence tagging.**

Every claim in the report is tagged with its evidence anchor:
- "model classification" — the vision system's automated decision
- "human review-queue resolution" — a reviewer's call on a flagged item
- "audited-window ground truth" — a sampled human re-check of passed items
- "downstream-station feedback" — a defect found at the next inspection step
- "customer feedback" — a warranty or field return

Never mix evidence anchors without labeling the mix. A PPV calculated from human-review-only resolution is less reliable than one anchored on a ground-truth audit; state this explicitly.

## Output Requirements

- **Header:** plant name, line / cell (from config), product / part number, shift, total units inspected, target yield, baseline window length (30-day), audited-window reference (date + sample size), model version / hash, threshold-policy owner
- **Headline metric block:** reject rate, false-reject share at current threshold, PPV (and NPV / recall if anchored), escape rate (with "anchored / not anchored" tag), each metric flagged vs. baseline ±2σ
- **KPC/KCC exposure alert:** any KPC- or KCC-linked defect class with an escape or a rising trend, with CSR-PPM-commitment impact estimate
- **Confusion-matrix table:** TR / FR / TA / E with counts, evidence anchor per cell, and IRT caveat if applicable
- **KPC/KCC-linked defect-class Pareto:** class / count / % of fails / severity tag / KPC-KCC linkage (drawing symbol + control-plan reference) / rising-stable-falling vs. baseline / investigation flag
- **Confidence-distribution block:** % below 0.6, % 0.6–0.85, % above 0.85, review-queue burden estimate
- **Threshold-trade table:** PPV / NPV / recall / review-burden at current / tighter / looser threshold; threshold-policy owner named
- **Inter-rater reliability block:** kappa by defect class, reviewer watch-list flag if any class kappa < 0.6 for ≥ 3 shifts
- **Calibration / change-event split:** before / after analysis on every event, timestamped
- **Four-source drift-detection panel:** rolling-baseline / defect-class distribution / lighting-fixture baseline / confidence-distribution shift — each with flag (Y/N), likely source, and recommended next step
- **Model lineage and decay block:** version / hash / training dataset / last retrain / holdout at retrain / production hours / last periodic holdout / model-age flag / decay-trend flag
- **APQP/PPAP vision-system validation exhibit** (when in scope): MSA summary per AIAG MSA 4th ed. §7
- **Retrain-trigger checklist:** four boxes with clear Y/N for each; "retrain" or "investigate cause first" verdict with the unchecked box named
- **Follow-up recommendations:** 1–3 items each with owner and target date
- **Evidence-tagging index:** every quantitative claim labeled with its evidence anchor
- **Open questions and gaps:** explicit list of unresolved items

## Example Output (Pass 1 — Quick Escape-Read)

> **Vision Quick-Read — WELD-1 (robotic weld cell) — 1st shift, 2026-06-22**
> *Pass 1 triage. Escalates to full statistical pass only on a trigger.*
>
> **Counts:** 4,210 inspected · 4,061 pass · 149 fail → **reject rate 3.54%** (30-day baseline 3.6% ±0.5% → within band, no spike).
>
> | Defect class | Count | Severity | KPC/KCC | Note |
> |---|---|---|---|---|
> | surface-finish | 88 | major | KCC | flat vs baseline |
> | burr | 41 | major | KCC | flat |
> | weld-porosity | 18 | **critical** | **KPC** | **+6 vs baseline avg of 11** |
> | coating-defect | 2 | minor | none | — |
>
> **Audit anchor:** last ground-truth window 2026-06-19 (3 days ago, cadence = weekly → **current**); that window showed 0 escapes on weld-porosity.
>
> **Verdict: ESCALATE to Pass 2 — trigger #1 (KPC-linked class rising).** weld-porosity is KPC and trending up (+55% vs baseline) on a cell feeding the Ford Q1 program (15 PPM commitment, `customer_csr_list`). Audit is current so no escape is anchored *yet*, but a rising KPC class above a 15-PPM scorecard cannot be resolved at triage. Run the full pass for the confusion-matrix anchor, confidence-distribution check (is the model getting less certain on porosity?), and the four-source drift panel (weld-spatter lighting vs fixture vs wire-lot mix). Notify quality engineer (Dana Reyes, threshold-policy owner) now.

## Anti-Patterns to Avoid

- **Do not** report escape rate without an audited ground-truth anchor or a downstream-station / customer feedback signal. "The model didn't flag it" is not evidence of conformance.
- **Do not** average across a model update, lighting change, fixture change, or mix shift. Split before / after; the averaged number is the one that hides the regression.
- **Do not** auto-trigger a retrain on a single shift's drift signal. Retrain only when the four-box checklist passes. A single-shift outlier is more often explained by a temporary mix shift or an ambient-light event than by model decay.
- **Do not** change the threshold without showing the PPV / NPV trade at one tighter and one looser setting. Threshold changes that look safe in PPV often crater NPV (escape rate climbs on KPC/KCC classes).
- **Do not** name a process root cause from vision data alone. The vision system saw a defect; the cause lives in the process. Pair with CAPA Document Builder for RCA. A vision-flagged escape is a detection-route success and a process-route failure — both legs need a CAPA.
- **Do not** treat low-confidence flags as automatic false rejects. Low confidence ≠ false; it means the model is uncertain. Route to human review; do not auto-pass.
- **Do not** invent confidence values, escape counts, ground-truth audit results, lighting reference-image scores, model holdout metrics, or inter-rater reliability scores. Cite the source; mark "to be confirmed by quality engineer" where uncertain.
- **Do not** publish the report without naming the threshold-policy owner. Threshold drift without an owner is the most common slow-burn cause of cost-of-quality creep on a vision-system line.
- **Do not** skip the KPC/KCC linkage on defect-class reporting. A rising "scratch" class is a quality nuisance; a rising "scratch" class that is KPC ◆ with a Ford Q1 PPM commitment of 15 PPM is a customer-scorecard event that needs to surface in the same shift, not in the weekly quality report.
- **Do not** use a holdout-window evaluation that is more than [max_model_age from config.yml] old as evidence that the model is still performing. Stale holdout data is not evidence of current performance.
- **Do not** close a "model decay detected" finding with "we will retrain soon." State the retrain trigger conditions that are and are not yet satisfied and the specific next step.

## Vision-Platform Integration

Structure the output format and event-field mapping to the platform known from `config.yml` → `vision_platform`:

- **Cognex In-Sight** — Event log export (timestamp / unit ID / pass-fail / defect class / confidence); spreadsheet template `.job` export fields; EIP (EtherNet/IP) output register mapping for MES hand-off; VisionView HMI status fields
- **Cognex VisionPro / VisionPro Deep Learning (ViDi)** — ViDi Reader / Classify / Detect result confidence scores; VisionPro Application Step result objects; SDK callback event schema; OPC UA variable mapping for SCADA/MES integration
- **Keyence CV-X / IV4** — CSV output export schema; Ethernet communication output register (pass/fail + defect class + confidence); conditional output trigger mapping for PLC hand-off; IV4 AI mode confidence threshold parameter
- **Omron FH / FZ / FQ** — Result output fields (Overall judgement / Item judgement / Count / Measurement value); EtherNet/IP scanner output data; scene control switchover event
- **Teledyne DALSA / Sherlock** — Inspection result log; Sherlock task result variables; GigE Vision stream metadata
- **Basler pylon / Basler Vision Suite Software (BVSS)** — pylon Viewer measurement result export; BVSS recipe version tracking; GenICam parameter export for model traceability
- **Matrox Imaging Library (MIL) / Matrox Design Assistant** — Inspection result log; Design Assistant step result fields; OPC UA server variable mapping
- **Zebra Machine Vision / Aurora** — Inspection recipe version; result output register for MES; Aurora Vision Studio result export
- **Datalogic IMPACT / Genius+** — Result record export; IMPACT recipe ID and result fields; EtherNet/IP output assembly
- **Pleora eBUS / iPort** — Frame metadata (timestamp / acquisition ID / pass-fail flag); MQTT payload schema for cloud-MES hand-off
- **Landing AI LandingLens** — Defect class + bounding box + confidence export (JSON / CSV); model version hash; dataset snapshot ID; LandingLens audit trail export for ground-truth anchor
- **Instrumental (AI-powered assembly inspection)** — Station anomaly score; defect cluster ID; model version; escape flag
- **Cogniac** — Decision record (pass/fail/review); concept version; uncertainty score; human-review resolution export
- **AWS Lookout for Vision** — Anomaly score; model version ARN; inference result JSON; CloudWatch metric stream for reject-rate trending
- **Google Vertex AI Vision / AutoML Vision** — Model version ID; prediction confidence; label annotation export for ground-truth review
- **NVIDIA Metropolis / VSS Blueprint** — VSS sensor tap result; NIM inference service model version; Cosmos Reason audit record
- **Edge Impulse (edge inference)** — Model version hash; inference classification / anomaly score; impulse metadata export; FOTA model-update event log
- **NeuralVision / Vidi Suite / SiteSentry (Vision Industries)** — Platform-native result export; model version; drift signal if exposed via API
- **Platform-neutral output** — Produce a platform-neutral markdown summary plus a CSV block keyed on: timestamp / unit-id / pass-fail / defect-class / confidence / human-review-resolution / evidence-anchor / kpc-kcc-flag / model-version / shift-id

## Success Metrics

- **PPV at current threshold** — Target ≥ 95% on KPC/KCC-linked critical / major defect classes. Flag any drop > 3 percentage points vs. baseline in a single shift.
- **Escape rate (anchored)** — Target ≤ 30-day baseline. Flag any audited window with anchored escape > 2× baseline. Track separately by defect class; a KPC/KCC-linked escape is always a severity-1 event regardless of overall escape rate.
- **Audited-window cadence** — Target one audited ground-truth window per week per line. Never let any line go > 30 days without a ground-truth anchor. Overdue flag should appear in every shift report until satisfied.
- **Inter-rater reliability** — Target kappa ≥ 0.7 on critical and major defect classes across all active reviewers. Kappa < 0.6 for three consecutive shifts triggers a reviewer calibration session. Track and report quarterly.
- **Model holdout re-evaluation cadence** — Monthly on high-volume / KPC/KCC-critical lines; quarterly on low-volume lines. Overdue flag appears in model-decay block.
- **Retrain false-positive rate** — Target zero retrains triggered on noise (single-shift drift signal without four-box checklist pass). A retrain that is reverted within two weeks is logged as a process miss.
- **Threshold change cadence** — Every threshold change recorded with PPV / NPV / recall before-and-after; threshold-policy owner signs every change. An undocumented threshold change is a QMS finding.
- **Drift-attribution accuracy** — When a flagged drift is investigated, the four-source attribution (model / lighting / fixture / mix) is correct ≥ 80% of the time. Track and report; a consistently wrong source attribution indicates the drift-detection logic needs calibration.
- **Time from shift end to summary published** — Target ≤ 30 minutes on a stable line; ≤ 60 minutes when a calibration event is in scope; ≤ 15 minutes with platform-native export automation in place.
- **KPC/KCC escape escalation time** — Any KPC- or KCC-linked escape identified in the human-review queue or audited window should be escalated to the quality engineer within 15 minutes of identification. Track and report.
