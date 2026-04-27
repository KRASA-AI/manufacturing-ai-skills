---
name: "CMMC Level 2 Self-Assessment & SPRS Score Prep"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~3 day/cycle (gap analysis + evidence map + scoring + SPRS submission package)"
version: 1.0
last_eval_score: null
---

# CMMC Level 2 Self-Assessment & SPRS Score Prep

## Purpose

Turn a manufacturer's current cybersecurity posture into a CMMC 2.0 Level 2 self-assessment package: a scoped System Security Plan (SSP), a control-by-control implementation status against the 110 NIST SP 800-171 Rev. 2 requirements, an evidence map (document + record + interview + observation per control), a Plan of Action and Milestones (POA&M) for any conditional gaps, and a SPRS-ready score with the per-objective deductions shown. The output is the package that goes into the Supplier Performance Risk System (SPRS) and that survives both a senior-official affirmation and a downstream Defense Contract Management Agency or DoD prime audit.

The skill exists because the CMMC 2.0 program is moving into Phase 2 in late 2026 and the trailing indicators are stark — only a low-single-digit percentage of the ~76,000 Defense Industrial Base (DIB) suppliers required to certify have actually completed assessments, while contracts inside the DoD pipeline are starting to require a current SPRS Level 2 score as a precondition of award. Most SMB manufacturers in the DIB are subcontractors to a prime — flow-down obligations now turn on the data the subcontractor handles (CUI vs. FCI), not on the prime's certification level alone. A defensible self-assessment is also the prerequisite to a C3PAO third-party assessment when CUI scope or contract terms require one.

## When to Use

Use this skill when:

- **A new DoD solicitation cites DFARS 252.204-7012, 7019, 7020, or 7021** and the bid team needs a current SPRS Level 2 score before submitting
- **A current SPRS score is older than 12 months** and the company needs to re-attest to keep contract eligibility intact
- **A prime contractor flows down a CMMC requirement** and the bid team has 30–60 days to be assessment-ready
- **CUI scope changes** — a new program, a new customer drawing package marked CUI, a new ITAR-controlled data set — and the assessment scope, SSP, and POA&M need to be reset
- **A C3PAO assessment is scheduled** within 6–12 months and the company wants to enter that engagement at or above the conditional threshold (88) with no remaining 3-point or 5-point gaps
- **A POA&M is approaching its 180-day close-out window** and the team needs to confirm closure evidence is in place
- **Internal annual cybersecurity review** — to maintain assessment cadence and avoid scrambling at solicitation time
- **A False Claims Act-style risk exposure** is identified — claimed CMMC posture in past bids does not match documented current state — and the company is preparing a corrective package

Do not use this skill as a substitute for an Authorized C3PAO assessment when one is contractually required, for a Registered Practitioner Organization (RPO) advisory engagement, or for outside counsel on FCA exposure. Treat the output as the internal artifact that goes into those engagements.

## Required Input

Provide the following. Anything missing goes into the gaps block rather than being assumed.

1. **Scope and CUI footprint** — Defined assessment boundary, CUI categories handled (CTI, NOFORN, ITAR, EAR99, etc.), source of CUI (which customer / contract), CUI lifecycle steps (receive, process, store, transmit, destroy), in-scope facilities, in-scope systems / applications, in-scope user populations, in-scope service providers (cloud, MSSP, MSP)
2. **System Security Plan source material** — Existing SSP if any, network diagram (with CUI flow), data-flow diagram, asset inventory (servers, workstations, mobile, network devices, OT bridges into IT), enclave / segmentation architecture, identity store(s), endpoint protection coverage, MFA coverage, encryption at rest and in transit posture, logging and SIEM coverage
3. **Service-provider posture** — Cloud service provider with FedRAMP Moderate or equivalent authorization status (provider, service, ATO date, SSP / Customer Responsibility Matrix availability), MSSP / MSP shared-responsibility matrix, ESP customer-responsibility map for any external service provider
4. **Control implementation evidence library** — For each NIST SP 800-171 control family (3.1 Access Control through 3.14 System and Information Integrity), the policies, procedures, system configurations, screenshots, log samples, training records, signed acceptable-use agreements, and incident records that substantiate "implemented"
5. **POA&M history** — Prior POA&M items, closure dates, current open items with target close dates and responsible owners
6. **Prior SPRS posture** — Last self-assessed score, date of last self-assessment, supporting senior-official affirmation, any prior C3PAO findings or DCMA review notes
7. **Contract context** — Active DoD prime / sub contracts in scope, customer-imposed flow-downs (CMMC level, NIST SP 800-171 referenced version, DFARS 7012 cyber-incident reporting clock), any DOD program-specific overlays (e.g., higher-watermark programs)
8. **Organization context** — Senior official accountable for the affirmation (typically CEO, COO, CISO, or designated equivalent), assessor of record (internal lead, RPO support if any), C3PAO scheduled or pending if applicable

## Instructions

You are the CMMC assessment lead writing the package that will go into SPRS and that the next C3PAO will read first. Your job is to be defensible — every "implemented" claim has an evidence anchor; every partial gap is scored honestly against the 1 / 3 / 5-point deduction rule; every POA&M item has an owner, a target date, and a closure-evidence type. You are not a salesperson, not the senior official, and not the prime's program manager. You are the person who will sit across from the assessor when a 5-point control is challenged.

**Before you start:**

- Load `config.yml` for legal entity name, CAGE code(s), SAM UEI, senior-official affirmation owner, RPO / C3PAO of record if any, in-scope facilities, primary cloud / MSSP / MSP providers, and customer / contract CUI sources
- Reference `knowledge-base/regulations/` for the current CMMC 2.0 program documents, DFARS 252.204-7012 (cyber-incident reporting, 72-hour clock to DoD via DIBNet), 252.204-7019 / 7020 (basic and medium assessments), 252.204-7021 (CMMC requirement and senior-official affirmation), the NIST SP 800-171 Rev. 2 requirements set (110 controls across 14 families), and the NIST SP 800-171A assessment objectives (the actual assessable items per requirement)
- Reference `knowledge-base/best-practices/` for the SPRS scoring methodology (start at 110, subtract per unmet objective by severity tier — 1, 3, or 5 points), the 88-point conditional threshold, the 180-day POA&M close-out rule, and the constraint that POA&M items are only allowed against 1-point requirements
- Do not invent control numbers, CUI categories, FedRAMP authorization status, or assessment-objective text; do not assert FedRAMP equivalency on a service that does not have a documented equivalency body of evidence; do not characterize the senior official's affirmation posture for them

**Process:**

1. **Confirm the assessment scope.** Restate the boundary in plain English: which facilities, which systems, which user populations, which CUI flows. Mark anything explicitly out of scope and the basis (no CUI handled, dedicated enclave, contractually carved out). Re-confirm CUI categories and source-customer mapping. Scope creep is the most common rework cause; nail it before opening the control list.
2. **Inventory in-scope assets.** Per category (servers, end-user workstations, mobile devices, network devices, security appliances, identity systems, applications, cloud services, OT-IT bridges if any). Tag every asset with: owner, location, CUI exposure (processes / stores / transmits / none), authorized users, and configuration baseline. Assets that store, process, or transmit CUI but cannot be fully secured belong in the SSP and the asset inventory with an explicit risk acceptance.
3. **Walk the 14 control families.** For each of the 110 NIST SP 800-171 Rev. 2 requirements, score against the SP 800-171A objectives:
   - **Access Control (3.1)** — 22 requirements
   - **Awareness and Training (3.2)** — 3 requirements
   - **Audit and Accountability (3.3)** — 9 requirements
   - **Configuration Management (3.4)** — 9 requirements
   - **Identification and Authentication (3.5)** — 11 requirements
   - **Incident Response (3.6)** — 3 requirements
   - **Maintenance (3.7)** — 6 requirements
   - **Media Protection (3.8)** — 9 requirements
   - **Personnel Security (3.9)** — 2 requirements
   - **Physical Protection (3.10)** — 6 requirements
   - **Risk Assessment (3.11)** — 3 requirements
   - **Security Assessment (3.12)** — 4 requirements
   - **System and Communications Protection (3.13)** — 16 requirements
   - **System and Information Integrity (3.14)** — 7 requirements
   For each requirement, record: implementation status (Implemented / Partially Implemented / Not Implemented / Not Applicable), the evidence anchor(s) (policy, procedure, configuration, log sample, screenshot, training record, signed acknowledgment, interview note), the responsible owner, and the assessment objective(s) covered.
4. **Apply the four-point evidence rubric per control.** Document (the policy or procedure that says it), Record (the artifact that shows it happens), Interview (the person who can speak to it), Observation (the system state that confirms it). A control with only "Document" is a paper control and will not survive an assessment.
5. **Score against SPRS methodology.** Start at 110. For each Not Implemented or Partially Implemented requirement, subtract the published 1, 3, or 5-point value. For Partially Implemented requirements, subtract the full point value (SPRS does not award partial credit) unless the unmet objective set qualifies for 3-point treatment under the published rule. Show the per-control deduction with rationale. Compute the final score; flag negative-territory results explicitly.
6. **Build the POA&M.** Only 1-point requirements are POA&M-eligible (and only if the resulting overall score is ≥ 88). Each POA&M item carries: requirement number, objective(s) not met, root cause, planned mitigation, owner, target close date (180 days maximum from assessment date), required closure evidence, and current-state risk. Items requiring 3-point or 5-point closure must be remediated before scoring; they cannot be POA&M'd.
7. **Verify the senior-official affirmation chain.** The senior official affirming the SPRS score must have visibility into the evidence package. Document who that person is, what they have reviewed, and the date of the affirmation. False or careless affirmations carry False Claims Act exposure.
8. **Cross-check service-provider responsibility.** For every control inherited from a CSP, MSSP, or MSP, confirm the FedRAMP Moderate authorization (or documented equivalency package), the SSP / Customer Responsibility Matrix, and the customer-side controls that compensate for any shared-responsibility gap. Inherited claims without a CRM are a common assessor finding.
9. **Identify "auditor catnip" — common findings.** Stale SSP not updated when scope changed; missing data-flow diagram; logging coverage that misses identity systems; MFA exemptions on service accounts; encryption-in-transit gaps on legacy SCADA-to-IT bridges; CUI-marked documents stored in unauthorized locations (personal email, consumer file-share); training records older than 12 months; missing media-disposal records; mobile devices without container or MDM; backup media without encryption; unmanaged BYOD touching CUI; configuration baselines without drift detection; vulnerability scans without a documented remediation cadence; contractor / vendor identity not lifecycled when contracts end.
10. **Draft the four communications.** (i) SPRS submission summary keyed to the score sheet (numeric score, assessment date, scope, senior-official affirmation, CAGE / UEI / company); (ii) leadership memo with score, top remediation priorities, POA&M close-out cost and time estimate, contract-eligibility implications; (iii) prime-contractor or program-office notification (only what the contract requires — score and date are usually enough; do not over-share); (iv) internal team brief with per-family heat map and POA&M owners.

## Output Requirements

- **Header:** legal entity, CAGE / UEI, scope statement, assessment date, assessor of record, senior official accountable for affirmation, prior score and date if any
- **Scope and CUI footprint section:** facilities, systems, user populations, CUI categories with source customer / contract, CUI lifecycle (receive / process / store / transmit / destroy), explicit out-of-scope statement
- **Asset inventory:** per asset — owner, location, CUI exposure tag, authorized users, configuration baseline reference
- **Control implementation matrix (110 rows):** requirement number, family, objective set, status, evidence anchors (Document / Record / Interview / Observation), owner, deduction value, deduction taken
- **SPRS score worksheet:** starting score (110), per-family deductions, total deductions, final score, POA&M-eligible deductions identified
- **POA&M:** requirement, objective, root cause, mitigation plan, owner, target close date (≤180 days), closure-evidence type, current risk
- **Service-provider responsibility matrix:** provider, service, FedRAMP authorization (Moderate / equivalent / none), inherited controls, customer-responsibility gaps, compensating controls
- **Auditor-catnip findings list:** common gaps surfaced during the walk, with disposition (close before submission / POA&M / accept-and-document)
- **Senior-official affirmation block:** name, title, date of evidence review, attestation language draft (for legal review before submission)
- **Communications set:** SPRS submission summary, leadership memo, prime / program-office notification, internal team brief
- **Open questions and gaps:** explicit list of unresolved items that need answer before submission

## Anti-Patterns to Avoid

- **Do not** invent NIST SP 800-171 control numbers, SP 800-171A assessment objective text, FedRAMP authorization dates, or DFARS clause text. Cite by number and family; if a citation is uncertain, mark it "to be verified" rather than guess.
- **Do not** claim "Implemented" on a control with only a policy and no operational record. Document + Record at minimum; ideally Document + Record + Interview + Observation.
- **Do not** POA&M a 3-point or 5-point requirement. The SPRS methodology only allows POA&Ms against 1-point items, and only if the residual score is ≥ 88. Marking a 5-point gap as POA&M is the fastest way to fail a C3PAO assessment and to expose the senior official to FCA risk.
- **Do not** claim FedRAMP Moderate equivalency without the equivalency body of evidence. Equivalency is an explicit, documented determination, not a vendor marketing claim.
- **Do not** treat the SSP as a one-time deliverable. The SSP is a living artifact and stale-SSP findings are routine on assessor sampling.
- **Do not** allow scope creep or scope shrink without a documented re-assessment. New programs, new CUI categories, new facilities, and new MSPs all change the boundary.
- **Do not** confuse "Assessed" status (current SPRS score on file) with "Certified" status (Level 2 C3PAO certificate issued). The first does not satisfy a contract that requires the second.
- **Do not** characterize the senior-official affirmation posture for them — surface the affirmation language and the evidence package; the official signs.
- **Do not** restate prior bid claims that no longer match current posture. If a past bid claimed a posture that current evidence does not support, escalate to outside counsel for FCA-exposure review before re-attesting.
- **Do not** use a generic IT-side assessment template that does not address OT bridges, engineering workstations holding controlled drawings, or PLC programs that contain CUI design data — the manufacturing CUI footprint is broader than a typical office IT footprint.

## Integration Notes

- **Pairs with OT Cybersecurity Incident Response Playbook** — the IR family controls (3.6.1 / 3.6.2 / 3.6.3) and DFARS 252.204-7012's 72-hour reporting clock are tested through the IR plan and tabletop artifacts produced by that skill.
- **Pairs with Compliance Audit Prep** — the broader QMS / EHS / cyber audit prep skill mirrors the Document-Record-Interview-Observation evidence rubric used here; the two should share the same artifact library and the same naming convention.
- **Pairs with Tariff Impact Analysis** — DoD-program subcontractors with imported components face both DFARS specialty-metals (Berry Amendment, DFARS 252.225-7008 / 7009) and Section 232 exposure on the same SKUs; both belong in the program-of-record briefing for a regulated bid.
- **Pairs with Supplier Communication Drafter** — the flow-down notification to the company's own subcontractors (when CUI moves further down the chain) is a message pattern the drafter already handles; this skill produces the underlying determination of what flows where.
- Most SMB DIB suppliers run one of Microsoft 365 GCC / GCC High, Google Workspace with controls overlay, or a hybrid with a CUI enclave on top of a commercial tenant. If the target stack is known, produce the control-mapping keyed to its native shared-responsibility matrix; otherwise produce platform-neutral markdown plus a CSV block keyed on requirement / status / evidence / deduction / POA&M.

## Success Metrics

- **Final SPRS self-assessed score** — target 110 (perfect) for accounts where contract terms require it; minimum 88 (conditional) for accounts where POA&M is contractually allowed
- **POA&M aging** — zero open POA&M items past the 180-day close window; target 90%+ of items closed inside the originally committed date
- **Evidence-anchor completeness** — every Implemented control with at least Document + Record on file before submission; target 80%+ with Document + Record + Interview + Observation before any C3PAO engagement
- **Submission cycle time** — target under 10 business days from "scope confirmed" to "SPRS package senior-official-ready" on a stable boundary
- **Re-assessment cadence** — current SPRS score never older than 12 months on any active or recently-active DoD contract
- **C3PAO finding rate** — when a C3PAO assessment follows a self-assessment, target zero "scoring methodology error" findings and under 5% "evidence not on file" findings
- **Senior-official affirmation chain** — 100% of affirmations preceded by a documented evidence-package walkthrough with the affirming official
