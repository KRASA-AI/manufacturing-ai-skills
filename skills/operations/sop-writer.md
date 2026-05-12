---
name: "SOP Writer"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~45 min/SOP"
version: 3.0
last_eval_score: 9.1
---

# SOP Writer

## Purpose

Turn process notes, operator interviews, or tribal knowledge into a controlled, audit-ready Standard Operating Procedure that functions as the governing document for a manufacturing process — complete with document-control header, safety callouts, PFMEA-linked quality checkpoints, KPC/KCC designations, PPE requirements, regulated-industry framing, QMS-platform export structure, and an ECN-triggered requalification handoff.

The SOP Writer is the upstream half of the controlled-document / digital-work-instruction pair — the SOP is the governing document that specifies the process requirements; the Work Instruction Generator produces the operator-execution artifact for the shop floor. Both must match in revision, approval, and scope. A change to either without updating the other is a document-control finding.

## When to Use

Use this skill whenever you need to:

- Create a new SOP for new equipment, new process, new cell, or new product family
- Rewrite an informal work procedure or "tribal knowledge" process into a controlled document
- Remediate a document-control finding in an ISO 9001 / IATF 16949 / AS9100 / ISO 13485 / FDA 21 CFR 820 / 211 audit
- Drive a CAPA-required procedure update (SOP revision as a corrective action)
- Cross-train operators on a process that currently lives in one person's head
- Generate an ECN-driven procedure update where a drawing or specification revision requires corresponding SOP revision and operator requalification
- Create a QMS-platform-native document ready for upload to MasterControl / ETQ Reliance / Plex / Greenlight Guru / Sparta TrackWise / IQS / Veeva Vault QMS / AssurX / SAP QM

## Required Input

Provide the following. Mark any item unknown as "TO BE COMPLETED" — never fabricate procedural detail.

1. **Process name and scope** — What operation is being documented? Which line, cell, or workstation (match the ID in `config.yml` → `line_cell_inventory`)? Which product family or part-number range?
2. **Step-by-step content** — Raw notes, bullet points, operator interview transcript, or existing draft. Include both the "what" (actions) and the "why matters" (so hazard tags and quality checkpoints can be placed correctly).
3. **Equipment and tooling** — Machine asset IDs (from `config.yml` → `asset_registry`), fixture numbers, gauge numbers with calibration-status requirement, consumable part numbers
4. **Materials and inputs** — Raw materials, components, drawing numbers with revision, applicable specifications
5. **Safety hazards** — Known hazards (pinch points, hot surfaces, chemical exposure, electrical, ergonomic, crush, fall, stored energy). Reference the JSA/JHA for this task if available.
6. **Quality requirements** — Critical-to-quality (CTQ) characteristics, KPC/KCC designations from drawing or PFMEA, inspection points, acceptance criteria, gauge types, SPC requirements if any
7. **Document metadata** — SOP number, revision, owner/process engineer name, reviewer, approver, effective date, supersedes revision; change description (if a revision, not initial release)
8. **Regulated-industry context** — From `config.yml` → `qms_framework` (ISO 9001 / IATF 16949 / AS9100D / ISO 13485 / FDA 21 CFR 820 / 211 / GAMP 5) — determines which regulatory clauses appear in the header and which additional fields are required
9. **QMS platform** — From `config.yml` → `qms_platform` — determines the output format for document-control upload

## Instructions

You are a manufacturing quality engineer writing a controlled document that must pass an ISO 9001 / IATF 16949 / AS9100 / FDA audit and be clear enough for a new operator to follow on day one. Your job is to produce a document that is both legally defensible as audit evidence and actually functional on the shop floor — those two requirements are not in conflict when the structure is right.

**Before you start:**

- Load `config.yml` for: company name, OSHA establishment ID, QMS framework (ISO 9001 / IATF 16949 / AS9100D / ISO 13485 / 21 CFR 820 / 211), QMS platform name, customer CSR list (for KPC/KCC regulated-step flagging), connected-worker platform (for DWI handoff reference), line/cell inventory, asset registry, PFMEA reference (if available), language/accommodation profile of the operator pool (for readability target and translation triggers), and the requalification workflow platform.
- Reference `knowledge-base/regulations/` for applicable regulatory clauses. Key references: ISO 9001:2015 §7.5 (documented information), IATF 16949:2016 §7.5.3 / §8.5.1.4 / §8.5.1.6 (manufacturing process design + work instructions), AS9100D §7.5 / §8.5.1, ISO 13485:2016 §4.2.3 / §7.5.1, FDA 21 CFR 820.40 / 820.70 (medical devices), FDA 21 CFR 211.100 / 211.180 (pharma), GAMP 5 Category 3–4 (configured / custom-programmed platforms in regulated industries), OSHA 29 CFR 1910 for LOTO / PPE / HAZCOM / confined-space references.
- Reference `knowledge-base/terminology/` for correct process terminology.

**Process:**

**Step 1 — Controlled-document header.**

Every SOP must carry a complete controlled-document header before the body begins. Use this exact structure, populated from the user's input and `config.yml`:

```
STANDARD OPERATING PROCEDURE

Document No:    [SOP-XXXX or company convention from config.yml → doc_numbering]
Title:          [Process name — specific enough to search for in a QMS platform]
Revision:       [Rev letter or number]      Effective Date: [YYYY-MM-DD]
Supersedes:     [prior revision identifier, or "N/A — Initial Release"]
ECN Reference:  [Engineering Change Number if this revision is ECN-driven, or "N/A"]
Owner:          [Name, Title, Line/Cell (from config.yml → line_cell_inventory)]
Reviewed By:    [Name, Title]               Date: [YYYY-MM-DD]
Approved By:    [Name, Title]               Date: [YYYY-MM-DD]
QMS Framework:  [ISO 9001 §7.5 / IATF 16949 §7.5.3 / AS9100D §7.5 / ISO 13485 §4.2.3 /
                 FDA 21 CFR 820.40 / 21 CFR 211.100 — select all applicable from config.yml]
Language(s):    [Primary language + any parallel-language versions generated]
```

**Step 2 — Content sections.**

Write each section using the required structure below. Every procedure step is a single, imperative action. One step = one action. Number sequentially.

```
1. PURPOSE
   [1–2 sentences: what this SOP accomplishes, why it exists, and which regulatory
   requirement it satisfies (e.g., "in accordance with IATF 16949 §8.5.1.4").]

2. SCOPE
   [Which product family, part-number range, line, shift, cell, or operation
   this applies to — and what is explicitly OUT of scope. Name the line/cell
   ID from config.yml → line_cell_inventory.]

3. RESPONSIBILITIES
   - Operator: [what they do — execution and real-time quality checks]
   - Lead / Supervisor: [setup verification, deviation disposition]
   - Quality Engineer: [checkpoint sampling, SPC monitoring]
   - Maintenance / Process Engineer: [equipment-specific responsibilities if any]
   - Document Control / QA Manager: [approval, revision control, distribution]

4. PERSONAL PROTECTIVE EQUIPMENT (PPE)
   Required at all times on this line:
   - [items — safety glasses, steel-toe boots, hearing protection, etc.]
   Required for specific steps (see callouts):
   - [Step X: add face shield. Step Y: add cut-resistant gloves level A4+, etc.]
   PPE authority: OSHA 29 CFR 1910.132(d) hazard-assessment requirement;
   reference JSA/JHA document number [from operator's task / work-order reference].

5. EQUIPMENT AND MATERIALS
   Equipment:
   - [Machine name, Asset ID from config.yml → asset_registry, gauge number with
     calibration frequency and last-cal date placeholder]
   Materials / Components:
   - [Part number, drawing number and revision, applicable specification]
   Consumables:
   - [Cutting fluid spec, adhesive type, lubricant grade, etc.]

6. SAFETY PRECAUTIONS
   - LOTO requirements: [Hazardous energy sources — electrical / pneumatic /
     hydraulic / stored mechanical / chemical — with OSHA 29 CFR 1910.147
     reference and the energy-isolation procedure document number]
   - Chemical exposure: [SDS reference by product name and SDS number;
     OSHA 29 CFR 1910.1200 HAZCOM requirement]
   - Ergonomic / repetitive-motion exposure: [if applicable]
   - Emergency stop location: [physical location or HMI E-stop ID]
   - Nearest first-aid station: [location]
   - Nearest eyewash / emergency shower: [location if chemicals present]

7. PROCEDURE

   7.1  PRE-START GATE
        Before any production begins on this SOP, the operator must verify:
        □  Work order scanned / production order confirmed in [ERP/MES from config.yml]
        □  Drawing revision on traveler matches the current approved revision [Rev X]
        □  Calibration sticker on [gauge ID] current (next due: _____)
        □  PPE in place per Section 4
        □  LOTO applied per Section 6 (if applicable)
        □  5S station check: no foreign objects, tooling in designated positions,
           previous job material removed and quarantined

   7.2  SETUP
        1. [Imperative step — single action]
        2. [Imperative step — single action]

        >> SAFETY [DANGER / WARNING / CAUTION / NOTICE]: [Hazard — signal word
           choice per ANSI Z535.4: DANGER = death/serious injury likely if not
           avoided; WARNING = death/serious injury possible; CAUTION = minor injury
           possible; NOTICE = property/quality damage, no injury risk]
           Mitigation: [specific action or guard]

        3. [Imperative step]

   7.3  OPERATION
        4. [Imperative step]

        >> QUALITY CHECKPOINT [KPC ◆ / KCC ▲ / Standard ●]: Check [characteristic]
           using [gauge / method / SPC chart].
           Accept: [criterion — tolerance, visual standard, SPC rule].
           Reject: [action — quarantine per NCR procedure, initiate CAPA ticket,
                    call quality engineer before continuing].
           Basis: [Drawing note / control plan reference / PFMEA line item]
           If SPC: [chart type — XbarR / XbarS / IMR / p / np / c / u; plot point
                    before proceeding if sampling plan requires]

        5. [Imperative step]

        >> QUALITY CHECKPOINT ●: ...

   7.4  SHUTDOWN / CHANGEOVER
        [Imperative steps — include end-of-run count verification, last-part
        inspection hold, gauge return to calibrated storage, workstation 5S
        restore, production-traveler sign-off]

8. QUALITY AND ACCEPTANCE CRITERIA
   KPC / KCC designations (from control plan [CP-XXXX Rev X] and PFMEA
   [PFMEA-XXXX Rev X]):
   - [Characteristic: KPC ◆ | Tolerance | Gauge | Frequency | Reaction plan ref]
   - [Characteristic: KCC ▲ | Specification | Method | Frequency | Reaction plan ref]
   Sampling plan:
   - [100% / AQL X.X per [ANSI/ASQ Z1.4] / SPC subgroup n=X every Y pieces]
   Nonconforming material handling:
   - [Reference NCR / MRB procedure by document number; no-decision-at-operator rule]

9. CHANGE CLASSIFICATION (for revisions only)
   This section determines whether an ECN and PPAP re-submission are required.
   Apply the following matrix based on the nature of the change:

   | Change Type | Examples | ECN Required? | PPAP Trigger? | Requalification? |
   |---|---|---|---|---|
   | Cosmetic / editorial | Spelling, formatting, non-technical wording | No | No | No |
   | Minor procedural | Sequence clarification, tool ID update, no parameter change | Optional | No | Notify operators |
   | Significant procedural | New step, deleted step, changed acceptance criterion | Yes | Evaluate per AIAG PPAP 4th ed. §2.2 | Full requalification |
   | Engineering change (process parameter, material, tooling, equipment) | Set-point change, new fixture, new material, new gauge | Yes, with DRE review | Full PPAP re-submission under customer CSR | Full requalification |
   | Regulatory-driven | FDA 21 CFR change, OSHA mandate, CSR update | Yes | Evaluate per quality framework | Full requalification |

   Regulated-industry supplement:
   - **ISO 13485 / FDA 21 CFR 820.40 / 820.75:** All changes that affect conformance to specified requirements require documented change control with impact assessment, validation re-review, and design history file update.
   - **FDA 21 CFR 211.180:** Reprocessing or specification changes require comparison against validation protocol; significant changes require site master file / ANDA / NDA supplement notification.
   - **GAMP 5 Cat 3–4:** Configured or custom-programmed platforms require change impact assessment against the validated system; impact = High requires full re-validation; impact = Low requires regression test documentation.
   - **IATF 16949 §8.5.1.4 / §8.3.6:** Manufacturing process design changes require updated control plan, PFMEA, work instructions, and operator training records before production.

10. RECORDS
    [Which forms are completed during this SOP and where they are filed. Examples:
    production traveler (work order system), inspection log (QMS platform / paper),
    setup verification sheet (document number), SPC chart entry (SPC platform from
    config.yml → spc_platform), calibration log (CMMS from config.yml → cmms_platform).]

11. REFERENCES
    - Drawing(s): [number, revision]
    - Control Plan: [CP number, revision]
    - PFMEA: [PFMEA number, revision]
    - Related SOPs: [document numbers — setup SOP, cleaning SOP, maintenance SOP]
    - Customer CSR(s): [applicable CSRs from config.yml → customer_csr_list with clause references for any KPC/KCC steps]
    - Regulatory standards: [ISO 9001 §7.5, IATF §7.5.3, etc. — per QMS framework]
    - JSA/JHA: [document number for this workstation's hazard assessment]
    - NCR / MRB Procedure: [document number]
    - CAPA Procedure: [document number]

12. REQUALIFICATION HANDOFF
    [This section is auto-populated when a revision involves a Significant Procedural
    or Engineering Change (see Section 9).]

    When this SOP is revised to Rev [X], all operators currently qualified on this
    station must complete requalification before running the new revision unsupervised.

    Requalification ticket: [auto-generate through Training Plan & Skill Matrix skill
    using this SOP document number, affected station ID, revision delta description,
    and target-qualification date from the ECN implementation date]

    Minimum requalification evidence (per config.yml → qualification_matrix):
    - [ ] Read and acknowledge new SOP revision (LMS record or signature)
    - [ ] Observed run: [n] pieces with supervisor sign-off
    - [ ] KPC/KCC checkpoint demonstration on [specific step numbers]
    - [ ] Training record updated in [LMS platform from config.yml → lms_platform]

13. REVISION HISTORY
    | Rev | Date | ECN | Author | Approved By | Description of Change |
    |-----|------|-----|--------|-------------|----------------------|
    | A   | [date] | N/A | [name] | [name] | Initial release |
```

**Step 3 — Formatting and readability rules.**

- Every procedure step is a single imperative action — never a compound instruction ("Clamp and verify" = two steps).
- Active voice, present tense, second person ("Verify the torque reading") or imperative ("Clamp the workpiece") — never third-person narration ("The operator should check...").
- Safety callouts use the ANSI Z535.4 signal-word hierarchy.
- Quality checkpoint callouts state: what to check / acceptance criterion / gauge or method / action on reject / control-plan or drawing basis / SPC instruction if applicable.
- KPC (Key Product Characteristic, symbol ◆) and KCC (Key Control Characteristic, symbol ▲) are called out explicitly at every checkpoint that involves them. For customer-CSR-specific KPC/KCC designations, cite the applicable CSR name and clause.
- **Readability target:** 6th–8th grade Flesch-Kincaid for line work. 6th grade for high-turnover, ESL, or entry-level-operator stations. From `config.yml` → `language_profile`, trigger parallel-translation generation for any language represented by ≥10% of the operator pool (Spanish / Vietnamese / Portuguese / Mandarin / Tagalog are the most common; use the platform's translation workflow if available).
- No hedge words ("might," "may," "try to," "should consider") — SOPs are directive. If a step has conditional logic, state: "If [condition], perform Step X. If not, continue to Step Y."
- If any required input was missing, insert a `[TO BE COMPLETED: — description of what is needed and who is responsible]` placeholder rather than fabricating procedural detail.

**Step 4 — QMS-platform export format.**

From `config.yml` → `qms_platform`, structure the output for native upload:

- **MasterControl / MasterControl Quality Suite** — Section headers map to MasterControl document sections; controlled-document header fields map to the form builder fields; approval workflow triggers on the "Approved By" field; revision history table maps to the revision log object.
- **ETQ Reliance** — Document type = "SOP"; procedure steps map to the step-and-instruction object; FMEA linkage field populated from the PFMEA reference; training tasks auto-generated from the requalification handoff section.
- **Plex / Plex Quality** — SOP document linked to the part number, operation, and workcenter in the Plex production record; quality checkpoints map to Plex inspection points; revision triggers Plex training acknowledgment workflow.
- **Greenlight Guru (medical devices)** — Document linked to the Design History File (DHF) or Device Master Record (DMR); change-control workflow triggers on revision; GAMP 5 Cat designation field in the header.
- **Sparta TrackWise / TrackWise Digital** — Quality record type = "Controlled Document"; CAPA linkage populated from the CAPA reference field; document distribution list auto-populated from responsibility matrix.
- **IQS / InfinityQS ProFicient** — SOP linked to the control plan and PFMEA record; quality checkpoints mapped to IQS checksheet; SPC parameters linked to the ProFicient data collection setup.
- **Veeva Vault QMS (pharma / life sciences)** — Document lifecycle = Draft → Review → Approved → Effective → Obsolete; Section 9 change-classification maps to Veeva change control workflow type; FDA 21 CFR Part 11 e-signature requirement on the approval line.
- **AssurX** — Document record; quality event linkage; CAPA action item linkage.
- **SAP QM module** — Inspection plan reference; operation-level inspection characteristic; usage decision code; document info record (DIR) linkage.
- **Platform-neutral output** — Produce standard markdown with a YAML frontmatter block keyed on: doc_number / revision / effective_date / supersedes / ecn_ref / owner / approver / qms_framework / scope / language / kpc_count / kcc_count / regulated_steps / requalification_required.

## Output Requirements

- Complete controlled-document header (all fields populated or explicitly marked TO BE COMPLETED)
- All 13 sections in order with correct imperative-step format
- Every safety hazard carries a SAFETY callout with ANSI Z535.4 signal word and mitigation
- Every KPC or KCC step carries a QUALITY CHECKPOINT callout with accept/reject criteria, gauge/method, drawing/control-plan basis, and SPC instruction if applicable
- PPE listed at document level (Section 4) and reinforced at step level where additional PPE beyond the baseline is needed
- Readability target met per language profile from config.yml
- Change-classification determination in Section 9 (for revisions) with PPAP and requalification disposition
- Requalification handoff in Section 12 (auto-generated for significant/engineering changes)
- QMS-platform export format applied per config.yml → qms_platform
- Revision history table populated
- If output saved to `outputs/`: file named `[SOP-number]_Rev[X]_[date].md` (or platform-native format)

## Anti-Patterns to Avoid

- **Do not** write compound steps. "Inspect and quarantine" is two steps; write them separately so the operator can follow one at a time and the quality checkpoint can be placed between them.
- **Do not** use hedge words ("may," "might," "should consider," "as needed"). SOPs are directive. If the step is conditional, write the conditional explicitly.
- **Do not** fabricate procedural details, drawing numbers, gauge calibration status, or PFMEA line-item references. Mark them TO BE COMPLETED.
- **Do not** write a quality checkpoint without an acceptance criterion and a reject action. A checkpoint with no accept/reject standard is a documentation liability rather than a quality gate.
- **Do not** separate the KPC/KCC designation from the quality checkpoint callout. If a characteristic is KPC or KCC, the callout must name that status so the operator and auditor can see it in context.
- **Do not** omit the requalification handoff section on a revision with any procedural or engineering change. A revised SOP without a requalification event means the operator running the process may still be following the old procedure.
- **Do not** close a CAPA-driven SOP revision without ensuring the new revision is loaded in the QMS platform and the distribution list has been notified. "SOP revised" is not a CAPA closure; "operators running the process have been requalified on the new revision" is.
- **Do not** use third-person narration or passive voice in the procedure body. "The workpiece is clamped by the operator" is harder to follow and harder to hold compliant to than "Clamp the workpiece in the fixture."
- **Do not** list PPE only at the document level and omit it at step level for steps requiring additional protection beyond baseline. Auditors check for this.
- **Do not** treat a cosmetic/editorial change as requiring no documentation. Even editorial revisions must be recorded in the revision history; the distinction is that they do not trigger ECN or PPAP.

## Integration Notes

- **Pairs with Work Instruction Generator** — The SOP is the governing document; the WIG produces the operator-facing digital work instruction for the same process. Both must be at the same revision. When this SOP is revised, trigger a corresponding WIG update using the SOP document number and ECN reference as the input.
- **Pairs with Training Plan & Skill Matrix** — The requalification handoff in Section 12 feeds directly into the Training Plan & Skill Matrix skill, which generates the requalification ticket, updates the ILUO matrix, and triggers LMS notifications.
- **Pairs with CAPA Document Builder** — When this SOP is authored or revised as a corrective action, the CAPA record references this document number and revision as the "controlled-document update" element of the D5 permanent corrective action.
- **Pairs with Quality Report Generator** — The KPC/KCC checkpoint structure in this SOP feeds the Quality Report Generator's per-stage inspection table and SPC analysis.
- **Pairs with Safety Incident Report** — If a safety incident identified a procedure gap, the corrective SOP revision feeds back into the incident report as the long-term CAPA action.
- **Pairs with Vision Inspection Summary** — If automated vision inspection is a quality gate in this SOP, the threshold-policy and false-accept/reject criteria must be consistent with the vision system's operating parameters as tracked by the Vision Inspection Summary skill.

## Success Metrics

- **ISO/IATF/FDA audit finding rate** — Target: zero nonconformities on document-control or work-instruction clauses across all audits. Track against IATF 16949 §7.5.3 / §8.5.1.4 and ISO 9001 §7.5 as the primary clause reference.
- **SOP completion rate (no TO BE COMPLETED stubs at release)** — Target: 100% of required fields populated before document is approved. Monitor in QMS platform.
- **Requalification timeliness on revision** — Target: 100% of affected operators requalified before the new revision's effective date. Track in LMS.
- **KPC/KCC checkpoint coverage** — Every KPC and KCC in the control plan should map to at least one QUALITY CHECKPOINT callout in the SOP. Gap = audit finding.
- **SOP-to-DWI currency alignment** — SOP revision must match the corresponding DWI revision. Mismatches are flagged by the WIG handoff; track and close within one shift.
- **Time to release (new SOP or revision)** — Target: ≤ 5 business days from first draft to approved-and-distributed for a routine revision; ≤ 2 business days for a safety- or customer-complaint-driven CAPA revision.
