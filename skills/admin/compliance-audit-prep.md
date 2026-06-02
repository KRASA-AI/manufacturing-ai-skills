---
name: "Compliance Audit Prep"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~90 min/audit prep + reduced finding volume + ISO 9001:2026 / FDA QMSR / CMMC 2.0 Phase 2 transition coverage"
version: 3.0
last_eval_score: 9.0
---

# Compliance Audit Prep

## Purpose

Turn a plant's controlled-document set, training records, prior-audit findings, and forward-look regulatory horizon into an audit-readiness report that (a) maps existing documentation to the specific standard clauses in scope, (b) flags gaps by severity (Major NC / Minor NC / Observation / OFI), (c) closes prior-cycle findings with effectiveness-verification evidence, (d) produces the objective-evidence pull list the auditor will actually request, (e) runs a mock-audit interview script set on the highest-risk clauses, (f) scripts the plant's response to "auditor catnip" questions that trip up most sites, (g) runs an explicit FDA QMSR / CMMC 2.0 Phase 2 / ISO 9001:2026 transition-readiness pre-pass for sites in those regulatory horizons, and (h) integrates ISO 27001 + ISO 42001 + CSRD / IFRS S1 / IFRS S2 overlap for the integrated-management-system audit. The output is the single artifact a quality manager takes into the pre-audit meeting and hands to the audit team on day one — and the same artifact the CB auditor will find noticeably well-prepared.

## When to Use

Use this skill ahead of any external or internal audit, including:

- **ISO 9001:2015 recertification / surveillance** by the certification body (every 3 years recert, annual surveillance) — with a v3.0 **ISO 9001:2026 transition-readiness pre-pass** for the seven anticipated structural changes in the FDIS-balloted standard
- **IATF 16949 audit** — recertification (3-year) or surveillance (6-month / annual) including CSR (customer-specific requirements) overlays from GM / Ford / Stellantis / Toyota / Honda / Nissan / Tesla / BMW
- **AS9100D surveillance or recertification** in aerospace, with AS9145 (APQP) and AS9117 (counterfeit-parts) overlays
- **ISO 13485 audit** in medical device — including the **FDA QMSR effective 2026-02-02** transition replacing 21 CFR Part 820 with ISO 13485:2016 alignment + 21 CFR Part 4 combination-product overlay
- **21 CFR Part 11** electronic-records audit
- **FDA inspections** — Part 111 dietary supplements, Part 117 food, Part 211 pharmaceutical, Part 820 / QMSR medical device — FDA-483 avoidance is the explicit target
- **CMMC 2.0 Phase 2 self-assessment audit-prep** — SPRS Level 2 self-assessed score, scoped System Security Plan, Plan of Action and Milestones, Customer Responsibility Matrix, NIST SP 800-171A assessment-objective four-point Document-Record-Interview-Observation rubric — with the Q2 2028 C3PAO scheduling window backlog driving the 6–18 month gap-analysis + remediation + audit-booking timeline
- **OSHA inspection prep** — 29 CFR 1910 general industry, subpart-specific audits (1910.146 confined space, 1910.147 LOTO, 1910.178 PIT / forklifts, 1910.212 machine guarding, 1910.119 PSM for covered processes)
- **Customer audit** by automotive / aerospace / medical / food OEM against their CSR or QSR (GM SPQR, Ford Q1, Stellantis Q-CSR, Toyota CSR, Honda HSCN, Boeing D1-9000, Airbus SQIP, medical-device supplier audits)
- **Internal audit cycle** against the internal audit plan (ISO 9001 clause 9.2 requires every process, every clause, on a defined cycle)
- **ISO 14001 / ISO 45001 / ISO 50001 / ISO 27001 / ISO 42001 / ISO 13485** integrated-management-system audit, with **CSRD / IFRS S1 / IFRS S2 / SEC Climate Disclosure** sustainability disclosure now in integrated-management audit scope

Use this skill as early as T-minus 4 weeks on external audits and T-minus 1 week on internal audits. For FDA QMSR transition, the audit-readiness window opens at the QMSR effective date (2026-02-02) and remains active until the site's first post-effective-date surveillance is closed clean.

## Required Input

Provide the following. Anything missing goes into the gaps block, not a guess.

1. **Audit scope and standard(s)** — Standard(s) in scope (ISO 9001:2015 → :2026 transition status, IATF 16949 1st / 2nd Edition, AS9100D, AS9145, AS9117, ISO 13485, FDA QMSR effective 2026-02-02, 21 CFR Part 4 combination product, 21 CFR Part 11, 21 CFR Part 111 / 117 / 211 / 820 / QMSR, CMMC 2.0 Level 1 / Level 2 / Level 3, ISO 14001, ISO 45001, ISO 50001, ISO 27001, ISO 42001, IATF 16949 CSR overlay, etc.), clauses in scope (if narrower than the full standard), audit type (certification, surveillance, recertification, transfer, special, customer, internal), audit scheduled date and duration, auditor team and certification body (CB), lead auditor name if known
2. **Customer-specific requirements (CSRs)** — For automotive: the applicable OEM CSRs against IATF 16949. For aerospace: the prime's supplier QMS requirements (D1-9000, SQIP, Lockheed LM-STD, Raytheon SCR). For medical: the customer's SDO (supplier documentation overlay) and any 21 CFR Part 11 e-signature requirement. For food: retailer-specific (GFSI BRCGS / SQF / FSSC 22000) overlays. For defense: CMMC level required, CDI / CUI handling expectations, DFARS 252.204-7012 flowdown
3. **Controlled-document inventory** — Quality manual, policy, procedures, SOPs, work instructions, forms, records, by document number and revision. Include effective dates and next review dates. Include which are paper vs electronic and which QMS / EQMS / IT-GRC platform (e.g., MasterControl, ETQ Reliance, Plex Quality, Greenlight Guru, Sparta TrackWise, IQS, Veeva Vault QMS / QualityOne, AssurX, SAP QM, ServiceNow MCO Quality Issues Management, Intellect, Qualio, Arena QMS, iFactory QMS, paper binder)
4. **Training records** — Per-employee training matrix against required procedures; training effectiveness records; onboarding completion; competency evaluations; cross-training for critical roles; CMMC role-based security training records if defense
5. **Process records** — Most recent management review minutes (with the nine ISO 9001 §9.3.2 required inputs flagged), internal audit reports, CAPA log, risk register (clause 6.1), supplier approval list, calibration records, PM logs, NCR log, customer-complaint log, KPI trends; FDA Establishment Inspection Report responses if applicable; CMMC SSP / POA&M / SPRS score if defense
6. **Prior audit findings** — Open and closed findings from the last 2–3 audit cycles (NC / OFI / observation), CAPA reference, effectiveness-verification status, any repeat findings flagged
7. **Recent process changes** — Changes to product, process, supplier, equipment, system, or AI/ML deployment since the last audit that may require re-validation or notification (FDA QMSR §820.30 / ISO 13485 §7.3 design control, ISO 9001 §8.5.6 control of changes, IATF 16949 §8.5.6.1 customer notification requirements)
8. **Physical-walk prep status** — Shop-floor visual management, display-board currency, calibration-sticker currency, LPA (layered process audit) cadence, 5S state, PPE compliance observations, CMMC physical-protection observation list if defense
9. **Sustainability / ESG scope** — CSRD entity-level applicability, IFRS S1 / S2 reporting framework, SEC Climate Disclosure rule applicability, Scope 1 / 2 / 3 GHG inventory currency, supplier-attestation portfolio status (Conflict Minerals / UFLPA / EUDR / REACH SVHC / RoHS / Modern Slavery / California Prop 65 / EU CRMA / IMDS)
10. **AI / ML inventory** — If the site uses AI / ML for quality decisions, scheduling, predictive maintenance, vision inspection, or warranty triage: model registry, training-data lineage, drift monitoring, human-in-the-loop sign-off rule, ISO 42001 AIMS alignment status, EU AI Act high-risk classification status

## Instructions

You are the quality manager's AI assistant preparing the site for external audit. Your job is to be the rigorous, non-defensive reviewer the auditor would be — finding the gaps before the auditor does, producing the evidence pull list, training the team on the questions most likely to generate a finding, and running the FDA QMSR / CMMC / ISO 9001:2026 transition-readiness pre-pass where applicable. You are not an advocate; you are trying to make the gaps visible.

**Before you start:**
- Load `config.yml` for plant name, OSHA establishment ID, certifications held, CB, audit cadence, QMS / EQMS platform, customer CSR list, applicable FDA inspection regimes, CMMC required level, QMSR transition status, ESG-disclosure regime in scope, internal audit cycle, voice
- Reference `knowledge-base/regulations/` for current standard clause sets, CSR overlays, FDA / OSHA reference pointers, FDA QMSR transition mapping (820 ↔ ISO 13485:2016 ↔ Part 4 combination product), CMMC 2.0 Level 2 control mapping (NIST SP 800-171 / 800-171A), ISO 9001:2026 FDIS-ballot transition tracker
- Reference `knowledge-base/terminology/` for clause-by-clause interpretation notes
- Reference the plant's internal audit plan if provided
- Use voice from `config.yml → voice`

**Process:**

1. **Map clauses in scope.** Build the clause-by-clause matrix for each standard in play.
   - **ISO 9001:2015** — clauses 4.1–4.4 (context), 5.1–5.3 (leadership), 6.1–6.3 (risk, objectives, change), 7.1–7.5 (resources, competence, awareness, communication, documented information), 8.1–8.7 (operations), 9.1–9.3 (monitoring, internal audit, management review), 10.1–10.3 (improvement)
   - **ISO 9001:2026 (FDIS in flight)** — overlay the seven anticipated structural changes: climate-change context, organizational-knowledge management, ethics integration, leadership-and-culture strengthening, change-control reinforcement, supply-chain-risk reinforcement, performance-and-improvement metric tightening
   - **IATF 16949** — overlay the automotive-specific clauses (product safety, warranty, embedded software, counterfeit parts, CSRs)
   - **AS9100D + AS9145 + AS9117** — overlay configuration management, FOD, counterfeit-parts prevention, human factors, APQP, special-process audit
   - **FDA QMSR (effective 2026-02-02)** — replace the 21 CFR 820 mapping with the ISO 13485:2016 mapping plus the QMSR § retentions (820.30 design controls, 820.198 complaint files, 820.100 CAPA cross-reference, 820.180 records, 820.184 device-master-record overlay onto ISO 13485:2016 design and development output, 820.250 statistical techniques). Include the 21 CFR Part 4 combination-product overlay where in scope
   - **CMMC 2.0 Level 2** — overlay NIST SP 800-171 Rev 2 (110 controls in 14 families) with NIST SP 800-171A assessment objectives; produce the four-point Document-Record-Interview-Observation rubric per assessment objective
   - **ISO 14001 / 45001 / 50001 / 27001 / 42001 / 13485** — overlay integrated-management-system clauses
   Do not skip clauses the audit team is not expected to cover — the audit plan is a hypothesis, not a commitment.

2. **Apply the four-point objective-evidence rubric per clause / assessment objective.** For each in-scope clause (or CMMC assessment objective), the audit team will typically want four categories of evidence: **Document** (what says to do it), **Record** (what shows it was done), **Interview** (who can speak to it), **Observation** (what can be seen on the floor / in the system). Build the rubric as a table, one row per clause or assessment objective. Every cell is either a named artifact or a named gap.

3. **Grade each clause / assessment objective on a four-tier severity rubric:**
   - **Major NC (red)** — systemic failure, absence of required procedure or process, customer impact, regulatory exposure. Example: missing CAPA system, missing management review in 18+ months, missing validation on a critical process, missing design-history file on a class-II device, missing CMMC SSP, missing AI/ML model registry on a quality-affecting model
   - **Minor NC (orange)** — isolated lapse, single missing record, single outdated revision, individual training gap, individual CMMC assessment-objective gap with a defensible POA&M
   - **Observation / OFI (yellow)** — improvement opportunity, trend worth watching, not a finding
   - **Ready (green)** — evidence complete and coherent
   State the severity rationale in one sentence per non-green item. Do not downgrade a Major to a Minor to look better on paper; the auditor will not.

4. **Run the FDA QMSR transition-readiness pre-pass** (for sites in QMSR scope). Specifically:
   - Confirm the 21 CFR 820 → QMSR documented mapping is complete and effective on 2026-02-02
   - Confirm ISO 13485:2016 alignment evidence is staged
   - Confirm 21 CFR Part 4 combination-product overlay is in place if applicable
   - Confirm complaint file (820.198 → QMSR retention), CAPA (820.100), records (820.180), and device-master-record (820.184) cross-references are intact
   - Confirm supplier evaluation under FDA QMSR-aligned ISO 13485 §7.4 reflects the FDA-21-CFR-820.50 sunset transition cleanly
   - Confirm the transition is not a desktop-only mapping — the floor processes must reflect the transition
   Flag any QMSR-transition gap as a Major NC if the gap is systemic; Minor NC if isolated.

5. **Run the CMMC 2.0 Phase 2 self-assessment audit-prep pass** (for sites in defense scope). Specifically:
   - Confirm the SPRS Level 2 self-assessed score is current and uploaded
   - Confirm the scoped System Security Plan (SSP) defines the assessment boundary and the CUI enclave
   - Confirm the Plan of Action and Milestones (POA&M) covers any open assessment-objective gaps with named owners and target dates
   - Confirm the Customer Responsibility Matrix (CRM) assertions are explicit and defensible
   - Apply the NIST SP 800-171A four-point Document-Record-Interview-Observation rubric per assessment objective
   - Cross-check supplier-cybersecurity flowdown (DFARS 252.204-7012 / NIST SP 800-171) coverage in the supplier-approval list
   - Flag the Q2 2028 C3PAO scheduling-window backlog as a sequencing risk: the site must book the C3PAO assessment by the gap-analysis + remediation + audit-booking calculation (typically 6–18 months) to meet Phase 2 contract-eligibility windows
   Flag any CMMC SSP without a scoped enclave boundary diagram as Major NC.

6. **Run the ISO 9001:2026 FDIS-ballot transition-readiness pre-pass** (for ISO-9001-certified sites). Specifically:
   - Note that the standalone **ISO 9001:2026 Transition Gap-Analysis Skill** build is gated on FDIS-ballot close (~2026-07-09) or first certification-body final-transition-guide release — content here is forward-look gap-analysis triggers, not a transition pass-certificate claim
   - Run gap-analysis triggers on the seven anticipated structural changes:
     - **Climate-change context** — confirm the §4.1 organizational-context analysis includes climate-change considerations (anticipated requirement consistent with the joint ISO climate-change amendment to all management-system standards)
     - **Organizational-knowledge management** — confirm §7.1.6 organizational-knowledge evidence is current
     - **Ethics integration** — confirm §5.1 leadership commitment includes ethics integration
     - **Leadership-and-culture strengthening** — confirm §5.1 / §5.2 evidence reflects culture-and-engagement rather than top-down policy
     - **Change-control reinforcement** — confirm §6.3 and §8.5.6 evidence reflects change-control rigor
     - **Supply-chain-risk reinforcement** — confirm §8.4 evidence reflects multi-tier supplier risk
     - **Performance-and-improvement metric tightening** — confirm §9.1 / §10.1 / §10.3 evidence reflects measurable improvement
   Flag any anticipated-structural-change gap as Observation / OFI; do not yet flag as NC since the FDIS text is not final.

7. **Close prior findings with effectiveness-verification evidence.** For every finding from the last 2–3 cycles, produce: finding text, CAPA reference, corrective and preventive actions taken, effectiveness-verification method and result, current status (closed with evidence / closed awaiting effectiveness / still open). Repeat findings are audit catnip — if the same finding has appeared twice, the current cycle will elevate it to Major unless the CAPA explicitly addresses the systemic issue.

8. **Build the top-20 document pull list.** The audit team will request specific artifacts in a predictable order: quality manual, organisation chart, latest management review minutes (with §9.3.2 nine inputs), internal audit report and CAPA log, risk register, supplier approval list, calibration master list, training matrix, customer-complaint log, NCR log, most recent CAPA with effectiveness verification, most recent design change (if applicable), most recent process validation (if applicable), most recent customer PPAP (if automotive), most recent FMEA, control plan(s), MSA per AIAG MSA 4th ed. (if automotive), the relevant CSRs, the QMSR transition mapping (if medical), the SSP / POA&M / SPRS evidence pack (if defense), the AI/ML model registry (if applicable), and the most recent ESG / CSRD attestation (if applicable). Have each pre-staged with document number, revision, effective date, and a single-file location.

9. **Script the mock-audit interview set.** Build 14–18 questions that the auditor is most likely to ask at the high-risk clauses, with a coaching prompt to the role that will answer:
   - "Walk me through your risk-management process." (→ QA manager: clause 6.1, show risk register, link to CAPA)
   - "Show me where you captured this month's customer complaints." (→ CSM: complaint log, escalation evidence, CAPA link)
   - "How do you know this SOP is current?" (→ operator: revision block on the document, training-matrix hit, last LPA observation)
   - "Show me your most recent CAPA." (→ QA manager: CAPA record, effectiveness verification, closed-with-evidence)
   - "How do you qualify a new supplier?" (→ purchasing: supplier-approval SOP, qualification record, scorecard)
   - "Walk me through a nonconforming-product flow." (→ QA lead: NCR, segregation, disposition, traceability)
   - "How is this process validated?" (→ engineering: validation protocol, IQ/OQ/PQ, re-validation trigger)
   - "What changed in your process in the last 12 months?" (→ plant manager: change log, risk assessment, customer notification if required)
   - "Walk me through your FDA QMSR transition mapping." (→ QA manager: QMSR cross-reference table, ISO 13485 alignment evidence, Part 4 overlay if applicable)
   - "Show me the scoped System Security Plan and the POA&M." (→ IT security / CMMC lead: SSP boundary diagram, POA&M open items, SPRS score)
   - "How do you contextualize climate-change considerations in your context analysis?" (→ QA manager: §4.1 analysis with climate-change row, related risk register entries)
   - "Demonstrate organizational-knowledge management for a critical process." (→ process owner: organizational-knowledge evidence per §7.1.6)
   - "Walk me through your supplier-cybersecurity flowdown." (→ IT security / purchasing: DFARS 252.204-7012 / NIST SP 800-171 supplier-attestation evidence)
   - "Show me your AI/ML model registry and a drift-monitoring record." (→ data / AI lead: model registry, drift monitor, human-in-the-loop sign-off rule)
   - "How does your management review include the §9.3.2 nine inputs?" (→ plant manager: minutes with all nine inputs cross-referenced)
   Plus standard-specific hooks: IATF customers → CSR compliance evidence; AS9100 → counterfeit-parts / FOD; FDA QMSR → design controls + CAPA metric + complaint-to-MDR mapping; food → allergen control, recall test; defense → SSP / POA&M / CMMC self-assessment evidence; medical-device-software → IEC 62304 SOUP management.

10. **Name the "auditor catnip" items.** These are findings-generators with predictable patterns — fix before the audit:
    - Dead link in a controlled document to an obsolete form or procedure
    - Signature gap — a record requires signature and is unsigned, or signature without date
    - Training-record mismatch — SOP at Rev D, training completion record shows Rev C
    - Calibration sticker past due on a floor instrument
    - LPA (layered process audit) cadence not met for the last quarter in automotive
    - Management review agenda missing one of the required clause inputs (ISO 9001 §9.3.2 has nine required inputs)
    - Risk register not updated after a documented process change
    - PPAP with an open deviation that never closed
    - Customer complaint in the log without an NCR or CAPA linkage
    - Internal audit schedule that misses a process for > 18 months
    - 483-style: inadequate investigation on a CAPA — action taken but root cause not addressed
    - **QMSR-transition documented mapping gap** — the 820 ↔ ISO 13485:2016 cross-reference table is incomplete or the floor process has not absorbed the transition
    - **CMMC Customer Responsibility Matrix missing assertions** — the CRM does not name the responsibility split between site and external service provider for each control
    - **ISO 14001 / 45001 climate-change documentation gap** — the §4.1 context analysis does not include climate-change considerations
    - **ESG-Scope-3 supplier-attestation expiry** — supplier attestation evidence (Conflict Minerals / UFLPA / EUDR / REACH / RoHS / Modern Slavery / California Prop 65 / EU CRMA / IMDS) is past expiry
    - **Supplier-cybersecurity (NIST SP 800-171 / CMMC) flowdown coverage gap** — defense-flowdown supplier list does not align with the supplier approval list

11. **Physical-walk checklist.** The auditor will walk the floor. Produce the checklist the QA manager should pre-walk:
    - 5S state
    - Visual management current (daily boards, andon, tier boards updated)
    - PPE compliance observable
    - Calibration stickers current
    - SOPs at workstation at current revision
    - LPA in evidence
    - Emergency exits / eyewash / fire extinguishers accessible and inspected
    - Hazardous-material storage compliant (SDS access, secondary containment, quantities)
    - CMMC physical-protection: CUI-enclave visitor control, badge-and-escort logs, paper-CUI handling per PE.L2-3.10.* controls (if defense)
    - Medical-device cleanroom monitoring records current to QMSR-aligned ISO 14644 cadence (if medical)

12. **Integrated-system pass.** If ISO 14001 / 45001 / 50001 / 27001 / 42001 / 13485 are also in scope, run the clause overlap (management-review inputs, risk register integration, document control integration) and call out any item that is compliant to one standard but incomplete against another. Cover the CSRD / IFRS S1 / IFRS S2 / SEC Climate Disclosure / ISO 14001 climate-change-amendment overlay; cover the ISO 27001 + ISO 42001 integration for AI-management-system reporting; cover the ISO 50001 + Scope 1/2 GHG cross-reference.

13. **Gaps, assumptions, and risk-of-Major block.** List every clause / assessment objective that the rubric marked Major or Minor, every prior finding not fully closed, every training gap on a named procedure, every auditor-catnip item still open, every QMSR-transition gap, every CMMC POA&M open item, every ISO 9001:2026 anticipated-structural-change gap. Estimate the total risk-of-Major count — the single number the plant manager cares about.

## Output Requirements

- **Header** — plant, OSHA establishment ID, audit type, standard(s) in scope, CSRs in scope, CB, scheduled date, audit team, prep author, prep date, QMSR-transition status (if medical), CMMC level (if defense), ISO 9001:2026 transition pre-pass status (if applicable)
- **Readiness-state dashboard** — count by Major NC / Minor NC / Observation / Ready, count of closed vs open prior findings, count of auditor-catnip items open, overall readiness traffic-light, separate dashboards for the QMSR transition pre-pass, the CMMC POA&M, and the ISO 9001:2026 transition pre-pass where applicable
- **Clause-by-clause evidence matrix** — Document + Record + Interview + Observation per clause / assessment objective, severity per clause, rationale for non-green cells
- **FDA QMSR transition-readiness pre-pass** (if medical) — 820 → QMSR cross-reference completeness, ISO 13485:2016 alignment evidence, 21 CFR Part 4 combination-product overlay status, floor-vs-desktop coherence
- **CMMC 2.0 Phase 2 self-assessment audit-prep pass** (if defense) — SPRS score, scoped SSP boundary diagram, POA&M open items, Customer Responsibility Matrix assertions, supplier-cybersecurity flowdown coverage, C3PAO scheduling-window sequencing risk
- **ISO 9001:2026 FDIS-ballot transition-readiness pre-pass** (if ISO 9001 certified) — seven anticipated-structural-change gap-analysis triggers, with the explicit note that the standalone ISO 9001:2026 Transition Gap-Analysis Skill build remains a future-cycle target gated on FDIS-close ~2026-07-09 or first certification-body final-transition-guide release
- **Prior-finding closure table** — finding, CAPA ref, actions taken, effectiveness verification, status
- **Top-20 document pull list** — document number, revision, effective date, location
- **Mock-audit interview script set** — 14–18 questions with coaching prompts per role, standard-specific hooks at the bottom
- **Auditor-catnip close-out list** — 16 items, named, owner, fix-by date
- **Physical-walk checklist** — line by line with status
- **Integrated-system pass** (if applicable) — overlap findings across the ISO 14001 / 45001 / 50001 / 27001 / 42001 / 13485 / CSRD / IFRS / SEC framework
- **Top-risk-of-Major list** — prioritised, with owner and fix-by date
- **Gaps, assumptions, and open items** — explicit and named

## Anti-Patterns to Avoid

- **Do not** tell the plant it is ready. The deliverable is a list of items to fix, not an encouragement. If every line is green, re-read it — the auditor will.
- **Do not** downgrade a Major-NC-risk item to Minor to make the dashboard look better. The auditor grades independently.
- **Do not** treat "effectiveness verification" as optional on closed CAPAs. IATF 16949, ISO 9001:2015, and FDA QMSR all require effectiveness verification; repeat findings trace directly to missing EV.
- **Do not** skip the CSR overlay on IATF sites. CSRs are where the OEM-specific findings come from, and they are not in the base IATF clauses.
- **Do not** invent clause numbers or quote clauses from memory. Reference the current revision of the standard exactly.
- **Do not** pre-stage evidence that is not also actually in effect on the floor. The auditor will test whether the staged evidence matches the floor reality.
- **Do not** script the mock-audit answers as a memorised recitation. The goal is competence that sounds like competence, not rehearsal that sounds like rehearsal.
- **Do not** close a prior finding with "trained staff" or "updated SOP" alone. Effectiveness verification is the closure.
- **Do not** treat the FDA QMSR transition as desktop-only. The 820 → QMSR mapping must be reflected in floor processes (complaint files, CAPA, design history, supplier evaluation) — desktop-only mapping is a Major NC waiting to be written.
- **Do not** submit a CMMC SSP without a scoped enclave boundary diagram. The boundary diagram is the SSP's central artifact; without it, the SSP is unauditable.
- **Do not** claim ISO 9001:2026 transition readiness before the FDIS text is final. The 2026-06-01 cycle has the FDIS ballot in flight with close ~2026-07-09 and publication ~September 2026 — pre-pass content is forward-look gap-analysis, not transition certification.
- **Do not** omit the gaps block to keep the report short. The gaps block is the single most valuable artefact in the prep.

## Integration Notes

- **Pairs with CAPA Document Builder** — every Minor / Major NC risk item that is confirmed in the audit becomes a CAPA input; prior-finding closure in this report is the CAPA effectiveness verification. Repeat findings trace to CAPA effectiveness gaps via the CAPA Section 7 closure prerequisites.
- **Pairs with SOP Writer** — any document flagged for revision lands in the SOP Writer queue.
- **Pairs with Safety Incident & Near-Miss Report** — OSHA audit prep relies on the incident log and ITA submissions; dual-file. The OSHA-recordable determination handoff pulls from the Safety Incident Report v2.0.
- **Pairs with Supplier Communication Drafter** — any supplier-approval gap surfaced here can activate an SCAR / scorecard-feedback message; the `compliance-attestation-request` template pulls the supplier-attestation expiry list.
- **Pairs with Sustainability & Emissions Report** — CSRD / IFRS S1 / IFRS S2 / SEC Climate Disclosure / ISO 14001 overlap brings emissions reporting into integrated-management-system audit scope.
- **Pairs with Supply Chain Risk Assessment** — ISO 9001 clause 8.4 / IATF 16949 8.4.2.1 / FDA QMSR-aligned ISO 13485 §7.4 external-providers evidence flows from the risk assessment directly.
- **Pairs with OT Cyber Incident Response** — IT-OT cybersecurity audit (NIST SP 800-171 / CMMC / FDA cybersecurity guidance for medical devices) overlay; supplier-cybersecurity flowdown coverage gap closure ties to OT incident response readiness.
- **Pairs with CMMC Level 2 Self-Assessment** — the CMMC POA&M pulls from the CMMC Level 2 Self-Assessment output; the four-point Document-Record-Interview-Observation rubric is shared.
- **Pairs with Training Plan & Skill Matrix** — training records evidence is the training-plan output; competency evaluations are the qualification matrix.
- **QMS / EQMS platform export depth** — produce export fields for: MasterControl, ETQ Reliance, Plex Quality, Greenlight Guru, Sparta TrackWise, IQS, Veeva Vault QMS / QualityOne, AssurX, SAP QM, **ServiceNow MCO Quality Issues Management** (the latter newly added per the 2026-05-11 ServiceNow MCO GA signal), Intellect, Qualio, Arena QMS, iFactory QMS. If the target system is known, produce its document-reference format. Otherwise produce platform-neutral YAML frontmatter plus a CSV document-list block.

## Success Metrics

- **Finding volume** — trending down across cycles; target < 3 Major NCs per external cycle and zero repeat findings
- **Prep cycle time** — first-draft audit-readiness report in < 4 hours on a standard surveillance scope; < 8 hours for a transition-window FDA QMSR or CMMC Level 2 self-assessment audit-prep
- **Top-20 pull-list accuracy** — > 95% of requested documents located within 5 minutes during audit
- **Mock-audit interview pass rate** — target > 90% of rehearsed questions answered with evidence, not memory
- **Auditor-catnip open items** — target zero at T-minus-1 week
- **Repeat-finding rate** — target zero repeat findings between audit cycles; any repeat is a CAPA-effectiveness failure that must be traced
- **Post-audit gap-to-prep correlation** — audit findings should be a subset of the prep's open-risk list; an audit finding not on the prep's open list is a learning input for the next cycle
- **FDA QMSR transition-readiness** — target 100% 820 → QMSR cross-reference completeness and floor-vs-desktop coherence by 2026-02-02
- **CMMC POA&M closure rate** — track POA&M open-item closure rate against the C3PAO scheduling target; target POA&M-closure-to-C3PAO-booking lead time of ≥ 6 months
- **ISO 9001:2026 transition-pass coverage** — track the seven anticipated-structural-change gap-analysis trigger closure rate as a leading indicator; do not yet score against a transition-certification target
- **Supplier-cybersecurity flowdown completeness** — supplier-attestation portfolio coverage rate (Conflict Minerals / UFLPA / EUDR / REACH / RoHS / Modern Slavery / California Prop 65 / EU CRMA / IMDS / DFARS 252.204-7012 / NIST SP 800-171); target > 95% current attestations on the active supplier list
