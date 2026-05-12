---
name: "OT Cybersecurity Incident Response Playbook"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~6 hr/incident (first 24 h)"
version: 1.0
last_eval_score: 8.8
---

# OT Cybersecurity Incident Response Playbook

## Purpose

Turn a suspected or confirmed cyber event on the plant floor into a structured, time-boxed response that protects life-safety, contains spread between IT and OT, preserves forensic evidence, restarts production from a known-good baseline, and meets every external notification clock the event triggers. The output is the first-24-hour brief — incident classification, containment sequence, evidence preservation list, recovery decision tree, and the four communications the event always forces (regulator, customer, insurer, workforce).

The skill exists because manufacturing has been the most-attacked industry by ransomware for several consecutive years and the 2026 ransomware curve has stabilized at an "elevated new normal" rather than receding. Average total cost of a manufacturing ransomware event is in the high single-digit millions, downtime accounts for the largest share of that cost (well above the ransom demand itself), and SMB manufacturers in particular tend to discover during the event that they do not have a written playbook scoped to OT. Generic IT-side IR plans assume re-image-and-restore on endpoints; an OT response has to think about programmable logic controllers (PLCs) that cannot be re-imaged, safety-instrumented systems (SIS) that must reach safe state before isolation, and process units that take hours to bring up cleanly even after the network is restored.

## When to Use

Use this skill when:

- **Ransomware note appears on a plant-floor host or HMI** — server, engineering workstation, historian, or operator screen
- **EDR / SIEM / OT monitoring (Claroty, Dragos, Nozomi, Armis, Tenable.ot, etc.) raises a high-severity alert** in the production network — lateral movement, suspicious PLC programming, unauthorized engineering session
- **An ICS-CERT, CISA, ISAC, or vendor advisory** identifies an actively-exploited vulnerability in deployed PLCs, drives, HMIs, or remote-access appliances and the question is whether the plant is exposed
- **A tabletop exercise** is being run and the team needs the actual decision sequence on the wall, not a generic IR template
- **A managed-security-provider (MSSP) call escalates a ticket** to the plant and the operations leader has 15 minutes to decide whether to stop the line
- **A customer or insurer requests proof of an incident-response plan** and the existing document is an IT-side plan with no OT scope
- **Post-incident** — to write the lessons-learned report, the regulator response, and the insurer claim narrative

Do not use this skill as a substitute for retained DFIR (digital-forensics and incident-response) counsel, the cyber-insurance carrier's incident-response panel, or the FBI / CISA / state breach-notification process. Treat the output as the internal operating brief that aligns the plant team while those external resources engage.

## Required Input

Provide whatever is known at the time of the event. Missing input goes into a gaps block rather than being estimated.

1. **Trigger** — How the event was detected (ransom note, EDR alert, OT-monitoring alert, customer report, abnormal PLC behavior, third-party notification), time of detection, who is reporting
2. **Affected scope (initial)** — Hosts, workstations, servers, HMIs, historians, engineering workstations, PLCs, drives, network segments, remote-access endpoints, plant areas / lines / cells
3. **Process state** — Lines running / down / starved / blocked, batches in progress, safety-critical processes (furnaces, presses, reactors, robotics cells), inventory of finished and in-process material at risk
4. **Architecture context** — IT/OT segmentation status (Purdue level zoning, DMZ, jump-host pattern), remote-access posture (VPN, vendor remote, cellular cellular modems on equipment), backup posture (last verified restore, immutability, off-network copy), EDR / OT-monitoring coverage
5. **People** — Plant manager, OT lead / controls engineer, IT lead, EHS lead, on-site or on-call DFIR, MSSP / SOC contact, cyber-insurance carrier and policy reference, retained outside counsel
6. **Regulatory and contractual exposure** — CISA CIRCIA reportable status (covered entity? covered cyber incident? ransom payment?), SEC reporting status (public-company materiality clock, 8-K Item 1.05 trigger), state breach-notification jurisdictions, CMMC / DFARS 7012 incident-reporting clock (DoD prime / sub), customer SLA breach-notification clauses, EU NIS2 / GDPR exposure if applicable
7. **Recovery baseline** — Documented golden images, PLC program backups (ladder logic, function blocks, HMI projects) with last-verified date, network configuration backups, AD / identity backups, validated-restore time on prior tabletop or real event
8. **Constraints** — Critical customer commitments in the next 72 hours, regulated batches in progress (FDA, AS9100, IATF), union-contract notification requirements, language for any operator-facing communication

## Instructions

You are the OT incident commander writing the brief that goes to the plant manager and the response team in the first hour. You are not the SOC, not the DFIR firm, and not legal — you are the person who keeps the team aligned on what is happening, what is being done, what is not yet known, and what external clocks have started. Every action you recommend needs an owner and a time. Every claim about scope needs an evidence anchor or a "to be confirmed" tag.

**Before you start:**

- Load `config.yml` for plant identity, retained DFIR firm, MSSP / SOC, cyber-insurance carrier with claim hotline, OT-monitoring platform, EDR vendor, backup vendor, CMMC level if applicable, and customer contractual breach-notification clauses
- Reference `knowledge-base/regulations/` for CISA CIRCIA covered-incident definitions and 72-hour / 24-hour reporting clocks (substantial cyber incident = 72 hours, ransom payment = 24 hours), DFARS 252.204-7012 incident-reporting clock (72 hours to DoD via DIBNet for incidents affecting CDI), SEC Item 1.05 materiality framing, state breach-notification jurisdictions, and the NIST SP 800-61 Rev. 3 incident-response lifecycle aligned to CSF 2.0 (Govern, Identify, Protect, Detect, Respond, Recover)
- Reference `knowledge-base/best-practices/` for the Purdue Reference Model zoning conventions, IEC 62443 zone-and-conduit segmentation, and the CISA #StopRansomware guide
- Do not promise legal positions, do not commit on regulator-classification calls, and do not assess privilege — flag those for outside counsel

**Process:**

1. **Classify the incident.** Triage to one of: (a) confirmed ransomware encryption underway, (b) ransom note with no confirmed encryption (extortion-only / data-theft), (c) suspected unauthorized access without encryption, (d) malware detection with no confirmed lateral movement, (e) unauthorized PLC programming or change-control violation, (f) supply-chain compromise (vendor remote-access tooling, software update). The class determines the containment sequence.
2. **Set life-safety as the gating constraint.** Before isolating anything, confirm that any safety-instrumented system (SIS), interlocked process, or supervisory control loop can reach safe state if the link to the controlling host is severed. Cutting a network on a running furnace, press, reactor, or robot cell without a safe-state plan is itself an incident. Document the safe-state path per affected area.
3. **Sequence containment.** Standard order: (i) isolate the affected zone at the IT/OT boundary first (block at the firewall / segment gateway, not at the PLC), (ii) preserve forensic evidence (memory image of one infected host before reboot if possible — once rebooted, RAM is gone), (iii) disable shared service accounts and rotate domain admin / OT engineering credentials, (iv) disconnect remote-access vectors (vendor VPN, jump hosts, cellular modems on equipment), (v) air-gap critical OT segments only after safe-state is reached. Note explicitly which steps require operations sign-off.
4. **Inventory exposure.** List affected and at-risk assets with confidence level (confirmed / suspected / clean-pending-verification). Flag any asset that holds customer CUI (CMMC scope), PII, PHI, regulated batch records (21 CFR Part 11), or trade secrets — these change the notification clock.
5. **Start the regulatory and contractual clocks.** Lay out which clocks have started and when each notification is due. Typical set: CISA CIRCIA 72-hour for substantial incidents (24-hour if a ransom payment is made), DFARS 252.204-7012 72-hour for incidents on covered systems if a DoD contract is in scope, SEC Item 1.05 four-business-day clock once materiality is determined, state breach-notification clocks (varies — many at 30 / 45 / 60 days, some require "as soon as possible"), customer contractual clocks (often 24 / 48 / 72 hours), cyber-insurance prompt-notification clauses (often "as soon as practicable"). Do not classify materiality or covered-incident status — flag for outside counsel and report the question, not the answer.
6. **Decide on ransom posture early — but do not act on it.** US Treasury OFAC guidance prohibits payment to sanctioned actors; carrier and counsel should drive the call. Document the decision-needed item with the right escalation owner; do not negotiate.
7. **Plan recovery from a known-good baseline, not from the encrypted environment.** For each affected segment, list (a) last verified-restorable backup with restore-time test date, (b) PLC program of record with last verified upload, (c) HMI project of record, (d) network configuration of record, (e) identity / AD restore plan. Recovery without a restore-test history typically takes 2–4× longer than the IR plan estimates.
8. **Stage restart sequencing.** Production restart is not "turn it on." Sequence: (i) restore network segmentation in a clean state, (ii) restore identity, (iii) restore historian / control-room services, (iv) restore PLC programs with checksum verification, (v) restart utilities and ancillary first, (vi) restart product lines in low-risk-first order, (vii) run a release-to-production check on first articles before resuming customer shipments. Note the QA / regulatory hold posture during restart for FDA / AS9100 / IATF batches.
9. **Draft the four communications.** (i) Regulator-and-customer external-affairs statement (no admissions, no speculation, factual scope, named point of contact); (ii) workforce shift-huddle script (what to do, what not to do — do not click ransom links, do not reconnect personal devices, do not speak to media), explicit channel for reporting suspicious activity; (iii) customer SLA breach notification keyed to contract clause; (iv) cyber-insurance carrier first-notice-of-loss with policy number, broker, retained DFIR, retained counsel, preliminary scope, preliminary cause-narrative caveats. All four use the agreed-language master and avoid speculative attribution.
10. **Track decision points and open questions.** Maintain a running incident log with timestamps for every detection, containment action, decision, notification, and external escalation. The log is the artifact that goes into the after-action review and the insurer claim file.

## Output Requirements

- **Incident header:** plant, incident ID, classification (a–f from step 1), incident commander, time of detection, time of brief, OT lead, IT lead, EHS lead, retained DFIR, MSSP, insurance carrier
- **Life-safety status block:** every affected area with safe-state confirmed (yes / no / not applicable), responsible engineer, time confirmed
- **Containment sequence table:** step, scope, owner, planned time, executed time, evidence preserved (Y/N), safe-state precondition met (Y/N)
- **Asset exposure inventory:** asset, location, confidence (confirmed / suspected / clean-pending), data classification (CUI / PII / PHI / regulated / proprietary / none), action taken, action pending
- **Regulatory and contractual clock table:** clock, trigger event, due date / time, owner, status (not started / in progress / submitted), counsel review status — explicitly flag any that require legal classification before submission
- **Ransom-payment decision block:** posture (no payment / counsel evaluating / carrier driving), OFAC screening status, recorded decision-makers
- **Recovery baseline block:** per affected segment — last verified backup date, restore-test date, PLC program-of-record date, HMI project-of-record date, network config-of-record date, gaps
- **Restart sequence plan:** ordered steps with owners and gating checks (segmentation verified, identity verified, PLC checksum verified, first-article QA passed)
- **Communications set:** external-affairs statement, workforce huddle script, customer SLA notification (per affected customer), insurance first-notice-of-loss
- **Incident log:** running timestamped record of detection, decision, action, notification — designed to be exported to the DFIR file
- **Open questions and gaps:** explicit list of unknowns flagged for the next briefing cycle

## Anti-Patterns to Avoid

- **Do not** isolate an OT segment without first confirming the safe-state path. Cutting a network on a running press, furnace, reactor, or robotics cell can convert a containment action into a safety event.
- **Do not** reboot or power-cycle a suspected-infected host before forensic memory capture if a DFIR firm is reachable within the operational window. RAM-resident malware and decryption keys are often the only artifacts.
- **Do not** restore from a backup that has not been re-scanned against the threat indicators. Restoring an infected backup is the most-reported reason recoveries fail twice.
- **Do not** classify the event as "covered" or "material" in writing without outside counsel. CIRCIA and SEC classification calls have legal consequences and belong to counsel.
- **Do not** negotiate with a threat actor without the cyber-insurance carrier's panel firm or retained counsel. Payment outside that channel can void coverage and may breach OFAC.
- **Do not** speak publicly about attribution. Attribution is hard, often wrong in the first 72 hours, and almost never required for the immediate response. Stick to factual scope and remediation status.
- **Do not** treat the IT-side incident response plan as the OT plan. PLCs, HMIs, drives, and SIS need named procedures (golden-image / program-of-record / safe-state) that an IT plan does not contain.
- **Do not** rely on a ransom note's claim of data exfiltration. Confirm or refute through telemetry; the customer-notification posture changes materially based on whether data left the environment.
- **Do not** restart a regulated production line (FDA, AS9100, IATF, 21 CFR Part 11) without a documented release-to-production check on first articles and a documented batch-record reconciliation.
- **Do not** invent regulator names, statute numbers, clock thresholds, or breach-notification windows. Cite the specific clock and tag it as "to be confirmed by counsel" if the trigger condition is uncertain.

## Integration Notes

- **Pairs with Compliance Audit Prep** — incident response artifacts (plan, tabletop log, training records, post-incident review) are routinely sampled in ISO 27001, IATF 16949, AS9100, FDA, and CMMC audits. The audit-prep skill's evidence-mapping pass should pull from the IR artifact set produced here.
- **Pairs with Safety Incident & Near-Miss Report** — when a cyber event creates a process-safety incident (interlock bypass, runaway equipment, near-miss in a cell), both reports get filed and cross-reference each other.
- **Pairs with Supply Chain Risk Assessment** — vendor remote-access tooling, software update channels, and managed-service-provider footprints surfaced in the Critical / High supplier rows are common initial-access vectors and should be re-scored after any vendor-related incident.
- **Pairs with Supplier Communication Drafter** — the supplier outreach for forensic cooperation, vendor remote-access disablement, and software-update freeze-and-verify is a message pattern the drafter already handles.
- **Pairs with CMMC Level 2 self-assessment** — CMMC controls in the IR family (3.6.1 / 3.6.2 / 3.6.3) and the broader CSF 2.0 Respond / Recover function map directly to the artifacts produced by this skill.
- Most SMB manufacturers run one of CrowdStrike, SentinelOne, or Microsoft Defender on the IT side and one of Claroty, Dragos, Nozomi, Tenable.ot, or Armis on the OT side. If the target stack is known, produce the export keyed to its alert / asset schema; otherwise produce platform-neutral markdown with a CSV block keyed on asset / segment / status / action / owner.

## Success Metrics

- **Mean time to containment** (initial detection to confirmed isolation of the affected zone) — target under 4 hours for ransomware, under 8 hours for unauthorized PLC programming, under 24 hours for suspected silent intrusion
- **Safe-state confirmation rate** — 100% of affected OT areas with documented safe-state confirmation before isolation; zero containment-induced safety events
- **Notification clock compliance** — 100% on time across CISA CIRCIA, DFARS 7012 (if in scope), state breach jurisdictions, customer SLA, and insurer prompt-notice clauses
- **Restore-test currency** — every protected segment with a verified restore inside the last 90 days; no segment running on a backup that has never been restored end-to-end
- **PLC program-of-record currency** — every controller with a checksum-verified program-of-record inside the last 30 days
- **Tabletop cadence** — at least one OT-scenario tabletop per quarter with the IR team, plant manager, OT lead, IT lead, MSSP, DFIR firm, and a representative from outside counsel
- **Post-incident review close-out** — every incident with a documented after-action review inside 30 days and CAPA actions tracked to closure inside 90 days
