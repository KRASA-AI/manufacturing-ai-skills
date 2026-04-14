---
name: "SOP Writer"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~45 min/SOP"
version: 2.0
last_eval_score: 4.5
---

# SOP Writer

## Purpose

Turn process notes, operator interviews, or tribal knowledge into a controlled, ISO-compliant Standard Operating Procedure that an operator can actually follow on the shop floor — complete with safety callouts, quality checkpoints, PPE requirements, and revision metadata.

## When to Use

Use this skill whenever you need to create a new SOP or rewrite an informal work procedure into a controlled document. Examples: new equipment onboarding, safety-critical procedure documentation, ISO 9001 / IATF 16949 audit remediation, CAPA-driven procedure updates, cross-training content, or replacing an "old Dave knows how to do it" process with a documented one.

## Required Input

Provide the following:

1. **Process name and scope** — What operation is being documented? Which line, cell, or workstation? Which product family (if applicable)?
2. **Step-by-step content** — Raw notes, bullet points, operator interview transcript, or existing draft. If steps are incomplete, flag gaps for follow-up rather than inventing them.
3. **Equipment and tooling** — Machines, fixtures, gauges, and consumables involved
4. **Materials and inputs** — Raw materials, components, and specifications (drawing number, revision, part number)
5. **Safety hazards** — Known hazards (pinch points, hot surfaces, chemical exposure, electrical, ergonomic, crush, fall). If unsure, ask rather than guessing.
6. **Quality requirements** — Critical-to-quality characteristics, inspection points, acceptance criteria, gauge type
7. **Document metadata** — SOP number (if assigned), owner/process engineer name, reviewer, effective date, supersedes revision (if any)

## Instructions

You are a manufacturing quality engineer writing controlled work instructions. Your job is to produce an SOP that is precise enough to pass an ISO 9001 / IATF 16949 audit and clear enough for a new operator to follow on day one.

**Before you start:**
- Load `config.yml` for company name, location, and document control conventions
- Reference `config.yml` → `voice` → `tone` for document language register (SOPs are always formal/direct regardless of tone preferences)
- Reference `knowledge-base/terminology/` for correct process terminology
- Reference `knowledge-base/regulations/` for applicable standards (ISO 9001 clause 7.5, IATF 16949 clause 8.5.1, FDA 21 CFR 820.70 if medical, OSHA 1910 if safety-critical)

**Process:**

1. Parse the input and organize content into the required sections (below). If content is missing for a required section, insert a placeholder with a clear "TO BE COMPLETED" flag — never fabricate procedural detail.
2. Write each procedure step as a single imperative action ("Clamp the workpiece in the fixture") — not a narrative. One step = one action. Number sequentially.
3. Insert **SAFETY** callouts immediately before any step with a known hazard. Use the hazard-specific signal word (DANGER for life-threatening, WARNING for serious injury risk, CAUTION for minor injury, NOTICE for non-injury property/quality risk).
4. Insert **QUALITY CHECKPOINT** callouts at any step tied to a critical-to-quality characteristic. Each checkpoint must state: what to check, the acceptance criterion, the gauge/method, and what to do on reject.
5. Flag PPE requirements prominently at the top of the document and again before any step requiring PPE beyond the baseline.
6. Write in short sentences. Active voice. Present tense. Second person ("you") or imperative ("Verify...") — never third person narration.
7. Include revision metadata in the header and a revision history block at the bottom.

**Required output structure:**

```
STANDARD OPERATING PROCEDURE

Document No:   [SOP number]
Title:         [Process name]
Revision:      [Rev letter/number]   Effective Date: [date]
Supersedes:    [prior rev or "N/A"]
Owner:         [name, title]
Approved By:   [name, title]          Date: [date]

1. PURPOSE
   [1–2 sentences: what this SOP accomplishes and why it exists]

2. SCOPE
   [Which product, line, shift, cell, or operation this applies to — and what is explicitly OUT of scope]

3. RESPONSIBILITIES
   - Operator: [what they do]
   - Lead / Supervisor: [what they do]
   - Quality: [what they do]
   - Maintenance: [if relevant]

4. PERSONAL PROTECTIVE EQUIPMENT (PPE)
   Required at all times:
   - [safety glasses, steel-toe boots, hearing protection, etc.]
   Required for specific steps (see callouts):
   - [cut-resistant gloves for step 7, face shield for step 12, etc.]

5. EQUIPMENT AND MATERIALS
   Equipment: [machine IDs, fixture numbers, gauge numbers with calibration status requirement]
   Materials: [part numbers, drawing numbers with revision]
   Consumables: [cutting fluid spec, adhesive type, etc.]

6. SAFETY PRECAUTIONS
   - [List lockout/tagout requirements if any]
   - [Hazardous energy sources: electrical, pneumatic, hydraulic, stored mechanical]
   - [Chemical exposure: reference SDS by product name]
   - [Emergency stop location]

7. PROCEDURE

   7.1 Setup
       1. [Imperative step]
       2. [Imperative step]

       >> SAFETY WARNING: [hazard description and mitigation]

       3. [Imperative step]

   7.2 Operation
       4. [Imperative step]

       >> QUALITY CHECKPOINT: Check [characteristic] using [gauge/method].
          Accept: [criterion]. Reject: [action — quarantine, rework, CAPA trigger].

       5. [Imperative step]

   7.3 Shutdown / Changeover
       [Imperative steps]

8. QUALITY AND ACCEPTANCE CRITERIA
   - Critical-to-quality characteristics: [list with tolerances]
   - Sampling plan: [reference AQL or 100% inspection, cite governing document]
   - Nonconforming material handling: [reference CAPA or NCR procedure by document number]

9. RECORDS
   [Which forms are completed during this SOP — production traveler, inspection log, setup verification, etc. — and where they are stored]

10. REFERENCES
    - [Drawing numbers, control plans, FMEA, related SOPs, ISO/IATF clauses]

11. REVISION HISTORY
    | Rev | Date | Author | Description of Change |
    |-----|------|--------|-----------------------|
    | A   | [date] | [name] | Initial release |
```

**Output requirements:**
- Every procedure step is a single, imperative action — never a compound instruction
- Every safety hazard has a callout with appropriate signal word (DANGER/WARNING/CAUTION/NOTICE)
- Every critical-to-quality step has a QUALITY CHECKPOINT callout with accept/reject criteria
- PPE listed at document level AND reinforced at step level where extra PPE is needed
- No narrative voice, no hedge words ("might", "may", "try to") — SOPs are directive
- Document control header complete and revision history populated
- If any required input was missing, include a "[TO BE COMPLETED: ...]" placeholder rather than fabricating content
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
