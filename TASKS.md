# TASKS — curriculum-standards-map

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-29 · Owner: TBD (maintainer) · Lane: donated
> v0.2 adds tasks from the merged competitive analysis (StandardAssociation layer, vocab-mapping table,
> CCSS © notice field, ingest contract + importer, CASE-primary identifiers, NGSS un-defer).

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug ID from the tables (e.g. `curriculum-standards-map-model-001`).
- `title` — the table's Title.
- `project` — `curriculum-standards-map`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (per table).
- `lane` — `donated` for all tasks here (no funded escrow). A funded task would add `fundedBudgetUsd`.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["education","oer","open-standards"]`.
- `riskTier` — `low | medium | high`. This project is **low** in the Elyos sense; alignment/licence/
  framework-terms judgement tasks are still authored carefully and gated, but remain `low` (no
  medical/legal/safety advice, no PII).
- `urgent` — boolean; `false` for all current tasks.
- `deliverable` — `pr | dataset | document | translation`. The **crosswalk itself is a `dataset`**
  (we author the alignment records *and* the `StandardAssociation` records, derived from open inputs).
  Model/templates/policies → `document`; code (validator, exporters, proposer, importer) → `pr`.
- `tokenEstimate` — `small | medium | large` (Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — **TO BE SECURED** until a consuming partner is confirmed.
- `verifiedNeed` — **`false`** until a named consumer (repository/district/platform) commits to use
  the crosswalk (the general need is real; per-task delivery need is unproven).
- `outputLicense` — **`CC-BY-4.0`** for the crosswalk dataset, templates, and documentation;
  **`MIT`** for code (validator, exporters, proposer).

Two blocking gates apply to mapping tasks before any record ships: the **dual licence/terms gate**
(`gate-002`, OER licence + framework terms) and **educator review** (every record). No mapping task
is pre-approved by appearing here.

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| curriculum-standards-map-model-001 | Alignment data model + authoring template (canonical `AlignmentRecord`) | writing | small | low | document | — | Technical |
| curriculum-standards-map-gate-002 | Dual licence/terms gate checklist (OER licence + framework terms) | design-spec | small | low | document | — | Licence/terms |
| curriculum-standards-map-ncpolicy-003 | NC (CC-BY-NC) acceptance policy for OER + downstream-commercial caveat | design-spec | small | low | document | gate-002 | Licence/terms |
| curriculum-standards-map-reviewer-004 | Name/secure the Licence/terms + Educator reviewers (blocking gate roles) | research | small | low | document | — | Maintainer |
| curriculum-standards-map-validator-005 | Schema validator + identifier-resolution checks (CASE GUID / ASN URI) | code | medium | low | pr | model-001 | Technical |
| curriculum-standards-map-export-006 | schema.org `AlignmentObject` exporter + golden fixtures | code | small | low | pr | model-001 | Technical |
| curriculum-standards-map-registry-007 | Frameworks registry + OER source registry (licence-classified) | research | small | low | document | gate-002 | Maintainer |
| curriculum-standards-map-outreach-008 | Consumer/partner outreach + baseline time-to-find & per-record review-minutes measurement | research | small | low | document | — | Steward |
| curriculum-standards-map-pilot-009 | Pilot slice: ≥50 educator-reviewed alignments (clean framework × CC-BY OER) | data | medium | low | dataset | model-001, gate-002, ncpolicy-003, reviewer-004, validator-005, export-006, registry-007 | Licence/terms, Educator |

**Acceptance criteria — key tasks**

- **model-001 (alignment data model + template)**
  - [ ] Canonical `AlignmentRecord` documented with all fields from PLAN: `oer{...,license{id,url,
        permitsDerivatives,commercialUse,snapshotRef},...}`, `standard{frameworkId,statementId,
        caseGuid,asnUri,frameworkTerms{license,permitsReference,permitsTextReproduction,ref}}`,
        `alignment{type,strength,coverage,evidence,aiConfidence}`, `review{status,reviewedBy,
        reviewerQualification,reviewedAt,auditedBy}`, `outputLicense`.
  - [ ] Confidence model documented: `strength` (exact/strong/partial/tangential) + `coverage`
        (full/partial); states AI score is advisory and educator `accepted` is the ship signal.
  - [ ] States deliverable is the mapping, not the content; reference-by-identifier is the default
        (**CASE GUID primary/required-where-available; ASN URI optional legacy capture**);
        `statementText` included only where `permitsTextReproduction:true`, with any mandated © notice
        (e.g. CCSS) as a required field.
  - [ ] Second record type **`StandardAssociation`** documented (from/to standards + `associationType`
        `exactMatch|isEquivalentTo|isRelatedTo|isPartOf|supersedes`), reusing the same gates + review;
        carries `notEndorsement:true` (a reviewed proposal, never an official equivalence claim).
  - [ ] **Vocabulary-mapping table** documented (our `type`/`strength`/`associationType` → schema.org
        `alignmentType` → CASE `CFAssociationType`) as the single source the exporters enforce.
  - [ ] Record-level metadata (`strength`, `coverage`, `aiConfidence`, `reviewerQualification`,
        `notEndorsement`) is specified to survive into every export; `aiConfidence` noted as not
        comparable across proposer model versions (record model+version in provenance).
  - [ ] One worked example of each record type included; `outputLicense: CC-BY-4.0`.

- **gate-002 (dual licence/terms gate)**
  - [ ] OER licence check: PASS only if licence is accepted (CC0/CC-BY/CC-BY-SA, or NC under
        `ncpolicy-003`) with `permitsDerivatives` + `commercialUse` recorded from a cited URL/clause
        and a snapshot (committed copy + SHA-256 + Wayback). Missing/unparseable = FLAG/EXCLUDE.
  - [ ] Framework-terms check: records `permitsReference` and `permitsTextReproduction` per
        framework from cited terms; `permitsReference:false` ⇒ framework EXCLUDED; reproduction only
        where explicitly permitted, with any **mandated © notice string captured in the artifact**
        (e.g. the verbatim CCSS "© 2010 NGA Center/CCSSO …" notice). NGSS assessed under the corrected
        premise (NAP copyright, WestEd trademark, broad non-profit reproduction right).
  - [ ] Standards skeletons must come from official machine-readable sources — **seeded from the CASE
        Network 50-state registry** where available — / official open data; PDF-scraping copyrighted
        standards is forbidden.
  - [ ] Produces a committed PASS/FLAG/EXCLUDE artifact per (OER source × framework) slice.

- **reviewer-004 (secure gate roles)**
  - [ ] A named Licence/terms reviewer who can read CC/OGL/CCSS/ACARA terms and apply `ncpolicy-003`.
  - [ ] At least one named Educator reviewer per in-scope subject + grade band, qualification
        recorded; second-reviewer/auditor identified for the precision audit.
  - [ ] Until filled, all tasks stay `verifiedNeed:false` and no record can pass review; roles
        recorded in PLAN governance section.

- **pilot-009 (pilot slice, end-to-end)**
  - [ ] Slice chosen licence-clean on both sides with machine-readable identifiers (e.g. Australian
        Curriculum (CC-BY) or England NC (OGL) × a CC-BY OER for one subject/grade band).
  - [ ] Slice passed `gate-002` (OER licence + framework terms) with the artifact committed.
  - [ ] ≥ 50 alignment records produced as candidates with evidence + confidence, then **educator-
        reviewed and accepted**; each record validates against the schema and every CASE GUID / ASN
        URI resolves.
  - [ ] Records reference standards by identifier; `statementText` present only where permitted.
  - [ ] Provenance recorded both sides (OER source/version/licence-snapshot; framework version/terms).
  - [ ] Published as an open CC-BY release with a coverage ledger; **delivered** to a consumer with
        the Steward's delivery-evidence artifact if one materialises, else released self-serve with
        the blocker surfaced. `verifiedNeed` flips to `true` only on confirmed consumer use.

**M0 Definition of Done:** gate roles named (blocking, before pilot review); `AlignmentRecord` model
+ template + dual licence/terms gate + NC policy + frameworks/OER registries published; schema
validator (with identifier resolution) and the schema.org exporter green in CI with golden fixtures;
≥ 50 educator-reviewed alignments for one clean pilot slice, schema-valid and identifier-resolving,
published as an open CC-BY release with a coverage ledger — delivered to a consumer (evidence
recorded) or released self-serve with the blocker surfaced; ≥ 1 outreach thread opened; baseline
time-to-find **and per-record review-minutes** measured; standards skeleton seeded from the CASE
Network registry; model includes the `StandardAssociation` type + vocabulary-mapping table.

---

## Milestone M1 — Gates hardened + interop exports + first delivery

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| curriculum-standards-map-snapshot-010 | Licence/terms snapshot capture in provenance flow | code | small | low | pr | model-001, gate-002 | Technical |
| curriculum-standards-map-case-011 | CASE-compatible exporter (1EdTech CASE v1.1) + golden fixtures | code | medium | low | pr | model-001, export-006 | Technical |
| curriculum-standards-map-csv-012 | CSV exporter + identifier columns + validation | code | small | low | pr | model-001 | Technical |
| curriculum-standards-map-framework-013 | Onboard a 2nd framework through the framework-terms gate | research | small | low | document | gate-002, registry-007 | Licence/terms |
| curriculum-standards-map-map-014 | ≥ 250 educator-reviewed alignments across ≥ 2 subjects | data | large | low | dataset | pilot-009, framework-013 | Licence/terms, Educator |
| curriculum-standards-map-audit-015 | Independent second-reviewer precision audit (sampled) | research | small | low | document | pilot-009, map-014 | Educator (auditor) |
| curriculum-standards-map-partner-016 | Secure first confirmed consuming partner | research | small | low | document | outreach-008 | Steward |
| curriculum-standards-map-ingest-032 | Ingest contract + sample importer (Ed-Fi `LearningStandard` + schema.org `educationalAlignment`) | code | small | low | pr | case-011, csv-012 | Technical, Steward |

**Acceptance criteria — key tasks**

- **ingest-032 (ingest contract + sample importer)**
  - [ ] Documents a tiny "ingest contract" (required columns/fields, identifiers, licence/notice
        fields) so a consuming partner's lift is near-zero — directly attacks the unsecured-partner /
        "delivered, not merged" risk.
  - [ ] Ships one sample importer for **Ed-Fi `LearningStandard`** and one for **schema.org
        `educationalAlignment`**, each with a golden round-trip fixture; code MIT; CI green; no credentials.

- **case-011 (CASE exporter)** *(pattern also applies to csv-012)*
  - [ ] Emits valid **CASE v1.1** JSON for framework/standard references **and `StandardAssociation`
        records** (`CFAssociationType`), version in `specVersions`.
  - [ ] Applies the **vocabulary-mapping table** from model-001 (our vocab → schema.org `alignmentType`
        → CASE `CFAssociationType`); confirms the exact CASE association terms against pinned v1.1.
  - [ ] Record-level `strength`/`coverage`/`aiConfidence`/`reviewerQualification`/`notEndorsement`
        survive into the export (consumer cannot render a reviewed proposal as authoritative fact); any
        mandated © notice (CCSS) is emitted with reproduced `statementText`.
  - [ ] Ships committed golden canonical-record → expected-CASE pairs, diffed in CI and validated
        against the pinned CASE schema.
  - [ ] Code MIT; `pnpm build && pnpm test && pnpm lint` green; DCO signed-off; no credentials.

- **map-014 (≥ 250 reviewed alignments, ≥ 2 subjects)**
  - [ ] Every record passed `gate-002` and **educator review** (accepted), with evidence + strength +
        coverage recorded; AI score never the ship signal.
  - [ ] Every CASE GUID / ASN URI resolves (resolver green); records schema-valid; exports validate.
  - [ ] Provenance complete both sides; reference-by-identifier respected; reproduction only where
        permitted.
  - [ ] Delivered to / published for the confirmed consumer (`partner-016`) with the Steward's
        delivery-evidence artifact; coverage ledger updated.

- **audit-015 (precision audit)**
  - [ ] An independent educator (not the original reviewer) re-checks a random sample of shipped
        records; precision (correct ÷ sampled) ≥ 95% recorded; misses become `needs-fix` records.
  - [ ] Audit method + sample size + result committed to the outcome ledger.

- **partner-016 (first confirmed consumer)**
  - [ ] A named repository/district/platform confirms it will ingest or publish the crosswalk records.
  - [ ] Preferred ingest format recorded (pins exporter priority); contribution mechanism documented.
  - [ ] Tasks for that consumer updated to `verifiedNeed:true` with `requestor` set.

**M1 Definition of Done:** dual licence/terms gate applied across ≥ 2 frameworks; CASE + CSV
exporters validated against pinned specs with golden fixtures; all shipped identifiers resolve; ≥ 1
confirmed consuming partner; ≥ 300 reviewed records (counting M0+M1) across ≥ 2 subjects delivered/
ingested; precision audit ≥ 95%.

---

## Milestone M2 — AI-assisted candidate generation & scale

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| curriculum-standards-map-ingest-017 | OER ingest under the bounded-access protocol (metadata + short evidence) | code | medium | low | pr | model-001, gate-002 | Technical |
| curriculum-standards-map-proposer-018 | AI candidate-alignment proposer (evidence + confidence → review queue) | code | large | low | pr | ingest-017, validator-005 | Technical, Educator |
| curriculum-standards-map-roster-019 | Educator reviewer roster (subjects × grade bands) sized for scale, using the per-record review-minutes baseline | research | small | low | document | reviewer-004 | Maintainer |
| curriculum-standards-map-map-020 | Scale to ≥ 600 reviewed records cumulatively (proposer-assisted) | data | large | low | dataset | proposer-018, roster-019 | Licence/terms, Educator |
| curriculum-standards-map-coverage-021 | Coverage ledger: ≥ 60% standards in one grade-band × subject have a reviewed OER | data | small | low | document | map-020 | Maintainer |
| curriculum-standards-map-assoc-033 | First standard↔standard association batch (e.g. CCSS↔a state, or AU↔England), expert-reviewed | data | medium | low | dataset | model-001, gate-002, map-014 | Licence/terms, Educator |

**Acceptance criteria — key tasks**

- **assoc-033 (standard↔standard association batch)**
  - [ ] `StandardAssociation` records for a chosen framework pair, each with `associationType`,
        `strength`, `coverage`, evidence, `aiConfidence`, and **expert review** (`accepted`); both
        endpoints reference standards by identifier (CASE GUID primary) that resolve in the validator.
  - [ ] Both frameworks passed `gate-002`; records carry `notEndorsement:true` (reviewed proposals, not
        official equivalence); CASE/schema.org exports validate; coverage/provenance complete.

- **proposer-018 (AI candidate proposer)**
  - [ ] Emits `(oer, standard)` candidates each with alignment `type`, `aiConfidence`, and a short
        `evidence` rationale into a **review queue**; **never auto-publishes** a record.
  - [ ] Output is clearly labelled "candidate — requires educator review"; reviewer must record
        evidence/decision (guards against automation bias).
  - [ ] Measured reviewer throughput (records/hour) improves vs. the manual M0/M1 baseline **without**
        precision dropping below 95% on the next audit.
  - [ ] Code MIT; tests + CI green; bounded-access protocol respected (no rehosted OER corpus).

- **map-020 (scale to ≥ 600 reviewed)**
  - [ ] ≥ 600 records cumulatively, all educator-accepted with evidence; gate + provenance complete.
  - [ ] Precision audit re-run and ≥ 95%; any regression halts scaling until fixed.
  - [ ] Delivered to a consumer; coverage ledger updated.

**M2 Definition of Done:** proposer feeds a review queue (no auto-publish); measured reviewer
throughput gain with precision held ≥ 95%; ≥ 600 reviewed records cumulatively across ≥ 2
frameworks; coverage ledger shows ≥ 60% of standards in at least one grade-band × subject have ≥ 1
reviewed aligned OER.

---

## Milestone M3 — Coverage, reuse outcomes & sustainability

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| curriculum-standards-map-reuse-022 | Track + verify external reuse events | research | small | low | document | map-014, map-020 | Steward |
| curriculum-standards-map-refresh-023 | Staleness/refresh: OER drift + standards re-publication detection | maintenance | medium | low | pr | validator-005, case-011 | Maintainer |
| curriculum-standards-map-usability-024 | Usability test: ≥ 50% time-to-find reduction vs. baseline | research | small | low | document | outreach-008, map-020 | Steward |
| curriculum-standards-map-scale-025 | Reach ≥ 1,000 reviewed records (≥ 2 frameworks, ≥ 2 subjects) | data | large | low | dataset | map-020, refresh-023 | Licence/terms, Educator |

**Acceptance criteria — key tasks**

- **refresh-023 (staleness/refresh)**
  - [ ] Link-check detects OER that moved/changed (404/content drift vs. recorded version+snapshot).
  - [ ] Detects standards framework re-publication (CASE version bump / changed identifiers);
        affected records flagged for possible re-review as `maintenance` tasks.
  - [ ] Coverage ledger regenerated; dataset re-released with bumped semver + date.

- **usability-024 (time-to-find)**
  - [ ] Small consented usability test with volunteer educators; no PII retained beyond aggregate
        timings; ≥ 50% reduction vs. the M0 baseline recorded with method + sample size.

- **reuse-022 (reuse tracking)**
  - [ ] ≥ 2 verifiable external reuse events (ingestion commit/permalink, catalog page, written
        confirmation, or third-party fork/citation); self-reported use excluded.

**M3 Definition of Done:** ≥ 2 verifiable external reuse events; ≥ 1,000 reviewed records delivered
cumulatively across ≥ 2 frameworks and ≥ 2 subjects; staleness/refresh process working; usability
test shows ≥ 50% time-to-find reduction; steward identified for ongoing liaison and outcome tracking.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| curriculum-standards-map-ngss-026 | Onboard NGSS once reproduction/reference terms confirmed | research | small | low | document | **Expected admissible** (NAP copyright, WestEd trademark, broad non-profit reproduction right). Gated on written confirmation of the binding limits (trademark, commercial-use, attribution) — corrected from "Achieve copyright / likely no reproduction" |
| curriculum-standards-map-lms-027 | LMS/OER-repo ingest adapter for a confirmed consumer's format | code | medium | low | pr | Pins to the partner's preferred format |
| curriculum-standards-map-i18n-028 | Map a non-English-medium framework (domain + language reviewer) | data | large | low | dataset | Widens reach; needs bilingual educator reviewer |
| curriculum-standards-map-dash-029 | Public coverage + outcome dashboard, incl. **link-rot / CASE-GUID-drift** signal (CI-generated) | code | medium | low | pr | Supports success-metric tracking; turns the staleness risk into a visible, fixable, reuse-trust signal |
| curriculum-standards-map-community-030 | Community contribution workflow under the same dual-gate + audit | design-spec | small | low | document | Scale beyond core team without lowering precision |
| curriculum-standards-map-funded-031 | Budget-capped funded proposer variant (batch candidate generation) | code | medium | low | pr | Funded lane; would add `fundedBudgetUsd`; out of v0.1.0 scope |

---

## Example task JSON

```json
{
  "id": "curriculum-standards-map-model-001",
  "title": "Alignment data model + authoring template (canonical AlignmentRecord)",
  "project": "curriculum-standards-map",
  "type": "writing",
  "lane": "donated",
  "priority": "high",
  "domain": ["education", "oer", "open-standards", "curriculum"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "OER are abundant and free but are not indexed against the curriculum standards that govern what teachers must teach, so teachers cannot find the free resource aligned to a given standard. This project builds an open crosswalk of educator-reviewed alignment records linking OER to standards by stable identifier (CASE GUID / ASN URI), not by reproducing copyrighted standards text. Before mapping any slice, the project needs one canonical AlignmentRecord model and authoring template that every export (CSV, JSON, schema.org AlignmentObject, CASE) is a projection of. The deliverable is the mapping, not the content: we link to OER at source and reference standards by identifier.",
  "objective": "Create the canonical AlignmentRecord data model and the authoring template that every per-slice mapping task and exporter will use.",
  "acceptanceCriteria": [
    "Canonical AlignmentRecord documents all fields: oer {id, title, url, publisher, license {id, url, permitsDerivatives, commercialUse, snapshotRef}, granularity, subject, gradeLevels[], provenance {retrievedAt, version}}, standard {frameworkId, statementId, caseGuid, asnUri, frameworkTerms {license, permitsReference, permitsTextReproduction, ref}}, alignment {type, strength, coverage, evidence, aiConfidence}, review {status, reviewedBy, reviewerQualification, reviewedAt, auditedBy}, outputLicense.",
    "Confidence model documented: strength (exact/strong/partial/tangential) and coverage (full/partial); states explicitly that aiConfidence is advisory and the ship signal is educator review.status=accepted.",
    "Template states the deliverable is the mapping not the content, that standards are referenced by identifier by default, and that statementText is included only where frameworkTerms.permitsTextReproduction is true.",
    "At least one fully worked example AlignmentRecord is included to guide contributors.",
    "Crosswalk/template output licensed CC-BY-4.0; any committed tooling passes pnpm build && pnpm test && pnpm lint and the commit is DCO signed-off."
  ],
  "resources": [
    "C:\\code\\elyos\\planning\\projects\\curriculum-standards-map\\PLAN.md",
    "C:\\code\\elyos\\packages\\schema\\src\\schemas.ts",
    "1EdTech CASE v1.1 specification",
    "Achievement Standards Network (ASN) URIs",
    "schema.org AlignmentObject / LearningResource; LRMI vocabulary"
  ],
  "output": "A canonical AlignmentRecord data-model definition plus a Markdown authoring template, committed to the project repo and ready for reuse by per-slice mapping tasks and exporters.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Task rollup

- **27 scheduled tasks** across M0–M3 (M0: 9 · M1: 8 · M2: 6 · M3: 4) + **6 backlog** = **33 total**
  (v0.2 added `ingest-032` ingest contract + importer and `assoc-033` standard↔standard batch).
- Deliverables: crosswalk **`dataset`** tasks (the alignment records) are gated behind both the dual
  licence/terms gate and educator review; **`document`** for model/policies/registries/audits;
  **`pr`** for validator/exporters/proposer/refresh.
- All tasks start `verifiedNeed:false`, `requestor: TO BE SECURED`, `lane: donated`, `riskTier: low`.
- Output licences: **CC-BY-4.0** (dataset/docs), **MIT** (code).
- Hard gates, never skipped: (1) dual licence/terms (`gate-002`), (2) educator review (every
  record), with a blind second-reviewer precision audit (`audit-015`) preserving ≥ 95% precision.

---

## Generated task index

Every table row above is now backed by a schema-valid machine-readable `tasks/<id>.json` (validated
against `packages/schema`; the Elyos CLI executes the JSON, not this table). The **full per-task
acceptance criteria live in each `tasks/<id>.json`** and reuse the checkable bullets from the
"Acceptance criteria — key tasks" sections above; the JSONs are authoritative for execution.

**Fan-out:** none — this backlog already enumerates discrete tasks. The mapping milestones
(`pilot-009`, `map-014`, `map-020`, `scale-025`) are **count-based** targets, not per-item fan-out,
and framework onboarding is already split into discrete rows (`framework-013`, `ngss-026`,
`i18n-028`). No languages, datasets, frameworks, or beneficiaries were fabricated. `funded-031` stays
`lane: donated` because PLAN.md defines no escrow/budget cap yet (it is out of v0.1.0 scope); it flips
to `funded` with `fundedBudgetUsd` set only when scoped.

| ID | JSON |
| --- | --- |
| curriculum-standards-map-model-001 | `tasks/curriculum-standards-map-model-001.json` (seed) |
| curriculum-standards-map-gate-002 | `tasks/curriculum-standards-map-gate-002.json` |
| curriculum-standards-map-ncpolicy-003 | `tasks/curriculum-standards-map-ncpolicy-003.json` |
| curriculum-standards-map-reviewer-004 | `tasks/curriculum-standards-map-reviewer-004.json` |
| curriculum-standards-map-validator-005 | `tasks/curriculum-standards-map-validator-005.json` |
| curriculum-standards-map-export-006 | `tasks/curriculum-standards-map-export-006.json` |
| curriculum-standards-map-registry-007 | `tasks/curriculum-standards-map-registry-007.json` |
| curriculum-standards-map-outreach-008 | `tasks/curriculum-standards-map-outreach-008.json` |
| curriculum-standards-map-pilot-009 | `tasks/curriculum-standards-map-pilot-009.json` |
| curriculum-standards-map-snapshot-010 | `tasks/curriculum-standards-map-snapshot-010.json` |
| curriculum-standards-map-case-011 | `tasks/curriculum-standards-map-case-011.json` |
| curriculum-standards-map-csv-012 | `tasks/curriculum-standards-map-csv-012.json` |
| curriculum-standards-map-framework-013 | `tasks/curriculum-standards-map-framework-013.json` |
| curriculum-standards-map-map-014 | `tasks/curriculum-standards-map-map-014.json` |
| curriculum-standards-map-audit-015 | `tasks/curriculum-standards-map-audit-015.json` |
| curriculum-standards-map-partner-016 | `tasks/curriculum-standards-map-partner-016.json` |
| curriculum-standards-map-ingest-017 | `tasks/curriculum-standards-map-ingest-017.json` |
| curriculum-standards-map-proposer-018 | `tasks/curriculum-standards-map-proposer-018.json` |
| curriculum-standards-map-roster-019 | `tasks/curriculum-standards-map-roster-019.json` |
| curriculum-standards-map-map-020 | `tasks/curriculum-standards-map-map-020.json` |
| curriculum-standards-map-coverage-021 | `tasks/curriculum-standards-map-coverage-021.json` |
| curriculum-standards-map-reuse-022 | `tasks/curriculum-standards-map-reuse-022.json` |
| curriculum-standards-map-refresh-023 | `tasks/curriculum-standards-map-refresh-023.json` |
| curriculum-standards-map-usability-024 | `tasks/curriculum-standards-map-usability-024.json` |
| curriculum-standards-map-scale-025 | `tasks/curriculum-standards-map-scale-025.json` |
| curriculum-standards-map-ngss-026 | `tasks/curriculum-standards-map-ngss-026.json` |
| curriculum-standards-map-lms-027 | `tasks/curriculum-standards-map-lms-027.json` |
| curriculum-standards-map-i18n-028 | `tasks/curriculum-standards-map-i18n-028.json` |
| curriculum-standards-map-dash-029 | `tasks/curriculum-standards-map-dash-029.json` |
| curriculum-standards-map-community-030 | `tasks/curriculum-standards-map-community-030.json` |
| curriculum-standards-map-funded-031 | `tasks/curriculum-standards-map-funded-031.json` |
