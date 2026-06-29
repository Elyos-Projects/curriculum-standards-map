# PLAN — curriculum-standards-map

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated
> Risk tier: **low** (accuracy-sensitive) · Deliverable family: open crosswalk **dataset** + tooling

## Executive summary

Teachers and curriculum coordinators waste enormous time answering one deceptively simple
question: *"Which openly-licensed resource actually teaches the standard I have to cover this
week?"* Open Educational Resources (OER) are abundant and free, but they are scattered across
dozens of repositories and are **not indexed against the curriculum standards that govern what a
teacher must teach** (Common Core, the Next Generation Science Standards, England's National
Curriculum, the Australian Curriculum, and others). The result is a discovery gap: high-quality
free material exists, but the teacher who needs it cannot find the piece aligned to *standard
CCSS.MATH.CONTENT.5.NF.B.4* on a Sunday night, so they buy a worksheet pack or skip the resource
entirely. The OER never reaches the student it could have helped.

This project builds an **open crosswalk**: a machine-readable, openly-licensed dataset of
**verified alignment records** that connect specific OER resources to specific curriculum
standards, addressed by stable, interoperable identifiers (1EdTech **CASE** GUIDs and
**Achievement Standards Network (ASN)** URIs) rather than by reproducing copyrighted standards
text. Each alignment record carries an alignment *type* (teaches / practices / assesses /
builds-toward), an alignment *strength*, the *evidence* that justifies it, full *provenance* for
both the OER and the standard, and a record of the **educator who reviewed it**. The crosswalk is
published as CSV + JSON + a schema.org `AlignmentObject` / CASE-compatible export so it can be
ingested by any learning platform, OER repository, or district catalog.

The **deliverable is the mapping, not the content.** We do not host, copy, modify, or republish
OER materials, and we do not reproduce the full text of copyrighted standards frameworks. We link
to the OER at its canonical source and reference each standard by its public identifier. This keeps
the legal surface small and the dataset maximally reusable.

Two constraints define the project's identity and are treated as hard gates, not preferences:
1. **License/provenance gate** — every OER we map must be verified openly licensed (CC0 / CC-BY /
   CC-BY-SA / public domain, with non-commercial CC-BY-NC handled only under an explicit written
   policy), and every standards framework we map *into* must be verified to permit referencing by
   identifier (and, separately, whether its text may be reproduced). Anything unclear is **flagged
   and excluded, never guessed.**
2. **Alignment-accuracy gate** — an AI session may *propose* alignments with evidence and a
   confidence score, but **no alignment ships without a qualified educator's review.** A wrong
   alignment is worse than no alignment: it sends a teacher to material that does not actually
   teach the standard, wasting the scarce time the project exists to save. Mis-alignment is the
   project's central risk and the reason an unreviewed AI-only crosswalk is explicitly out of scope.

Risk tier is **low** in the Elyos sense (no health/legal/safety advice, no PII, open content), but
the project is **accuracy-sensitive**: its value is destroyed by silent inaccuracy, so educator
review and an honest confidence model are first-class, not optional.

## Problem & beneficiaries

**Who is helped.**
- **Classroom teachers** (primary beneficiary) — especially those in under-resourced schools who
  cannot buy commercial standards-aligned material and must assemble lessons from free resources
  under severe time pressure. The crosswalk turns "search the whole web" into "look up the
  standard, get the vetted free resources."
- **Curriculum coordinators / instructional coaches** — who build scope-and-sequence documents and
  must show coverage of every required standard; a crosswalk lets them find OER gaps and fills.
- **OER repositories and platforms** (OER Commons, OpenStax, LibreTexts, learning-management
  systems, district catalogs) — who can ingest the open alignment records to make their own
  collections discoverable by standard, increasing reach of material they already host.
- **Home educators, tutors, and self-directed adult learners** — who use standards as a coverage
  checklist and need free material per topic.
- **Students** (ultimate beneficiary) — who receive instruction from better-matched free material.

**The verified need.** The *general* need is well established: the discovery/alignment gap for OER
is widely documented in OER-adoption research and is exactly the problem that paid
"standards-alignment" services monetize. We treat the general need as real. However, the
**per-partner, per-framework need is TO BE SECURED**: we have **not** yet confirmed a named
school, district, OER repository, or curriculum body that has agreed to *consume and act on* this
crosswalk. Until a named beneficiary commits to using the alignment records (ingesting them,
linking them from their catalog, or distributing them to teachers), individual tasks carry
`verifiedNeed: false`. This honesty matters because Elyos's bar is **delivered, not merged**: a
crosswalk that no teacher or platform actually uses has not delivered an outcome, however correct
it is.

**Partner org.** TO BE SECURED. Candidate channels: OER Commons / ISKME (which already hosts
standards-alignment metadata), OpenStax, CK-12 Foundation, LibreTexts, Khan Academy, a state/LEA
curriculum office, and the 1EdTech CASE Network community. M0 includes explicit partner-outreach
work; no partner is assumed, and no framework is mapped until both its licence terms and at least a
plausible consumer are identified.

## Goals and non-goals

**Goals**
- Produce a reusable **alignment data model + authoring template** (the canonical mapping record)
  and an interoperable schema (CASE-compatible / schema.org `AlignmentObject` / ASN URIs).
- For each in-scope (OER source × standards framework) pair, deliver **educator-reviewed,
  source-verified alignment records** that measurably reduce a teacher's time-to-find a
  standard-aligned free resource.
- Make **licence/provenance verification** (both sides: OER licence *and* framework terms) a
  non-skippable, auditable gate.
- Make **educator review** a non-skippable gate, with an honest, published **alignment-confidence
  model** so consumers can filter by reviewed strength.
- Reference standards by **stable identifier** (CASE GUID / ASN URI) so the crosswalk survives
  standards re-publication and is interoperable, and so we avoid reproducing copyrighted text.
- Ship the crosswalk in formats real consumers ingest (CSV, JSON, CASE export, schema.org) with a
  validator and a public "what's covered / what's missing" coverage ledger.

**Non-goals**
- We do **not** host, mirror, copy, modify, or republish OER content. We link to it at source.
- We do **not** reproduce the full text of copyrighted standards frameworks. We reference
  identifiers and, at most, the short official statement code/label where the framework licence
  explicitly permits it (e.g. OGL / CC-BY / CC0 frameworks).
- We do **not** ship AI-proposed alignments that no educator has reviewed.
- We do **not** rank, score, or rate OER *quality*, nor make purchasing or "best resource"
  recommendations. We assert *alignment*, not *merit*.
- We do **not** author, grade, or assess curriculum; we are an index, not a courseware product.
- We do **not** collect, process, or store any student, teacher, or other personal data.
- We do **not** claim or imply official endorsement by any standards body (no "certified aligned").
- We do **not** editorialise on contested civic/history standards; alignment is factual coverage,
  not commentary (see non-partisan stance in Data & compliance).

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Vanity metrics ("alignments produced") are explicitly
excluded; an alignment that is never reviewed or never consumed does not count.

| Metric | Baseline | Target (first 6 months) |
| --- | --- | --- |
| Alignment records **educator-reviewed and accepted** (verified, shippable) | 0 | ≥ 600 reviewed records across ≥ 2 subjects |
| Alignment records **ingested/used by a named consumer** (repository, district, platform) — last-mile delivered | 0 | ≥ 1 consumer ingesting ≥ 300 records |
| Confirmed consuming partners (named beneficiary committed to use the crosswalk) | 0 | ≥ 1 secured |
| Time-to-find a standard-aligned free resource (teacher task, measured in a small usability test) | establish baseline at M0 (e.g. ~10–20 min unaided) | ≥ 50% reduction with the crosswalk |
| Alignment precision (sampled audit: reviewer-confirmed correct ÷ shipped) | n/a | ≥ 95% of shipped records correct on independent re-audit |
| Licence/provenance errors in shipped records (OER licence or framework-terms errors) | n/a | **0** licence errors (hard target) |
| Standard coverage within a chosen scope (e.g. one grade-band × subject) | 0% | ≥ 60% of standards in scope have ≥ 1 reviewed aligned OER |
| Reuse signal: crosswalk forked/cited/linked by an independent third party | 0 | ≥ 2 verifiable external reuse events |

Notes on honest measurement:
- **"Used by a consumer"** must be externally verifiable: an ingestion commit/permalink, a catalog
  page rendering our alignments, or a written confirmation from the consuming org. Self-reported
  use does not count.
- **Precision audit** is run by a *second* educator on a random sample, not the original reviewer,
  to avoid rubber-stamping.
- **Coverage** is reported honestly including standards with *zero* aligned OER (a real, useful
  finding — it shows where the commons has gaps), via the public coverage ledger.
- **Time-to-find** is measured in a small, ethics-light usability test with volunteer educators
  (no PII retained beyond aggregate timings; consent recorded). It is a directional metric, not a
  controlled trial.

**Defining "measurably reduces time-to-find" so DoDs are verifiable.** Each shipped alignment
contributes to a per-standard *coverage record*; the headline outcome is the usability-test delta
above plus the coverage percentage. "Accepted" = passed both gates (licence + educator review) with
the gate artifacts committed. "Delivered" = a named consumer has ingested or published the records
(evidence artifact recorded by the Steward).

## Scope

**In scope**
- The **alignment crosswalk dataset**: educator-reviewed records linking OER resources to standards
  by identifier, with type, strength, evidence, provenance, and reviewer.
- The **alignment data model + authoring template** and its interoperable exports (CSV, JSON,
  schema.org `AlignmentObject`, CASE-compatible JSON, ASN URI references).
- **Licence/provenance triage** for OER sources and **framework-terms triage** for standards
  frameworks, each recorded as a committed gate artifact.
- **AI-assisted candidate generation**: an OER ingest + candidate-alignment proposer that emits
  *proposals with evidence and confidence* for educator review (never auto-published).
- A **validator** (schema + identifier-resolution checks) and a public **coverage ledger**.
- A **frameworks registry** and an **OER source registry** (licence-classified candidate backlogs).

**Out of scope**
- Hosting, mirroring, modifying, or republishing OER content or full standards text.
- OER *quality* rating, ranking, or recommendation; pedagogy authoring; assessment generation.
- Any student/teacher personal data, learning analytics, or usage tracking of individuals.
- Automated, unattended publishing of alignments without educator review.
- Mapping into a framework whose licence terms cannot be verified to permit referencing.
- Mapping OER whose licence cannot be verified as open (CC0/CC-BY/CC-BY-SA/PD; NC only per policy).
- Any framing implying official accreditation/endorsement by a standards body.
- High-stakes domains (no medical/legal/safety *advice*); subject content is general education only.

**Initial scope selection (first frameworks & OER sources).** To avoid boiling the ocean, M0/M1
deliberately start with framework × source pairs that are (a) clearly licence-clean on both sides
and (b) have stable machine-readable identifiers:
- **Frameworks (referencing terms verified before any mapping):** England **National Curriculum**
  (Crown copyright under the **Open Government Licence**, text reproducible with attribution);
  **Australian Curriculum** (ACARA, **CC-BY-4.0**, text reproducible with attribution); **Common
  Core State Standards** (public licence permits use/reference with attribution — referenced by
  identifier; reproduction of statement text only as the licence allows). **NGSS** is *deferred*
  until its reproduction/reference terms are confirmed in writing (its copyright is more
  restrictive — see Open questions), but referencing by ASN/CASE identifier is evaluated under the
  framework-terms gate.
- **OER sources (CC-BY unless noted):** OpenStax, Illustrative Mathematics, EngageNY/Eureka
  (CC-BY), PhET Interactive Simulations (CC-BY), Siyavula (CC-BY), LibreTexts (per-page CC, must be
  checked per resource). **CK-12** is CC-BY-**NC** and is handled only under the NC policy.

## Solution approach & architecture

This is a **data/knowledge-mapping project with light software** (a canonical data model + an
AI-assisted candidate generator + validators/exporters). It is not a content platform and moves no
OER content.

**Pipeline (per (OER source × framework) batch)**
1. **Dual licence/terms gate** — verify the OER source's licence permits open reuse (and record
   it) *and* verify the framework's terms permit referencing by identifier (and, separately,
   whether statement text may be reproduced). PASS recorded as a committed artifact; any doubt →
   FLAG/EXCLUDE.
2. **Identifier resolution** — resolve the framework's standards to canonical identifiers (CASE
   GUIDs from the CASE Network, and/or ASN URIs). Build/confirm the standards skeleton from the
   framework's *official* machine-readable source where one exists (CASE registry, ACARA/DfE
   open data) — never by scraping copyrighted PDFs.
3. **OER ingest** — fetch OER *metadata and openly-licensed content text* from the canonical
   source under a bounded-access protocol (see below); extract topic, grade, subject, learning
   objectives. We index; we do not store the full content as a republished copy.
4. **Candidate alignment proposal (AI-assisted)** — for each OER unit, the proposer emits candidate
   `(oer, standard)` alignments, each with: alignment *type*, a *confidence* score, and a short
   *evidence* rationale (which part of the OER addresses which part of the standard). Output is a
   review queue, not a published record.
5. **Educator review (mandatory gate)** — a qualified reviewer confirms/edits/rejects each
   candidate, sets the final alignment *type* and *strength*, and signs off. Only reviewed records
   advance. A second-reviewer audit samples shipped records for precision.
6. **Validation & export** — validate against the alignment schema and resolve every identifier;
   emit CSV, JSON, schema.org `AlignmentObject`, and CASE-compatible exports; update the coverage
   ledger.
7. **Deliver to consumer** — a human hands/ingests the records to the named consumer
   (repository/district/platform); the Steward records the acceptance/ingestion evidence.

**Canonical alignment record (single source of truth; all exports are projections of it).**

```
AlignmentRecord {
  id                         // stable crosswalk record id
  oer {
    id, title, url,          // canonical source URL — we link, never rehost
    publisher,
    license { id, url, permitsDerivatives:boolean, commercialUse:boolean, snapshotRef },
    granularity,             // e.g. course | unit | lesson | activity | item
    subject, gradeLevels[],
    provenance { retrievedAt, version }
  }
  standard {
    frameworkId,             // e.g. ccss-math | au-v9 | eng-nc
    statementId,             // human code, e.g. CCSS.MATH.CONTENT.5.NF.B.4
    caseGuid,                // 1EdTech CASE GUID (if available)
    asnUri,                  // ASN URI (if available)
    frameworkTerms { license, permitsReference:boolean, permitsTextReproduction:boolean, ref }
    // statementText included ONLY if framework terms permit reproduction; else omitted, link only
  }
  alignment {
    type,                    // teaches | practices | assesses | builds-toward | prerequisite
    strength,                // exact | strong | partial | tangential  (see confidence model)
    coverage,               // full | partial  (does the OER cover the whole statement?)
    evidence,                // short rationale: what in the OER addresses what in the standard
    aiConfidence,            // 0..1 from the proposer (advisory only; never the ship signal)
  }
  review {
    status,                  // proposed | accepted | rejected | needs-fix
    reviewedBy,              // educator identifier/role (no PII beyond what consent allows)
    reviewerQualification,   // e.g. "licensed K-12 math teacher"
    reviewedAt, auditedBy?   // second-reviewer precision audit
  }
  outputLicense              // crosswalk record license (CC-BY-4.0)
}
```

**Alignment-confidence model (published, so consumers can trust-filter).**
- `exact` — the OER unit's objective maps one-to-one to the full standard statement.
- `strong` — the OER substantively teaches/assesses the standard; minor scope difference.
- `partial` — the OER covers *part* of the standard (record `coverage: partial`).
- `tangential` — related but not a coverage claim; included only with explicit reviewer note.
`aiConfidence` is advisory metadata for triage *only*; the **ship signal is the educator
`review.status: accepted`**, never the model score. Consumers can filter on `strength`.

**Bounded OER-access protocol (so "index, don't rehost" is enforceable).**
- Fetch from the **canonical source URL** only; prefer official metadata/API/sitemaps.
- Extract enough text to judge and evidence alignment; do **not** commit a full reproduction of the
  OER body into the repo. Evidence snippets are short, attributed quotations within fair-use/licence
  bounds, or references by URL + locator.
- The crosswalk stores **links + metadata + short evidence**, not the OER corpus.
- Respect each source's robots/Terms and rate limits; stop and flag on access restrictions.

**Tech stack.** TypeScript, ESM, pnpm workspaces (Elyos convention). The data model, validator,
exporters, and candidate proposer are small Node packages with minimal dependencies. Records are
authored/stored as JSON + CSV; exports include schema.org JSON-LD and CASE JSON. No runtime
service; everything runs locally or in CI. The AI candidate-proposer runs in the **donated lane**
(a human runs their own agent over the prepared workspace); no headless funded execution is assumed
(a funded variant could batch-propose under a budget cap — out of scope for v0.1.0).

**Pinned interop spec versions** (recorded in the model's `specVersions`, bumped only by a
deliberate task):
- **1EdTech CASE** v1.1 (Competencies & Academic Standards Exchange) for framework/standard
  identifiers and exchange.
- **schema.org** `AlignmentObject` / `LearningResource` (current) for `educationalAlignment`.
- **ASN** URIs for standards identity where CASE GUIDs are unavailable.
- **LRMI** (Learning Resource Metadata Initiative) vocabulary alignment for educational metadata.

**Key decisions (locked for v0.1.0).**
- **Reference standards by identifier (CASE GUID / ASN URI), not by reproducing text.** Text is
  included only where the framework licence explicitly permits and only the official statement.
- **Canonical-record-first**: one `AlignmentRecord`; CSV/JSON/CASE/schema.org are projections.
- **Two blocking gates** (dual licence/terms, and educator review) encoded as committed artifacts.
- **Crosswalk dataset licensed CC-BY-4.0**; **code MIT**. (CC0 considered for max interop but
  CC-BY chosen to preserve the attribution/provenance chain to reviewers and sources.)
- **No quality/merit claims** — alignment only.
- **Standards skeletons sourced from official machine-readable releases**, never PDF-scraped.

## Data, licensing & compliance

**This is the critical section.** The crosswalk sits at the junction of two copyright regimes (OER
content *and* standards-framework text), so both are treated conservatively.

**OER source licences we accept (must permit reuse; derivative status recorded):**
- **CC0 1.0 / Public Domain** — accepted; provenance still recorded.
- **CC-BY 4.0** (and compatible versions) — accepted; attribution required and recorded.
- **CC-BY-SA 4.0** — accepted; share-alike noted. The crosswalk *links* to (does not incorporate)
  the OER, so the crosswalk record itself is not a derivative of the OER body; share-alike applies
  to reused OER text, which we do not redistribute. Recorded per source.
- **CC-BY-NC** (e.g. CK-12) — accepted **only** under the written NC policy (`policy`), because the
  crosswalk is a non-commercial public-commons artifact but downstream commercial consumers exist;
  default is *flag/escalate*, and any NC mapping records `commercialUse:false` and a downstream-use
  caveat. Not accepted by default.
- Anything **all-rights-reserved, no licence, or non-derivative where we would need a derivative**
  → **flagged and excluded, never guessed.**

**Objective OER-licence acceptance criterion.** An OER PASSes only if its licence is on the
accepted list (or accepted under the NC policy) **and** `license.permitsDerivatives` and
`license.commercialUse` are recorded from a cited licence URL/clause, with a `snapshotRef`
(committed copy + SHA-256 + Wayback save URL). Missing/unparseable evidence = FLAG/EXCLUDE.

**Standards-framework terms (a separate, equally hard check).** Standards text is frequently
**copyrighted even when publicly available**, and the rules differ per framework. We verify, per
framework, two distinct permissions and record both:
- `permitsReference` — may we *reference* the standard by identifier/code and assert an alignment to
  it? (Near-universally yes; this is the crosswalk's core operation.)
- `permitsTextReproduction` — may we *reproduce the statement text* in the dataset?
Known starting positions (to be re-verified in writing at the framework-terms gate, not assumed):
  - **England National Curriculum** — Crown copyright under **OGL**; reference *and* text
    reproduction permitted with attribution. `permitsTextReproduction: true`.
  - **Australian Curriculum (ACARA)** — **CC-BY-4.0**; reference and reproduction permitted with
    attribution. `permitsTextReproduction: true`.
  - **Common Core State Standards (CCSS)** — public licence permits use with attribution; we
    reference by identifier and reproduce statement text **only** to the extent the CCSS public
    licence allows, with the required NGA/CCSSO attribution. Default: reference by ID; reproduce
    only the official statement code/text where clearly permitted. `permitsTextReproduction`
    recorded per the licence reading.
  - **NGSS** — **deferred**: copyright held by Achieve; reproduction terms are more restrictive.
    We do **not** reproduce NGSS text and treat NGSS mapping as gated on written confirmation that
    referencing by identifier is acceptable (Open questions). Likely `permitsReference: true`,
    `permitsTextReproduction: false` — but unconfirmed, so excluded until verified.
Any framework whose `permitsReference` cannot be confirmed → **excluded**. Where
`permitsTextReproduction` is false, the record omits `statementText` and carries the identifier +
official source link only.

**Provenance model.** Every alignment record stores, for the OER: source URL, publisher, retrieval
timestamp, version, licence id + URL + snapshot (committed copy + SHA-256 + Wayback). For the
standard: framework id, statement id/code, CASE GUID and/or ASN URI, framework terms ref + snapshot.
For the alignment: who proposed it (AI/tool version), the evidence, and **who reviewed it**.
Provenance is part of the committed deliverable; an unprovenanced record is invalid.

**Privacy / PII stance.** The project handles **no personal data** by design: no student data, no
learner analytics, no teacher accounts. The only personal-adjacent data is the **reviewer
identity**, recorded with consent and minimised to a role/qualification (e.g. "licensed K-12 math
teacher, US") plus an opaque reviewer id unless the reviewer consents to attribution. Usability-test
timings are aggregated and consented; no individual learner is recorded. If any OER unexpectedly
contains personal data (e.g. student work samples), it is flagged and excluded.

**Non-partisan stance (civic/history/social-studies standards).** Where the framework includes
contested civic, historical, or social topics, the crosswalk asserts only *factual coverage*
("this OER addresses standard X"), never an editorial judgment about the standard or the resource's
viewpoint. We do not select or exclude OER on ideological grounds, and we do not annotate political
"slant." Contested-topic alignments get an extra reviewer note and avoid any framing that endorses
or attacks a position.

**"Not advice / not endorsement" framing.** The crosswalk is a discovery aid for educators applying
professional judgment. It is **not** an official accreditation, and alignment does **not** imply the
standards body endorses the OER. This disclaimer ships with the dataset and every export.

**Attribution & output licence.** Every record attributes the OER publisher per its licence and the
standards framework per its terms (e.g. NGA/CCSSO for CCSS, ACARA for the Australian Curriculum,
Crown/OGL for England). The **crosswalk dataset is licensed CC-BY-4.0**; **tooling code is MIT**.
The dataset's licence covers *our* contribution (the alignment records) and does not relicense the
upstream OER or standards.

## Quality, review & risk gates

**Risk tier: low** (no health/legal/safety advice, no PII, open content) — but **accuracy-critical**.
The dominant risk is not legal harm but *misinformation-by-mis-alignment*: a confidently wrong
record sends a teacher to material that does not teach the standard. Review is therefore designed to
catch alignment errors, not just licence errors.

**Required review before a record is "done":**
- **Licence/terms reviewer** (mandatory, every batch): confirms the OER licence permits reuse
  (recorded with snapshot) **and** the framework terms permit referencing (and whether text may be
  reproduced). Hard gate.
- **Educator reviewer** (mandatory, every record): a qualified teacher/curriculum specialist for
  the subject + grade band confirms each alignment's *type*, *strength*, and *coverage*, with
  evidence. **No record ships without this.** Reviewer qualification is recorded.
- **Second-reviewer precision audit**: an independent educator re-checks a random sample of shipped
  records each milestone to produce the precision metric (guards against rubber-stamping).
- **Technical reviewer**: confirms schema validity, identifier resolution (every CASE GUID / ASN
  URI resolves), exports validate, and CI is green.

**Test fixtures & golden files (so "CI green" means something).**
- **Schema validator** — golden valid/invalid `AlignmentRecord` fixtures that must pass/fail.
- **Exporters** — golden canonical-record → expected CSV / schema.org JSON-LD / CASE JSON pairs,
  diffed in CI and validated against the pinned spec.
- **Identifier resolver** — fixtures asserting that sample CASE GUIDs / ASN URIs resolve and that
  bogus identifiers are rejected.
- Fixtures use synthetic or clearly-permissive public examples only; no copyrighted standards text
  is committed where reproduction is not permitted.

**Definition of Shipped.** An alignment record is *shipped* only when: (1) it passed the dual
licence/terms gate (artifacts committed); (2) a qualified educator accepted it (review recorded);
(3) it validates against the schema and every identifier resolves; (4) it is **delivered to a named
consumer** — ingested by a repository/district/platform or published in the open crosswalk release
that a confirmed consumer has agreed to use — with the Steward's delivery-evidence artifact
recorded. Producing reviewed records is *necessary but not sufficient*; **delivered, not merged.**

## Roadmap & milestones

**M0 — Foundation & cold-start (thin)**
- Goal: build the data model + gates + a minimal toolchain, prove the end-to-end flow on **one
  small (framework × OER source) slice**, and open partner outreach.
- **Cold-start de-risking.** Choose a slice that is licence-clean on both sides and has machine-
  readable identifiers (recommended pilot: **Australian Curriculum (CC-BY) × OpenStax (CC-BY)** or
  **England National Curriculum (OGL) × a CC-BY OER** for one subject/grade band), so a real
  *delivered* outcome is reachable via a self-serve open release even before a consumer is secured.
- Exit criteria: (1) `AlignmentRecord` model + authoring template published; (2) dual licence/terms
  gate checklist (incl. NC policy + framework-terms checks) exists and is applied to the pilot
  slice; (3) schema validator + one exporter (schema.org `AlignmentObject`) working in CI with
  golden fixtures; (4) ≥ 50 alignment records for the pilot slice **educator-reviewed and accepted**
  with gate artifacts; (5) the pilot published as an open CC-BY release with a coverage ledger — and
  **delivered** to a consumer if one materialises, else released self-serve with the blocker
  surfaced; (6) ≥ 1 partner-outreach thread opened; (7) baseline "time-to-find" measured.

**M1 — Gates hardened + interop exports + first delivery**
- Goal: make both gates rigorous, add CASE-compatible export + identifier resolution, get real
  consumption.
- Exit criteria: (1) licence/terms gate codified as a reviewable artifact and applied across ≥ 2
  frameworks; (2) CASE export + CSV export validated against pinned specs with golden fixtures;
  (3) every shipped record's CASE GUID / ASN URI resolves (identifier resolver green); (4) ≥ 1
  confirmed consuming partner; (5) ≥ 300 reviewed records across ≥ 2 subjects delivered/ingested by
  the partner (or in a release the partner has agreed to use); (6) precision audit ≥ 95% on a
  sample.

**M2 — AI-assisted candidate generation & scale**
- Goal: increase throughput with the candidate proposer while keeping the educator gate intact.
- Exit criteria: (1) candidate proposer emits `(oer, standard)` proposals with evidence +
  confidence into a review queue (never auto-publish); (2) measured **reviewer throughput** (records
  reviewed per hour) improves vs. the manual M0/M1 baseline **without** precision dropping below
  95%; (3) ≥ 600 reviewed records cumulatively across ≥ 2 frameworks; (4) coverage ledger reports
  ≥ 60% of standards in at least one grade-band × subject have ≥ 1 reviewed aligned OER.

**M3 — Coverage, reuse outcomes & sustainability**
- Goal: demonstrate real downstream reuse, coverage breadth, and a maintenance model.
- Exit criteria: (1) ≥ 2 verifiable external reuse events; (2) ≥ 1,000 reviewed records delivered
  cumulatively, spanning ≥ 2 frameworks and ≥ 2 subjects; (3) staleness/refresh process documented
  (handles OER moving/changing and standards re-publication); (4) a steward identified for ongoing
  consumer liaison and outcome tracking; (5) usability-test shows ≥ 50% time-to-find reduction.

Dependencies: M1 depends on M0's model + gates; M2's proposer depends on M1's schema + exports;
M3 depends on a body of delivered records from M1–M2.

## Work breakdown

The itemized, schema-mapped backlog lives in `TASKS.md`, organized by the milestones above. Each
milestone has a task table (`ID | Title | Type | Size | Risk | Deliverable | Depends on |
Reviewer`), acceptance criteria for the most important tasks, and a Definition of Done. A
sized-but-unscheduled backlog and one complete, schema-valid example Task JSON are included there.
The candidate **frameworks registry** and **OER source registry** (licence-classified backlogs that
feed per-slice mapping tasks) are referenced from `TASKS.md`; no framework or source becomes a
mapping task until it passes the dual licence/terms gate.

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the data model, gates, registries, and backlog.
- **Licence/terms reviewer:** TBD (TO BE SECURED) — mandatory, non-skippable gatekeeper for both
  OER licences and framework terms; must be filled **before the M0 pilot is reviewed**. Can read
  CC/OGL/CCSS/ACARA terms and apply the NC policy. May rotate among ≥ 2 qualified reviewers, but at
  least one must always exist or mapping halts. Until named, all tasks remain `verifiedNeed:false`.
- **Educator reviewer(s):** TBD (TO BE SECURED) — qualified teachers/curriculum specialists per
  subject + grade band who confirm each alignment. The **central quality gate**; no record ships
  without one. A roster covering the in-scope subjects/grades is required before scaling.
- **Second-reviewer / auditor:** an independent educator who runs the precision audit each milestone.
- **Technical reviewer(s):** rotation verifying schema validity, identifier resolution, exports, CI.
- **Steward (last-mile owner):** TBD — owns the relationship with consuming repositories/districts/
  platforms and confirms ingestion/use (the "delivered" signal). Critical: Definition of Shipped is
  *delivery*, not production.
- **Partner / requestor:** TO BE SECURED — named consumer(s) (OER repository, district/LEA,
  platform) and, where relevant, the standards body's community (e.g. CASE Network).

## Dependencies & integrations

- **External standards/specs (pinned):** 1EdTech **CASE** v1.1, **ASN** URIs, schema.org
  `AlignmentObject`/`LearningResource`, **LRMI**. Versions recorded in `specVersions`, bumped only
  via a deliberate task.
- **Standards frameworks (terms-gated before use):** England National Curriculum (OGL), Australian
  Curriculum (ACARA, CC-BY-4.0), Common Core State Standards (public licence), NGSS (deferred).
  Machine-readable identifiers via the **CASE Network** registry and **ASN**.
- **OER sources (licence-gated before use):** OpenStax, Illustrative Mathematics, EngageNY/Eureka,
  PhET, Siyavula, LibreTexts (per-page), CK-12 (NC, policy-gated). None assumed in scope until gated.
- **Consuming platforms (output targets):** OER Commons / ISKME, OpenStax, district catalogs, LMS
  importers; ingestion is via open files (CSV/JSON/CASE/schema.org), no live coupling.
- **Elyos pieces:** Task JSON schema (`packages/schema`), donated-lane CLI workspace/PR flow
  (`packages/cli`), good-deed definition + refusal guardrails. No funded-lane/runner dependency for
  v0.1.0 (a budget-capped funded proposer variant is a possible future, out of scope here).

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| Inaccurate alignment (OER claimed to teach a standard it doesn't) misleads teachers | High | High | Mandatory educator review per record; independent second-reviewer precision audit; honest confidence model; AI score never the ship signal | Educator reviewer |
| Reproducing copyrighted standards text beyond licence terms | Medium | High | Framework-terms gate records `permitsTextReproduction`; default reference-by-identifier; reproduce only where explicitly permitted (OGL/ACARA/CCSS-as-allowed); NGSS deferred | Licence/terms reviewer |
| Mapping an OER whose licence isn't actually open (or NC misused) | Medium | High | OER licence gate with cited URL + snapshot; NC handled only under written policy; exclude on doubt | Licence/terms reviewer |
| No consuming partner → crosswalk produced but never used (fails "delivered") | Medium | High | M0 outreach; self-serve open release as fallback; steward role; `verifiedNeed:false` until secured | Steward |
| Standards re-published / re-coded; identifiers drift | Medium | Medium | Reference by stable CASE GUID/ASN URI; record framework version; staleness/refresh task; resolver in CI | Maintainer |
| OER content moves or changes, breaking links/evidence | Medium | Medium | Record version + retrieved date + snapshot; link-check + drift detection in refresh process | Maintainer |
| AI proposer biases reviewers toward over-accepting (automation bias) | Medium | Medium | Proposer output framed as *candidate*; reviewers must record evidence; blind precision audit measures real correctness | Educator reviewer |
| Scope creep into rating OER quality / recommending resources | Medium | Medium | Explicit non-goal; reviewers reject merit claims; only alignment asserted | Maintainer |
| Contested civic/history standards draw partisanship | Low | Medium | Non-partisan stance; factual-coverage-only; extra reviewer note; no ideological selection/annotation | Educator reviewer |
| Implying official endorsement ("certified aligned") | Low | Medium | "Not endorsement/not accreditation" disclaimer shipped with dataset + every export | Maintainer |
| CASE/schema.org spec drift breaks exports | Low | Low | Canonical-record-first; exporters pinned to spec versions; golden fixtures in CI | Maintainer |

## Security & privacy

- **Small threat surface:** no runtime service, no data hosting, no accounts. Main surfaces are CI
  and the published crosswalk files.
- **No PII by design:** no student/teacher/learner data. Reviewer identity is minimised to
  role/qualification + opaque id (attribution only with consent). Usability-test data is aggregated
  and consented; raw timings are not tied to identities in the release.
- **Secrets handling:** tooling needs no credentials by default. Any API token (e.g. to a CASE
  registry or a consumer's ingest endpoint) is supplied by the human operator and never written to
  logs, receipts, or committed files (Elyos rule).
- **Bounded OER access:** fetch from canonical sources, respect robots/Terms and rate limits, store
  links + metadata + short evidence only — never a rehosted OER corpus.
- **Abuse/misuse prevention:** refuse and flag any task that would (a) launder a non-open resource
  as "open," (b) reproduce copyrighted standards text beyond licence, (c) inject ideological slant
  into civic/history alignments, or (d) imply official endorsement. Records stay descriptive,
  source-verified, and reviewer-signed.

## Sustainability & maintenance

- **Post-delivery ownership:** the steward maintains consumer relationships and outcome tracking;
  the maintainer keeps the model, gates, registries, validator, and exporters current with CASE/
  schema.org and framework re-publications.
- **Refresh:** a scheduled link-check + drift-detection pass flags OER that moved/changed and
  standards frameworks that were re-coded (CASE version bump); affected records become `maintenance`
  tasks and may need re-review. The coverage ledger is regenerated each release.
- **Outcome tracking:** the steward records ingestion/use events and external reuse signals against
  the success metrics; precision audits and coverage are reviewed each milestone. The dataset is
  versioned (semver + dated releases) so consumers can pin and upgrade deliberately.
- **Community model:** the frameworks/OER registries and the gate artifacts are public, so external
  educators can contribute reviewed alignments under the same gates (a path to scale beyond the core
  team), with the second-reviewer audit preserving precision.

## Open questions

- Which consumer is the first confirmed partner (OER Commons/ISKME, OpenStax, a district/LEA, an
  LMS)? Their preferred ingest format pins the priority exporter.
- **NGSS terms:** does Achieve's licence permit referencing NGSS by identifier in an open dataset
  (almost certainly yes) and is text reproduction definitely out (likely)? NGSS stays deferred until
  this is confirmed in writing.
- **CCSS text reproduction:** exactly how much statement text may we reproduce under the CCSS public
  licence vs. reference-by-identifier only? Default conservative (ID + link) until confirmed.
- Do we adopt **CASE GUIDs** as primary identity and ASN as fallback, or carry both always?
  (Leaning: carry both where available; CASE primary for exchange.)
- Should the crosswalk dataset be **CC0** (max interop) instead of **CC-BY-4.0**? (Leaning CC-BY to
  preserve provenance/attribution chain; revisit if a consumer needs CC0.)
- How is **CK-12 (CC-BY-NC)** treated for consumers who may use the crosswalk commercially? The NC
  policy must resolve this before any CK-12 mapping.
- What is the minimum **reviewer roster** (subjects × grade bands) needed before M2 scaling so the
  educator gate is never the bottleneck *and* never skipped?
- What counts as a sufficiently "verifiable external reuse event" for the outcome metric?

## References

- Elyos work rules — `C:\code\elyos\CLAUDE.md`
- Good Deed Definition + risk tiers — `C:\code\elyos\docs\good-deed-definition.md`
- Task JSON schema — `C:\code\elyos\packages\schema\src\schemas.ts`
- Portfolio roadmap — `C:\code\elyos\planning\ROADMAP.md`
- Sibling plan (house style) — `C:\code\elyos\planning\projects\open-data-datasheets\PLAN.md`
- 1EdTech **CASE** (Competencies & Academic Standards Exchange) specification
- **Achievement Standards Network (ASN)** — standards identifiers/URIs
- schema.org `AlignmentObject` / `LearningResource`; **LRMI** vocabulary
- Common Core State Standards (NGA/CCSSO public licence); Next Generation Science Standards (Achieve)
- Australian Curriculum (ACARA, CC-BY-4.0); England National Curriculum (Crown copyright, OGL)
- OER sources: OpenStax, Illustrative Mathematics, EngageNY/Eureka, PhET, Siyavula, LibreTexts, CK-12

---

## Appendix A — Improvements applied

The 25 improvements below were identified against the first draft and **applied** to the plan above
(and to `TASKS.md`). Each states the gap and the concrete change made.

1. **Two gates, not one.** The first draft mirrored sibling plans with a single licence gate. OER
   crosswalks have *two* copyright regimes, so a distinct **framework-terms gate** (`permitsReference`
   vs `permitsTextReproduction`) was added alongside the OER-licence gate, with both encoded as
   committed artifacts.
2. **Reference-by-identifier as the default.** To avoid reproducing copyrighted standards text, the
   plan locks **CASE GUID / ASN URI referencing** as the default and reproduces statement text only
   where a framework's terms explicitly permit (OGL/ACARA/CCSS-as-allowed). NGSS deferred.
3. **Educator review as a hard ship gate.** Elevated alignment review from "nice to have" to a
   **non-skippable gate**; an unreviewed AI-only crosswalk is now an explicit non-goal and the AI
   confidence score is explicitly *never* the ship signal.
4. **Honest confidence model published.** Added a 4-level `strength` model (exact/strong/partial/
   tangential) plus `coverage` (full/partial) so consumers can trust-filter, instead of a single
   opaque "aligned" boolean.
5. **Precision audited by an independent second reviewer.** Added a blind second-reviewer sample
   audit each milestone to produce the precision metric and guard against rubber-stamping/automation
   bias — a risk unique to AI-proposed alignments.
6. **Automation-bias risk named and mitigated.** Added the specific risk that the AI proposer nudges
   reviewers to over-accept, with mitigations (candidate framing, mandatory evidence, blind audit).
7. **"Delivered, not merged" wired into the metric.** Separated "reviewed/accepted" from
   "delivered to a named consumer," with a Steward-recorded delivery-evidence artifact, so producing
   records is not mistaken for the outcome.
8. **verifiedNeed honesty.** Set `verifiedNeed:false` across tasks and marked partner/consumer **TO
   BE SECURED**, with M0 outreach and a self-serve open-release fallback so a real delivered outcome
   is still reachable.
9. **Coverage ledger reports gaps, not just hits.** Made "standards with zero aligned OER" a
   first-class, published output — a genuinely useful signal about where the commons is thin.
10. **Bounded OER-access protocol.** Added an enforceable "index, don't rehost" protocol (canonical
    source only, short evidence snippets, no committed OER corpus, robots/Terms respected) so the
    "deliverable is the mapping, not the content" principle is operational, not aspirational.
11. **No-quality-rating non-goal sharpened.** Explicitly forbade ranking/scoring OER merit or making
    "best resource" recommendations; the crosswalk asserts *alignment*, not *quality*.
12. **Non-partisan stance for civic/history standards.** Added an explicit factual-coverage-only
    rule, no ideological selection/annotation, and an extra reviewer note for contested topics
    (Elyos civic guardrail).
13. **"Not endorsement / not accreditation" disclaimer.** Added a shipped disclaimer so an alignment
    is never read as an official standards-body endorsement of the OER.
14. **PII stance made precise.** Clarified the only personal-adjacent data is reviewer identity
    (consented, minimised to role/qualification + opaque id) and that usability-test data is
    aggregated/consented; OER containing personal data is flagged and excluded.
15. **Provenance covers both sides + the reviewer.** Provenance now records OER source/version/
    licence-snapshot, framework version/terms-snapshot, *and* who proposed and who reviewed each
    record; an unprovenanced record is invalid.
16. **Pinned interop specs.** Pinned CASE v1.1, ASN, schema.org `AlignmentObject`, and LRMI in a
    `specVersions` block, bumped only by a deliberate task, so exporters don't silently drift.
17. **Canonical-record-first architecture.** One `AlignmentRecord`; CSV/JSON/CASE/schema.org are
    projections — avoids hand-maintaining four divergent formats.
18. **Initial scope chosen by licence-cleanliness.** Locked a first slice (ACARA/England NC × CC-BY
    OER) that is clean on *both* sides with machine-readable identifiers, de-risking cold start.
19. **Standards skeletons from official machine-readable sources.** Forbade PDF-scraping copyrighted
    standards; skeletons come from CASE registry / official open data only.
20. **Identifier resolution tested in CI.** Added a resolver with fixtures so every shipped CASE
    GUID / ASN URI must resolve and bogus identifiers fail — a concrete "CI green means something."
21. **Reviewer-roster gating before scale.** Made an adequate educator roster (subjects × grade
    bands) a prerequisite for M2 scaling so the gate is never the bottleneck *or* skipped; surfaced
    as an open question + a task.
22. **NC-licence policy made a blocking artifact.** CK-12/CC-BY-NC handling is deferred to a written
    NC policy and `commercialUse:false` is recorded, protecting commercial downstream consumers.
23. **Staleness/refresh for two moving targets.** Refresh process now handles *both* OER drift
    (moved/changed content) and standards re-publication (CASE version bumps), with affected records
    becoming `maintenance` tasks needing possible re-review.
24. **Measured time-to-find outcome.** Added a baseline-at-M0 usability test and a ≥50% reduction
    target so the headline benefit is measured, not asserted.
25. **Community contribution path under the same gates.** Added a sustainability path for external
    educators to contribute reviewed alignments through the identical dual-gate + blind-audit
    process, enabling scale beyond the core team without lowering precision.

## Review sign-off

**Reviewer:** Senior staff engineer + TPM (drafting agent), self-review pass.
**Date:** 2026-06-28 · **Doc version reviewed:** 0.1.0

**Completeness check (PLAN_SPEC 17 sections):** all present and in order — Executive summary;
Problem & beneficiaries; Goals and non-goals; Success metrics (outcomes); Scope; Solution approach
& architecture; Data, licensing & compliance; Quality, review & risk gates; Roadmap & milestones;
Work breakdown; Governance, roles & stakeholders; Dependencies & integrations; Risks & mitigations;
Security & privacy; Sustainability & maintenance; Open questions; References. ✔

**Correctness / guardrail check.**
- Good-deed criteria: tangible public benefit (teachers/students) ✔; freely available (CC-BY dataset,
  MIT code) ✔; not primarily for-profit (NC handling protects against commercial capture; consumers
  may include platforms but primary benefit is public educators) ✔; no harm/discrimination/
  misinformation (educator gate + precision audit directly target mis-alignment misinformation;
  non-partisan stance) ✔; specific enough for AI sessions with review ✔.
- Licensing: dual gate (OER licence + framework terms), reference-by-identifier default, NGSS
  deferred, NC policy-gated, snapshots required — conservative and specific ✔.
- Privacy: no PII by design; reviewer identity minimised/consented ✔.
- Risk tier **low** is correct in the Elyos sense (no medical/legal/safety advice); accuracy
  sensitivity handled via mandatory educator review rather than an inflated risk tier ✔.
- Lane **donated**; no funded budget assumed; CLI-prepares-workspace model respected ✔.

**Fixes applied during review.**
- Reconciled the headline "low risk" with the accuracy sensitivity by stating both explicitly
  (avoids the reader concluding review is optional).
- Ensured every metric is externally verifiable (added "self-reported does not count" and the blind
  audit) — removed an earlier vanity-metric ("records produced").
- Confirmed Definition of Shipped requires *delivery* + evidence artifact, consistent with the
  success metrics and TASKS DoDs.
- Verified TASKS.md IDs, dependencies, and the example Task JSON are schema-valid (all required
  fields present; `deliverable` uses `dataset` for the crosswalk itself, `document`/`pr` for
  templates/tooling; `verifiedNeed:false`; honest `requestor: TO BE SECURED`).

**Residual items for a human decision (not blockers to planning):** confirm the first consuming
partner; obtain written NGSS/CCSS reproduction terms; decide CC-BY vs CC0 for the dataset; ratify
the NC policy; size the minimum reviewer roster before M2. These are tracked in Open questions and
as M0/M1 tasks.

**Sign-off:** Plan is internally consistent, guardrail-compliant, and ready for maintainer review.
