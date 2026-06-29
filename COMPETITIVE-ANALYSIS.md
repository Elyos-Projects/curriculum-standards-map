# Competitive & Improvement Analysis — `curriculum-standards-map`

*Analyst pass against PLAN.md v0.1.0 (2026-06-28) and TASKS.md. Web-researched, cited. Scope:
an open, machine-readable crosswalk linking OER to curriculum standards (CCSS, NGSS, state,
international), referenced by stable identifiers (CASE GUID / ASN URI), educator-reviewed.*

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong: dual copyright gate (OER licence **and** framework terms), reference-by-
identifier as default, educator review as a hard ship gate, an honest confidence model, "delivered
not merged," and a canonical-record-first architecture. The 25-item Appendix A shows the obvious
failure modes were already closed. Findings below are corrections and sharpenings, ordered by impact.

**1.1 The NGSS deferral rests on a factually shaky IP premise (highest-impact finding).**
The plan defers NGSS because "copyright held by Achieve; reproduction terms are more restrictive …
likely `permitsTextReproduction: false`." The public record is materially different:
- Copyright to the NGSS is held by **the National Academies / NAP**, not Achieve (Achieve *managed*
  the development process); the **"NGSS" name/logo is a trademark owned by WestEd**.
- NGSS's own terms state that **"states, districts, schools, teachers and non-profit education
  entities may copy, reproduce, alter, adapt, edit, delete and rearrange any and all parts of the
  NGSS … without permission."** ([nextgenscience.org trademarks/copyright](https://www.nextgenscience.org/ngss-trademarks-and-copyright/ngss-trademarks-and-copyright))

So for a non-profit open crosswalk, NGSS **text reproduction is very likely permitted** (the binding
constraints are (a) the WestEd **trademark**/no-confusion rule, (b) a probable **commercial-use**
limitation that interacts with the project's own NC concerns, and (c) attribution). The plan's
*caution* (defer until confirmed in writing) is correct process; its *stated rationale and predicted
outcome* are wrong and should be corrected so the project doesn't needlessly exclude the single most
requested science framework. NGSS also already exists in ASN and CASE form, easing identifier work.

**1.2 The "cross-framework crosswalk" promise is under-modeled vs. the OER→standard focus.**
The brief and title imply **standard↔standard** crosswalks (CCSS↔state, US↔international,
old-version↔new-version). The plan is almost entirely **OER→standard** alignment (discovery). The
`AlignmentRecord` has no first-class representation of a standard-to-standard association
(`exactMatch` / `isRelatedFamily` / `isPartOf` / `isEquivalentTo`), which is precisely what CASE's
association model and schema.org expose. This is a scope/identity gap: either (a) explicitly narrow
the project name/exec-summary to "OER alignment" or (b) add a `StandardAssociation` record type.
Recommend (b) — it is the higher-value, more defensible differentiator (see §3–4) and reuses the
same gates.

**1.3 CASE vs. ASN identifier strategy — treat CASE as primary, ASN as legacy fallback, not co-equal.**
The plan "leans CASE primary, carry both always." Research supports going further: CASE Network now
holds **414 frameworks** and 1EdTech has published **free, authenticated K-12 ELA + Math standards
for all 50 states** with states publishing more directly ([1edtech CASE](https://www.1edtech.org/standards/case),
[CASE Network announcement](https://www.imsglobal.org/article/1edtech-consortium-announces-free-50-state-digital-k-12-academic-standards-registry)).
ASN is now **owned/operated by D2L** and, while its HTTP URIs still resolve
([asn.desire2learn.com](https://asn.desire2learn.com/content/welcome-achievement-standards-network)),
it shows little recent investment. "Carry both always" doubles maintenance for diminishing benefit.
Recommendation: **CASE GUID primary and required where one exists; ASN URI optional, captured only
when present** (valuable for older NGSS/CCSS URIs and pre-CASE frameworks). Keep the resolver test
for both but don't *gate* on ASN.

**1.4 GUID stability is asserted but the minting/versioning reality is unaddressed.**
CASE GUIDs are **per-publication**: a re-published or re-imported framework can mint *new* GUIDs, and
identity is only as stable as the publisher's practice. The plan records framework version (good) but
should state explicitly: (a) which CASE GUID is canonical when multiple publications of the same
framework exist; (b) that not all frameworks/statements have a CASE GUID (fallback to `statementId`
code + framework version); (c) a re-resolution/drift task when a framework is re-issued. The risk row
"identifiers drift" exists but the mitigation should name the GUID-minting problem specifically.

**1.5 CCSS licence reading is correct but should pin the operational limit.**
The CCSS public licence grants a "limited, non-exclusive, royalty-free license to copy, publish,
distribute, and display" the standards, **provided** the "© 2010 NGA Center / CCSSO" notice and
attribution appear, and it covers the standards (not the third-party examples)
([thecorestandards.org/public-license](https://www.thecorestandards.org/public-license/)). This means
**`permitsTextReproduction` is plausibly `true` for CCSS statement text** with the mandated notice —
the plan's default ("ID + link only, reproduce only where clearly permitted") is *safe* but may be
needlessly conservative. Action: confirm and, if reproducing, **the verbatim © notice string must be
a required, validator-enforced field** on any record carrying CCSS `statementText`. Add the notice
text to the gate artifact, not just "attribution recorded."

**1.6 Alignment-type vocabulary should map to schema.org's controlled `alignmentType`.**
The plan's `type` vocab (`teaches | practices | assesses | builds-toward | prerequisite`) is sensible
but is a custom vocabulary. schema.org `AlignmentObject.alignmentType` has its own established values
(e.g. `teaches`, `assesses`, `requires`, `educationalSubject`, `educationLevel`). For genuine interop
the model needs an explicit, documented **crosswalk of our type vocab → schema.org alignmentType**
and → CASE `CFAssociationType` (`ext:`/`isRelatedTo`/`exactMatch`/etc.). Without it, "schema.org
export" is structurally valid but semantically lossy. Add a vocabulary-mapping table to model-001.

**1.7 "Alignments are judgments, not facts" — make it machine-visible, not just prose.**
The plan rightly frames alignments as reviewed proposals. But the *export* should carry this:
`strength`, `coverage`, `aiConfidence`, reviewer-qualification, and a `assertedBy`/`notEndorsement`
flag should survive into the CASE/schema.org output so a downstream consumer can't silently render
the alignment as authoritative fact. Today the disclaimer is dataset-level; it should also be
record-level metadata.

**1.8 Coverage-scope realism.** Targets (≥600 reviewed records / 6 mo, ≥60% coverage of one grade-band
× subject) are credible for a donated-lane, educator-gated effort, precisely *because* educator review
is the throttle. Good. One realism gap: the plan never states the **expected human-review cost per
record** (minutes/record) feeding the roster-sizing open question — add it so M2's "throughput
improves without precision dropping" has a measurable baseline.

**1.9 Minor.** (a) `aiConfidence` 0..1 should specify it is *not* comparable across model versions —
record proposer model+version (already in provenance — cross-reference it). (b) The CC-BY-SA reasoning
("we link, not incorporate, so no derivative") is sound but should explicitly cover **evidence
snippets** quoted from CC-BY-SA OER, which *are* incorporated — keep them short/attributed/fair-use,
already implied by the bounded-access protocol but worth stating in the licensing section. (c) "low
risk + accuracy-critical" framing is handled well; keep it.

Overall: **complete, internally consistent, guardrail-compliant.** The two corrections that matter are
§1.1 (NGSS premise) and §1.2 (standard↔standard crosswalk under-modeled).

---

## 2. Competitive landscape

| Project | What it is | Strengths | Weaknesses (vs. our goal) |
|---|---|---|---|
| **1EdTech CASE Network** | Free registry of standards *frameworks* in CASE digital format; the de-facto interop spec | Authoritative, authenticated state standards; 414 frameworks; free 50-state ELA/Math; the identity layer everyone is converging on | Publishes the **standards themselves**, not OER↔standard alignments; cross-framework *associations* are sparse/uneven; not a discovery dataset for free resources |
| **Achievement Standards Network (ASN)** | Resolvable HTTP URIs for standards statements (CCSS, NGSS, WMO, etc.) | Stable globally-unique URIs; long history; includes NGSS/CCSS | Now D2L-owned with limited recent investment; an *identifier* service, not a living OER crosswalk; freshness/maintenance uncertain |
| **Common Standards Project** | Open, free **API** of standards for all 50 states + orgs, GUIDs; sponsored by Common Curriculum | Genuinely open, machine-readable, free API; GUIDs for integration | Standards *data*, not alignments; sustainability of a vendor-sponsored side project; no educator-reviewed OER linkage |
| **Academic Benchmarks** (now Instructure/Certica) | Commercial standards data **and correlations** (standard↔standard crosswalks) in XML/JSON | Has the actual *correlations* we'd build; powers Ed-Fi learning-standards reference data | **Proprietary/paid**; closed; the thing we'd open up |
| **Ed-Fi Alliance** | Open K-12 data-exchange standard; `LearningStandard` + crosswalk via Academic Benchmarks | Huge install base (250+ vendors, 32 states, ~12k districts); open standard | A *data-exchange* model, not an OER alignment corpus; its standards reference data is Academic-Benchmarks-sourced (paid) |
| **OER Commons / ISKME** | Large OER library with embedded CCSS/NGSS **alignment + Achieve rubric** tooling | Real OER + real alignments + scale; LRMI implementer | Alignments are **locked in the platform**, not published as an open exportable crosswalk dataset; quality varies; merit-rubric ≠ machine-readable alignment records |
| **Khan Academy / Illustrative Mathematics** | Single-publisher internal alignment of their own content to standards | High-quality, deeply curated within their catalog | Single-publisher, not cross-publisher; alignments not released as an open interoperable dataset |
| **Learning Registry** | Store-and-forward network for resource alignment/paradata | Pioneered open alignment-metadata exchange | Largely **dormant**; infra-centric; never produced a maintained cross-framework crosswalk |
| **EdGraph / state DOE catalogs** | State-DOE standards portals; knowledge-graph efforts | Authoritative per-jurisdiction | Per-state silos; not cross-framework; rarely OER-linked or openly exportable |

Sources: [1EdTech CASE](https://www.1edtech.org/standards/case) ·
[CASE Network 50-state registry](https://www.imsglobal.org/article/1edtech-consortium-announces-free-50-state-digital-k-12-academic-standards-registry) ·
[ASN (D2L)](https://asn.desire2learn.com/content/welcome-achievement-standards-network) ·
[Common Standards Project API](https://github.com/commonstandardsproject/api) /
[site](https://commonstandardsproject.com/) ·
[Academic Benchmarks via Ed-Fi/Elevate](https://www.ed-fi.org/technology-partners/elevate-standards-alignment/) ·
[Ed-Fi Data Standard](https://www.ed-fi.org/ed-fi-data-standard/) ·
[OER Commons/ISKME alignment](https://www.iskme.org/our-work/achieve-oer-online-evaluation-tool) ·
[Learning Registry](https://link.springer.com/chapter/10.1007/978-1-4939-0530-0_4).

**The landscape's shape:** there is a rich *identity* layer (CASE, ASN, Common Standards Project), a
*paid correlations* layer (Academic Benchmarks/Ed-Fi), and *platform-locked alignments* (OER Commons,
Khan, IM). **No one ships an open, cross-publisher, educator-reviewed, exportable OER↔standard (and
standard↔standard) crosswalk dataset with provenance and confidence.** That is the gap.

---

## 3. Gaps we can fill

1. **An open, downloadable OER↔standard alignment *dataset*** (CSV/JSON/CASE/schema.org) — not a
   platform feature, not a paywalled correlation, not an API behind a vendor. Nobody offers this.
2. **Cross-publisher coverage.** Khan/IM/OpenStax each align *their own* content; we align *across*
   publishers to the *same* standard, which is what a teacher actually needs.
3. **Provenance + confidence as first-class data.** Competitors give a binary "aligned" tag; we ship
   `strength`, `coverage`, evidence, reviewer qualification, and a re-audited precision number.
4. **Standard↔standard crosswalks, opened.** Academic Benchmarks sells correlations; CASE has thin
   associations. An open CCSS↔state and US↔international crosswalk is genuinely scarce (see §1.2).
5. **A published "where the commons is thin" coverage ledger** — standards with *zero* aligned OER.
   No competitor exposes negative space; it's a uniquely useful funding/authoring signal for OER orgs.
6. **Honest dual-licence provenance** (both OER licence *and* framework terms, snapshotted) — most
   alignment tools never record the standards-IP side at all.

---

## 4. Differentiators to win

- **Open + exportable + interoperable, by construction.** Canonical record → CASE/schema.org/ASN
  projections under CC-BY; ingestible by OER Commons, Ed-Fi, any LMS. We are the *only* open node in
  a landscape of registries (no alignments), paywalls, and walled gardens.
- **Educator-reviewed, not auto-tagged** — and we *prove* it with a blind second-reviewer precision
  audit (≥95%). Trust is the moat; an AI-only crosswalk would be commoditised and distrusted.
- **Confidence + coverage you can filter on**, including the negative-space ledger competitors hide.
- **Dual-IP provenance** makes the dataset safe to reuse commercially-adjacent (clear licence chain),
  which platform-locked alignments can't offer downstream.
- **Cross-framework, not single-publisher / single-jurisdiction.** The standard↔standard layer (§1.2)
  is the defensible, hard-to-copy asset.

**Single strongest differentiator:** *the only open, machine-readable, educator-reviewed crosswalk
dataset carrying provenance + confidence that any platform can ingest* — competitors either keep
alignments inside a product (OER Commons, Khan) or sell correlations (Academic Benchmarks); CASE/ASN
give identities but not the alignments.

---

## 5. Claude API leverage (and the hard limits)

**Where Claude adds real throughput (all into a *review queue*, never auto-published):**
1. **Candidate-alignment proposer with rationale.** Given OER unit text + a standard statement,
   draft `(oer, standard)` candidates with `type`, `coverage`, a 0..1 confidence, and a short
   *evidence* rationale ("section 3 develops adding fractions with unlike denominators → 5.NF.B.1").
   This is the core M2 accelerator; framed as candidates, it directly attacks reviewer time.
2. **CASE / schema.org `AlignmentObject` generation + validation.** Claude drafts the JSON-LD /
   CASE-compatible projection from the canonical record and pre-checks it against the pinned spec,
   leaving humans to verify rather than author.
3. **Coverage / gap analysis.** Over a framework skeleton, cluster which statements have zero/weak
   OER coverage and summarise the negative-space ledger into human-readable "what to author next."
4. (Secondary) **Licence/terms triage assist** — extract licence id, derivative/commercial flags,
   and the attribution string from a source's licence page into the gate artifact *for human sign-off*.
5. (Secondary) **Standard↔standard candidate associations** — propose `exactMatch`/`isRelatedTo`
   links across frameworks for expert review.

**Where Claude must NOT decide (hard guardrails — encode in the workflow):**
- **The alignment judgment is the educator's, not the model's.** `aiConfidence` is advisory triage
  metadata; the ship signal is `review.status: accepted`. An unreviewed AI alignment never ships.
- **Standards IP is human-verified.** Claude may *draft* the licence/terms reading; a qualified
  licence/terms reviewer must confirm `permitsReference` / `permitsTextReproduction` against the
  actual licence. No mapping proceeds on a model's legal opinion.
- **No fabricated standards, codes, GUIDs, or statement text.** Every `statementId` / CASE GUID /
  ASN URI must resolve against the official registry; the validator must reject anything that doesn't.
  Claude must not "guess" a plausible code or paraphrase a standard as if verbatim.
- **Crosswalks are reviewed *proposals*, not authoritative facts** — and the record/export must say so
  (§1.7). Claude must not assert endorsement, merit, or "certified aligned."
- **Automation-bias countermeasure:** because a confident proposer nudges reviewers to over-accept,
  reviewers must record independent evidence, and the blind audit measures real precision.

---

## 6. Ten concrete optimizations

1. **Correct and re-open the NGSS question (§1.1).** Replace the inaccurate "Achieve copyright /
   likely no reproduction" rationale with the real position (NAP copyright, WestEd trademark, broad
   non-profit reproduction right) and run the framework-terms gate to likely *un-defer* NGSS.
2. **Add a `StandardAssociation` record type (§1.2)** for standard↔standard crosswalks
   (`exactMatch`/`isRelatedTo`/`isPartOf`/version-supersedes), reusing the same dual gate + review.
   This makes the project name true and unlocks the most defensible asset.
3. **Make CASE GUID primary/required-where-available; demote ASN to optional legacy capture (§1.3).**
   Cut maintenance, follow the ecosystem's center of gravity.
4. **Ship a vocabulary-mapping table** (our `type`/`strength` → schema.org `alignmentType` → CASE
   `CFAssociationType`) and enforce it in the exporters so interop is semantic, not just syntactic.
5. **Make the CCSS © notice a validator-enforced field** on any record carrying CCSS `statementText`
   (§1.5); store the exact notice string in the gate artifact.
6. **Carry confidence/coverage/notEndorsement into the exports** (§1.7) so consumers can't render a
   reviewed proposal as authoritative fact.
7. **Seed standards skeletons directly from CASE Network's free 50-state registry** rather than
   building per-state — the authenticated state ELA/Math data already exists and is free.
8. **Publish a tiny "ingest contract" + sample importer** (one for Ed-Fi `LearningStandard`, one for
   schema.org `educationalAlignment`) so a consuming partner's lift is near-zero — directly attacks
   the "delivered, not merged" risk and the unsecured-partner blocker.
9. **Record a per-record human-review-minutes figure** (§1.8) to give M2's throughput target a real
   baseline and to size the reviewer roster honestly.
10. **Add a public link-rot/drift dashboard** (OER URL resolves + CASE GUID resolves) generated in CI,
    turning the staleness risk into a visible, fixable maintenance signal and a reuse-trust signal.

---

## 7. Parallel & perpendicular spin-offs

- **Reusable crosswalk engine (perpendicular, high value).** The canonical-record + dual-gate + review
  queue + confidence model + multi-format exporter is **domain-agnostic mapping infrastructure**.
  Extract `packages/crosswalk-core`: any "map entities in source A to entities in framework B with
  provenance, confidence, and expert review" problem reuses it.
- **`rare-cancer-registry-templates` (direct methodology tie).** That project's crosswalk of local
  data fields → registry standards (e.g. ICD-O, common data elements) is the *same shape* as
  standard↔standard mapping — share the engine, the confidence model, and the "judgment not fact +
  expert sign-off" gate.
- **`oer-everything` (parallel, symbiotic).** This crosswalk is the *index layer* over an OER corpus;
  oer-everything is a natural producer/consumer. Co-design the alignment record so oer-everything
  resources carry our identifiers natively.
- **`teacher-lesson-plans` (parallel consumer).** Lesson plans tagged with CASE GUIDs become instantly
  discoverable-by-standard via the crosswalk; the proposer can suggest standards for a draft plan.
- **`quiz-bank-open` (parallel consumer).** `assesses`-type alignments map items→standards; a quiz
  bank is the highest-value consumer (assessment alignment is what districts most demand).
- **CASE/crosswalk **MCP server** (tooling spin-off).** An MCP server exposing tools like
  `resolve_standard(case_guid|asn_uri)`, `search_standards(framework, query)`,
  `propose_alignment(oer_text, standard_id)`, and `export_case(record)` lets any agent (Claude or
  otherwise) author and verify alignments against the live registry — a clean, reusable surface that
  also enforces the "no fabricated codes" guardrail (every id is resolved, not invented).
- **Coverage-ledger-as-a-service (perpendicular).** The negative-space ledger ("standards with no open
  resource") is a fundable signal for OER authoring orgs and foundations — a standalone product.

---

## 8. Open questions

1. **NGSS re-evaluation:** given the broad non-profit reproduction right, what *exactly* are the
   binding limits — WestEd trademark use, a commercial-use boundary, attribution form? Does that let
   us un-defer NGSS text reproduction for a CC-BY non-profit dataset? (See §1.1.)
2. **Standard↔standard scope:** do we commit to the crosswalk layer now (§1.2), and if so which first
   pair (CCSS↔a state, or AU↔England)? This decides whether the project name is honest.
3. **Identifier authority:** when a framework has multiple CASE publications/GUIDs, whose GUID is
   canonical for us, and how do we re-bind on re-publication? (§1.4)
4. **CCSS text reproduction:** confirm we may reproduce statement text with the mandated © notice, and
   make the notice a required field. (§1.5)
5. **CC-BY vs CC0 for the dataset** — does a target consumer (Ed-Fi/Academic-Benchmarks-adjacent, or
   OER Commons) need CC0 for frictionless ingest? Revisit if so.
6. **Reviewer economics:** minutes-per-record and the minimum roster (subjects × grade bands) before
   M2 scaling, so the educator gate is neither bottleneck nor skipped.
7. **Partner-first vs. self-serve:** is the fastest "delivered" path an OER Commons / Ed-Fi ingest, or
   a self-serve open release others fork? Which ingest contract do we build first? (§6.8)
8. **Funded-lane future:** a budget-capped batch proposer could 10× candidate generation — worth a
   thin spike to size cost-per-candidate, even though it's out of scope for v0.1.0.
