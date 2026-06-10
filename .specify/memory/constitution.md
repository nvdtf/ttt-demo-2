<!--
  ===================== Sync Impact Report =====================
  Version change: N/A → 1.0.0 (initial adoption)

  Modified principles: (all new)
    - I. Human Spec Authority
    - II. Escalation Discipline
    - III. Deployable Probe First
    - IV. Static Vanilla Stack
    - V. Requirement Provenance
    - VI. Spec-Bound Review

  Added sections:
    - Core Principles (6 principles)
    - Escalation Protocol
    - Review Process
    - Governance

  Removed sections: None (initial adoption)

  Templates requiring updates:
    - .specify/templates/plan-template.md        ✅ compatible (Constitution Check is a dynamic gate)
    - .specify/templates/spec-template.md         ✅ compatible (generic requirement structure)
    - .specify/templates/tasks-template.md        ✅ compatible (phase structure accommodates probe-first)
    - .specify/templates/commands/*.md            ✅ no command files exist

  Follow-up TODOs: None
  ==============================================================
-->

# TTT Demo 2 Constitution

## Core Principles

### I. Human Spec Authority

Humans decide WHAT to build and WHY at specification altitude.
Agents own HOW the specification is realized in code.

- Specifications, acceptance criteria, and prioritization MUST
  originate from or be ratified by a human decision-maker.
- Agents MUST NOT unilaterally redefine scope, reorder priorities,
  or alter success criteria.
- Agents have full autonomy over implementation details: file
  structure, algorithms, variable names, and code style — provided
  the result satisfies the spec.

### II. Escalation Discipline

Workers escalate ONLY when one of these conditions is met — and
nothing else:

1. **Spec ambiguity forcing a decision** — the spec is silent or
   contradictory on a point that materially affects the outcome,
   and no reasonable default exists.
2. **Constitutional conflict** — fulfilling the spec would require
   violating another principle in this constitution.
3. **Unachievable acceptance criteria** — a stated acceptance
   criterion cannot be met within the declared technology
   constraints or is logically impossible.

All other uncertainties MUST be resolved by the worker using
reasonable judgment. Spurious escalations waste human attention
and are themselves a defect.

### III. Deployable Probe First

The first milestone of every feature MUST be a deployable probe:
a minimal, end-to-end slice that can be opened in a browser (or
equivalent target) and manually verified.

- The probe proves the critical path works before any polish.
- No feature is considered "started" until its probe is live.
- Subsequent milestones build on the probe incrementally.

### IV. Static Vanilla Stack

The project MUST be a static site using vanilla JavaScript with
no build step and no backend.

- No transpilers, bundlers, or compile-to-JS languages.
- No server-side runtime (Node, Deno, Python, etc.) in production.
- All assets MUST be servable from a static file host.
- Third-party libraries loaded via CDN `<script>` tags are
  permitted only when hand-rolling the equivalent would be
  unreasonable; each inclusion MUST be justified in the spec.

### V. Requirement Provenance

Every requirement MUST carry provenance: who decided it, and at
what weighted percentage of influence.

- Format: `[Requirement] — decided by <actor> @ <weight>%`
- Weights across all actors for a single requirement MUST sum
  to 100%.
- Provenance MUST be recorded at specification time and preserved
  through planning and task generation.
- Absence of provenance on a requirement is a spec defect that
  MUST be resolved before implementation begins.

### VI. Spec-Bound Review

The reviewer arbitrates against the specification only.

- Acceptance or rejection MUST cite specific spec requirements or
  acceptance criteria.
- Style decisions on which the spec is silent (naming, formatting,
  code organization) are the worker's call and MUST NOT be grounds
  for rejection.
- If a reviewer believes a spec-silent concern is important enough
  to block, they MUST first amend the spec — then reject against
  the amended spec.

## Escalation Protocol

This section operationalizes Principle II.

| Trigger | Required in escalation |
|---------|----------------------|
| Spec ambiguity | Quote the ambiguous text; state the two+ interpretations; recommend one |
| Constitutional conflict | Name the conflicting principles; explain why both cannot be satisfied |
| Unachievable criteria | State the criterion; cite the constraint that makes it impossible |

Workers MUST NOT escalate for:
- Stylistic preferences
- "Nice to have" improvements not in the spec
- Uncertainty that can be resolved by reading existing code or docs

## Review Process

This section operationalizes Principle VI.

1. The reviewer MUST read the spec before reviewing code.
2. Each review comment MUST reference a spec requirement (by ID)
   or acceptance criterion.
3. Comments on spec-silent matters MAY be left as non-blocking
   suggestions but MUST NOT block merge.
4. If the reviewer identifies a gap in the spec, the correct
   action is to file a spec amendment — not to reject the
   current implementation.

## Governance

This constitution supersedes all ad-hoc practices. Amendments
follow this procedure:

1. **Propose**: Draft the change with rationale and affected
   principles.
2. **Review**: All active human stakeholders MUST review.
   Agents MAY comment but do not vote.
3. **Adopt**: Requires explicit human approval. Update the
   version number per semantic versioning:
   - **MAJOR**: Principle removed or fundamentally redefined.
   - **MINOR**: New principle added or existing principle
     materially expanded.
   - **PATCH**: Clarifications, wording, or formatting fixes.
4. **Propagate**: After adoption, verify all dependent templates
   (plan, spec, tasks) remain consistent. Update the Sync
   Impact Report at the top of this file.

Compliance review: every spec and plan MUST include a
"Constitution Check" section verifying alignment with the
active principles before implementation begins.

**Version**: 1.0.0 | **Ratified**: 2026-06-10 | **Last Amended**: 2026-06-10
