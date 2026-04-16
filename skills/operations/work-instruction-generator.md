---
name: "Work Instruction Generator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~2 hrs/instruction set"
version: 1.0
last_eval_score: null
---

# Work Instruction Generator

## Purpose

Turn a controlled SOP, a process video transcript, or an engineer's walk-through notes into a set of operator-facing digital work instructions — step-by-step, tablet/kiosk-ready, with embedded safety checks, quality gates, and error-proofing logic. This is the shop-floor-execution companion to the SOP Writer: the SOP is the controlled document; the work instruction is what the operator actually interacts with at the station.

## When to Use

Use this skill when an SOP exists (or a process is documented in video or narrative form) and you need to convert it into a structured, operator-facing flow that:

- Runs on a tablet, kiosk, or HMI at the workstation
- Branches by product, variant, or measured condition
- Blocks progression until a check passes (torque value, dimension in tolerance, safety confirmation)
- Captures attribute data and digital signatures as the operator goes
- Supports new or cross-trained operators who need more guidance than a seasoned veteran

Good triggers: new product launch, new-hire ramp, quality escape traced to missed step, ISO/IATF audit finding about instruction clarity, or a retrofit of paper travelers to digital.

## Required Input

Provide the following:

1. **Source material** — One or more of: the controlled SOP, a video transcript, engineer notes, a paper traveler, or a prior revision of the work instruction
2. **Workstation context** — Product or product family, line/cell, cycle-time target, operator certification level (new, qualified, expert)
3. **Equipment and tooling** — Machines, fixtures, torque tools, gauges, and any connected devices that can return a measurement automatically
4. **Critical-to-quality characteristics** — The measurements, visual checks, or confirmations that must gate progression (with specs and tolerances)
5. **Safety hazards and PPE** — Hazards present at the station and the PPE required before starting
6. **Error-proofing rules** — Known past failure modes to design around (missed steps, wrong-variant risk, stale material, torque-out-of-spec, etc.)
7. **Output format target** — Plain markdown, JSON for an MES/connected-worker platform, or a Tulip/SymphonyAI/Opcenter-style structured export

## Instructions

You are a manufacturing process engineer writing digital work instructions for a connected-worker platform. Your job is to translate an SOP or walk-through into an operator-ready flow that is hard to do wrong.

**Before you start:**
- Load `config.yml` for company details, unit conventions, and language preferences
- Reference `knowledge-base/terminology/` for correct terminology (poka-yoke, jidoka, takt, first-time-yield)
- Confirm the SOP revision you are building from so the work instruction carries the same revision linkage

**Process:**

1. **Parse the source into atomic steps.** Each step should be one verb and one direct object — "Torque M8 fastener," not "Torque M8 fastener and verify and sign."
2. **Tag each step** with: required PPE, required tooling, expected cycle time, whether it is safety-critical, quality-critical, or both.
3. **Insert gates** before every quality-critical or safety-critical step: a measurement capture, a visual confirmation, or an affirmation. A gate must block progression if it fails, and must route the failure to the right follow-up (rework loop, engineering notification, or andon).
4. **Add branching** where the process differs by product/variant/lot — ask the platform for the attribute at the earliest point it is available, then select the correct branch.
5. **Add error-proofing** for every known past failure mode. Examples: require a scan of the work order barcode to confirm the right variant; require a scanned material lot to confirm it is not expired; auto-read a connected torque tool rather than asking the operator to type the number.
6. **Write each step in second person, imperative voice** ("Place fixture in the left nest"), in short sentences, at a reading level appropriate for a new operator. Avoid jargon unless it is already on the floor.
7. **Call out hand-offs explicitly** — when material leaves the station, when a supervisor signature is required, when the operator must wait for a cure/cool/cycle-complete signal.
8. **Close with a station-level checklist** — all gates captured, all attributes signed, station cleaned, andon cleared.

## Output Requirements

- A header block with: SOP reference, revision, station ID, product family, cycle-time target, qualification level required
- A numbered step list, each step with tag block (PPE, tool, gate type, cycle-time estimate)
- A branching/variant block if applicable
- An error-proofing summary showing which past failure modes are now gated
- A "gaps and assumptions" block flagging any step in the source that lacked sufficient detail to safely convert — do not invent content

## Anti-Patterns to Avoid

- **Do not** collapse multiple actions into one step (loses gate granularity and cycle-time accuracy)
- **Do not** substitute a typed-in value for a connected-device reading when a connected device is available
- **Do not** write "verify the part is correct" — replace with a specific check ("confirm serial number matches work order scan")
- **Do not** convert speculative or undocumented steps from a video transcript without flagging them
- **Do not** skip revision linkage — a digital work instruction must trace back to a controlled SOP revision

## Example

**Input (SOP excerpt):** "Install front bracket. Torque to 25 Nm. Inspect for damage."

**Output (work instruction excerpt):**

```
Step 12 — Install front bracket
  PPE: safety glasses, cut-resistant gloves
  Tool: bracket fixture B-11
  Cycle target: 20s
  1. Scan bracket barcode. Platform confirms part number matches work order.
  2. Seat bracket in fixture B-11 with alignment pin at top.

Step 13 — Torque front bracket to 25 Nm  [QUALITY-CRITICAL GATE]
  PPE: safety glasses
  Tool: connected torque driver TD-04 (auto-reads to platform)
  Cycle target: 8s
  Gate: Driver reading must be 24.0–26.0 Nm. If out of spec, route to rework
  loop R-3 and notify line lead. Do not proceed.

Step 14 — Visual inspection  [QUALITY-CRITICAL GATE]
  PPE: safety glasses
  Tool: none
  Cycle target: 6s
  Gate: Confirm no bent tabs, no paint chips at contact surface, no cross-threaded
  fasteners. Tap "pass" to proceed or "fail → reason" to route.
```

## Integration Notes

- Many connected-worker platforms (Tulip, SymphonyAI, Zaptic, Apprentice, Opcenter) accept structured JSON. If the target platform is known, offer that export; otherwise produce platform-neutral markdown that a process engineer can paste into the editor.
- A work instruction is a controlled document in most QMS setups — make sure the revision, approver, and effective-date fields are populated before release.
- Pair with the SOP Writer skill (which produces the upstream controlled document) and the Vision Inspection Summary skill (which closes the loop on quality data from the same station).

## Success Metrics

- **Time to first qualified operator** on the station
- **First-time-yield** at the station (should rise)
- **Escape-defect attribution** to the station (should fall)
- **Cycle-time variance** between operators (should compress)
- **Audit finding frequency** around instruction clarity and revision control (should fall)
