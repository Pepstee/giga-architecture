# Jarvis sovereign runtime — implementation blueprint

**Status:** PROPOSED LIVING BLUEPRINT · PLANNING AUTHORITY ONLY  
**Version:** 1.2  
**Date:** 2026-08-19  
**Target:** the unratified architecture in 11_JARVIS_SOVEREIGN_RUNTIME_PROPOSED_ARCHITECTURE.md  
**Current implementation evidence:** codebase-analysis-docs/CODEBASE_KNOWLEDGE.md and machine checks  
**Authority:** this blueprint grants no data access, external-action, identity, deployment, policy-change
or Personal Vault authority

## 1. Purpose

This is the durable execution blueprint for turning the current GIGA codebase into the proposed
Jarvis sovereign runtime. It is designed to remain useful today, after a quiet week, after a
six-month pause, or after partial abandonment.

GIGA is also the operator's live work folder. Architecture work is therefore subordinate to
current work: it must reduce a demonstrated problem, preserve existing workflows and stop before
an uncertain or oversized change can consume the operator's time. Doing no architecture work is
a valid outcome.

The unit of progress is an independently valuable implementation slice, not a calendar phase. A
slice may depend on earlier verified slices, but it may never depend on a future slice for its own
correctness or usefulness. If work stops after any signed-off slice:

- the last deployed state remains runnable;
- existing manual paths remain available;
- completed slices keep producing their stated value;
- incomplete code is not promoted into an official path;
- no later slice is required to make an earlier receipt truthful;
- future work can resume from durable contracts and evidence rather than chat history.

The catalogue is deliberately open-ended. Its size is an observation, never a target or planning
constraint. New evidence may split a compound slice, add a missing outcome, or prove that a slice
should be merged, superseded or dropped. Stable IDs preserve references; the architecture never
optimises for a round total.

## 2. Relationship to existing documents

This file has a distinct responsibility and lifecycle:

| Document | Responsibility | Why it cannot serve as this blueprint |
|---|---|---|
| 04_VIRTUAL_TWIN_TARGET.md | Current canonical virtual-twin target | Registered historical target; it predates the operator's sovereign Jarvis requirements. |
| 05_TRANSITION_ROADMAP.md | P0–P8 evidence and promotion strategy | Coarse phase plan whose gates bundle many capabilities; not a selectable implementation backlog. |
| 10_DETERMINISTIC_EXECUTION_BOUNDARY.md | Selected policy-kernel design | Covers one cross-cutting boundary, not the complete runtime. |
| 11_JARVIS_SOVEREIGN_RUNTIME_PROPOSED_ARCHITECTURE.md | Proposed end-state architecture and requirements | Defines what and why; its S0–S11 stages are too broad for intermittent implementation. |
| This blueprint | Living slice catalogue, canonical completion checklist, dependencies, gates and restart protocol | Tracks what is open, claimed, blocked or closed and how to build the target without changing the authority of the documents above. |

Until proposal 11 is ratified, only slices compatible with current canon and current authority may
be implemented. Ratification is itself Slice JSR-001; it is not implied by the existence of this
file.

# Part I — Instruction manual

This part governs how a person or AI uses the blueprint. It is not optional process decoration: a
slice that bypasses these rules is not READY, regardless of how attractive the implementation is.

## 3. What “independently valuable” means

A slice qualifies only if all six conditions hold:

1. **Closed outcome:** it ends with a usable behaviour, contract, diagnostic, safety control or
   restore capability—not merely scaffolding that waits for another component.
2. **Backward compatibility:** existing official behaviour either remains unchanged or is replaced
   behind a proven rollback seam.
3. **No future dependency:** its acceptance proof uses only prerequisites that already exist.
4. **Observable value:** an operator or machine can demonstrate what improved immediately.
5. **Abandonment safety:** stopping after the slice cannot strand a half-migrated writer, schema,
   authority path or deployment.
6. **Durable evidence:** code, tests, documentation and a receipt survive beyond the implementing
   conversation.

A database table with no supported lifecycle is not a complete slice. A shadow classifier that
produces a useful mismatch report is complete even before enforcement, because the report has
standalone diagnostic value.

## 4. Slice lifecycle and status language

Every slice has one and only one status:

| Status | Meaning |
|---|---|
| PROPOSED | Defined here but not yet checked against its current prerequisites. |
| READY | Prerequisites and required operator decisions are satisfied; implementation may begin. |
| IN PROGRESS | An isolated branch/worktree exists; no production claim is made. |
| VERIFIED | Acceptance commands pass and evidence is recorded, but deployment is not necessarily active. |
| DEPLOYED | The verified slice is active on an official surface with a rollback path. |
| PAUSED | Work stopped before verification; the last verified state remains authoritative. |
| BLOCKED | A named prerequisite, decision or external dependency is missing. |
| NEEDS REVALIDATION | A previously closed receipt no longer matches the current compatibility fingerprint; re-run its proof before deciding whether new repair work is needed. |
| SUPERSEDED | A later slice replaces the interface through an explicit compatibility/migration receipt. |
| DROPPED | Deliberately abandoned; no future plan should silently assume it exists. |

READY does not mean authorised to widen Jarvis's authority. It means safe to implement within the
authority already granted.

### 4.1 Canonical checklist markers

The **Checklist** and **Evidence / work order** columns in the Slice catalogue are the canonical
manual progress ledger until JSR-003 deploys a machine-readable registry. Readiness waves,
recommendations, chat history and similarities to existing code are not status evidence.

| Marker | Allowed lifecycle status | Selection meaning |
|---|---|---|
| `[ ]` | PROPOSED or READY | Open. It may be considered only after the selection and proof-of-absence gates. |
| `[~]` | IN PROGRESS or PAUSED | Claimed. Resume from the linked work order; do not start a second implementation. |
| `[!]` | BLOCKED or NEEDS REVALIDATION | Do not implement the original scope. Resolve the named blocker or re-run the existing receipt's proof. |
| `[x]` | VERIFIED, DEPLOYED, SUPERSEDED or DROPPED | Closed. Never select or repeat this slice; read the receipt/status to understand the outcome. |

A checked box means **closed**, not necessarily deployed: its exact status distinguishes verified,
deployed, superseded and deliberately dropped work. A checked box without a durable evidence link
is invalid and must fail review. No row may be pre-checked merely because similar code appears to
exist; completion requires the slice's own gate and receipt.

### 4.2 Anti-repetition protocol

Before any slice work:

1. Find the catalogue row by stable ID and inspect its marker, exact status and evidence link.
2. If it is `[x]`, stop. Use the recorded capability; do not rebuild, polish or “finish” it again.
3. If it is `[~]`, open the linked work order and resume from its last verified checkpoint rather
   than restarting discovery or implementation.
4. If it is `[!]`, perform only the named unblock/revalidation activity. Revalidation may rerun
   checks; it is not permission to repeat implementation.
5. Only `[ ]` rows can enter the normal selection gate. When selected, atomically change the row to
   `[~] IN PROGRESS` and link its work order before modifying implementation files.
6. At sign-off, atomically write `[x] VERIFIED` or `[x] DEPLOYED` and the exact receipt link. A
   deliberate closure uses `[x] SUPERSEDED` or `[x] DROPPED` with its decision receipt.

Never reopen or erase a closed history row. If later drift breaks a completed capability, first mark
it `[!] NEEDS REVALIDATION` and rerun its acceptance proof. If the proof fails and new implementation
is genuinely required, create a narrowly scoped repair/superseding slice with a new stable ID and
link both directions. This preserves the truth that the original work was completed for its recorded
source and compatibility versions while preventing a future worker from unknowingly doing it twice.

Useful manual queries:

- open work: `rg '^\| JSR-[0-9]{3} \| \[ \]' architecture/12_JARVIS_SOVEREIGN_RUNTIME_IMPLEMENTATION_BLUEPRINT.md`
- active/paused work: `rg '^\| JSR-[0-9]{3} \| \[~\]' architecture/12_JARVIS_SOVEREIGN_RUNTIME_IMPLEMENTATION_BLUEPRINT.md`
- blocked/revalidation work: `rg '^\| JSR-[0-9]{3} \| \[!\]' architecture/12_JARVIS_SOVEREIGN_RUNTIME_IMPLEMENTATION_BLUEPRINT.md`
- closed work: `rg '^\| JSR-[0-9]{3} \| \[x\]' architecture/12_JARVIS_SOVEREIGN_RUNTIME_IMPLEMENTATION_BLUEPRINT.md`

## 5. Operating laws

### 5.1 The blueprint is a menu, not a queue

- Do not execute catalogue IDs in numeric order.
- Do not optimise for completed-slice count, percentage complete or backlog velocity.
- Select at most one implementation slice at a time.
- Choose a slice only because it addresses current friction, repeated manual cost, a concrete
  present risk or a prerequisite for already-selected near-term work.
- Architectural symmetry, theoretical completeness and possible future usefulness are not enough.
- If no slice clears the selection gate, continue normal work and leave the blueprint unchanged.

Success is measured by verified friction removed, risk reduced or capability gained—not by how
much of the catalogue has been touched.

### 5.2 Selection gate

Before marking a slice READY, answer all of the following:

1. What current, observable problem does this solve?
2. Who or what experiences the problem, and how often?
3. What evidence shows the problem exists now?
4. What is the smallest independently useful outcome?
5. What happens if the slice is never built?
6. Can configuration, documentation, an existing command or a manual procedure solve it instead?
7. Is the expected value greater than the implementation, verification and maintenance cost?

An answer such as “the architecture says so” fails the gate. When several slices qualify, prefer
the smallest one that protects or improves work already happening in GIGA.

### 5.3 Proof of need and proof of absence

Implementation cannot begin until the worker has searched the relevant scope for an existing
solution. Inspect canonical documentation, registries, source, tests, scripts, configuration,
deployment files and known sibling-product boundaries. Record the search terms, paths and closest
candidates in the work order.

Classify the outcome in this order:

1. **ADOPT EXISTING** — use the canonical behaviour as it is.
2. **CONFIGURE EXISTING** — expose or correct configuration without new machinery.
3. **REPAIR EXISTING** — fix the owned implementation that already has the responsibility.
4. **EXTEND EXISTING** — add the smallest missing behaviour to the canonical owner.
5. **WRAP EXISTING** — preserve ownership and add a compatibility or policy boundary.
6. **BUILD NEW** — create a genuinely distinct responsibility only after the earlier options fail.
7. **DO NOTHING / DROP** — the value does not justify change.

Discovering that a capability already exists can complete the work order immediately. A new file,
service, schema or abstraction requires a written explanation of why the closest canonical owner
cannot serve the responsibility, lifecycle, ownership boundary or required format.

### 5.4 Protection of current work

- Treat the working folder as production data, even when no service is deployed.
- Do not move, rename or reorganise live files for architectural neatness.
- Do not replace a current command or workflow without a verified compatibility path and rollback.
- Preserve unrelated and dirty user changes; never absorb them into a slice.
- Keep Market Aligner, Orchestrator and other product ownership boundaries intact.
- Do not make external writes, invoke providers, change credentials or widen authority unless the
  selected slice and operator authorisation explicitly require them.
- Prefer an isolated branch/worktree and disposable state for implementation and failure tests.
- If architecture work conflicts with current work, current work wins and the slice becomes PAUSED.

### 5.5 Default time budgets

Unless the operator explicitly authorises a different budget:

| Activity | Default ceiling | Required result |
|---|---:|---|
| Orientation and current baseline | 15 minutes | Named current problem, affected owner and baseline evidence |
| Reuse/proof-of-absence search | 20 minutes | Closest existing candidates and outcome classification |
| Normal implementation attempt | 60–90 minutes | Verified useful increment or an explicit stop result |
| Any single unattended slice | 4 hours absolute | Stop, preserve the last green state and report; never roll into another slice |
| Continuation into another day | Not permitted by default | Requires fresh operator authorisation and a revised work order |

If the smallest useful implementation cannot credibly fit inside the authorised budget, decompose
it before coding. Research or design may be a separate read-only work order, but it cannot claim
the implementation slice is complete. Finishing early means stopping early; unused time is not a
reason to expand scope.

### 5.6 Mandatory stop conditions

Stop implementation and report the evidence when any condition occurs:

- 30 minutes pass without new evidence or a reduction in uncertainty;
- the same approach or blocker fails twice;
- the discovered scope or prerequisite set grows to more than twice the work order estimate;
- an adequate canonical implementation is found;
- present value becomes uncertain or the acceptance proof cannot be stated;
- progress requires unrelated cleanup, migration or dependency upgrades;
- the change would interfere with current work or make rollback uncertain;
- new operator authority, personal-data access or an external write is required;
- an external dependency is unavailable; or
- the baseline is already broken outside the selected slice's scope.

The correct result is then ADOPT EXISTING, PAUSED, BLOCKED or DROPPED—not prolonged autonomous
experimentation. Preserve diagnostics and the last verified state so another session can decide
whether a smaller slice is worthwhile.

### 5.7 Rules for background or unattended AI work

- Execute exactly one authorised work order; never interpret “improve the architecture” as open
  permission to work through the catalogue.
- Do not perform adjacent refactors, cleanup, dependency upgrades, schema redesigns or service
  creation unless they are explicit acceptance requirements.
- Do not start a second slice, spawn a long-running programme or continue overnight to fill time.
- Retry a failed approach no more than twice.
- Do not widen data, tool, identity or external-action authority to bypass a blocker.
- Preserve a runnable checkpoint before each risky step and return to it on failure.
- Report material progress or uncertainty at least every 30–45 minutes during attended work.
- When the acceptance gate passes, stop and hand control back; do not polish beyond the work order.

There is intentionally no “implement the giga-architecture” work mode. Valid modes are bounded
inspection, adoption/configuration, repair, extension/wrapping, or a separately authorised build.

### 5.8 Decision rationale

| Operator need or observed failure mode | Manual decision | Why it follows |
|---|---|---|
| GIGA remains the folder used for current work | Current work has priority; compatibility and rollback are mandatory | Architecture cannot be called useful if implementing it interrupts the work it is meant to support. |
| Large AI projects can run for days on something unnecessary | Every attempt has a narrow work order, hard time ceiling and mandatory stop conditions | Autonomy without a stopping contract turns uncertainty into wasted time and uncontrolled scope. |
| Work may already exist elsewhere in the folder or a sibling product | Proof of absence and the adopt/configure/repair/extend/wrap hierarchy precede new code | Reusing the canonical owner is faster and avoids two implementations drifting apart. |
| Spare implementation time is intermittent | Each selected slice must be useful alone and preserve the last working state | Pausing after one hour, one week or six months must not strand scaffolding or a half-migration. |
| The eventual architecture is broad and will change | The catalogue is open-ended and stable IDs are references, not a completion target | Future evidence can split, supersede or drop work without invalidating already verified value. |
| Jarvis may eventually act autonomously and impersonate the operator | Planning, data access, identity and external-action authority stay explicit and separate | A blueprint or unattended worker must never manufacture permission merely because a capability is technically possible. |
| The operator wants explanations for architectural choices | Work orders and completion reports bind each change to a present problem and evidence | A future session can assess the reason for a decision without trusting chat history or aesthetic preference. |
| Completed work must not be repeated | Every catalogue row has a canonical marker and evidence link; closed rows are immutable | A future person or AI can distinguish reuse, resumption, revalidation and genuinely new repair work before touching code. |

## 6. Universal slice work-order contract

Before implementation, copy the chosen catalogue entry into a slice work order and resolve:

- exact objective and non-goals;
- prerequisite receipt identifiers and source hashes;
- affected official entry points and truth-domain owner;
- schemas/interfaces changed, including compatibility version;
- authority before and after the slice;
- expected external or state receipt;
- canonical verification commands;
- failure injections and negative controls;
- rollback command or restoration procedure;
- observability added;
- migration hazards H1–H8 checked, with non-applicable hazards explicitly recorded;
- documentation and registry entries that must change in the same commit.

### Copyable work order

> **Slice:** [ID and name]  
> **Current checklist state:** [marker, lifecycle status and prior evidence/work-order link]  
> **Outcome class:** [ADOPT EXISTING / CONFIGURE / REPAIR / EXTEND / WRAP / BUILD NEW]  
> **Current problem and evidence:** [observable present condition]  
> **Smallest useful outcome:** [one end-to-end result]  
> **Non-goals:** [explicitly excluded adjacent work]  
> **Search scope and terms:** [documents, paths, registries, code and tests inspected]  
> **Closest existing candidates:** [what was found and why it is or is not sufficient]  
> **Allowed paths/systems:** [exact scope]  
> **Forbidden changes/actions:** [current-work, authority and external-effect boundaries]  
> **Prerequisites and receipts:** [verified existing dependencies only]  
> **Time budget:** [orientation, search and implementation ceilings]  
> **Acceptance commands/evidence:** [objective pass condition]  
> **Rollback:** [command or restoration procedure]  
> **Early-stop conditions:** [defaults plus slice-specific conditions]  
> **Completion report location:** [durable receipt/log]

Before touching code, the worker must present a concise preflight containing: the problem, scope
searched, closest existing implementation, reason reuse is insufficient, smallest proposed change,
current workflows at risk, rollback, time budget and evidence that would cause an early stop.

### Definition of Done

A slice is VERIFIED only when all applicable items pass:

1. The implementation is the smallest end-to-end path that delivers the catalogue outcome.
2. Existing canonical tests pass; focused positive, negative, replay and rollback tests pass.
3. No model output can grant authority, directly commit protected state or self-certify completion.
4. A receipt binds source, configuration, policy/registry version, inputs, outcome and proof.
5. Failure leaves the previous verified state usable.
6. Security-sensitive files retain owner-only permissions and secrets are absent from bundles/logs.
7. Documentation, command inventory, architecture index and the canonical checklist row are updated
   together; a closed marker and its receipt link land atomically.
8. The slice's immediate value is demonstrated under real or faithful disposable conditions.
9. Dirty or unrelated user changes are neither absorbed nor rewritten.
10. Any temporary weakness has a named closing slice and is registered as residual risk.

Passing unit tests alone does not satisfy deployment or external-effect claims.

### Completion report

Every finished attempt records:

- outcome class and final lifecycle status;
- elapsed time against the authorised budget;
- files and systems inspected or changed;
- verification commands and evidence;
- rollback result or confirmation that no runtime state changed;
- effect on current work;
- remaining risk or blocker; and
- a possible next slice, clearly marked as not authorised and not automatically started.

## 7. Branch, pause and restart protocol

1. Select one READY slice; do not combine unrelated catalogue IDs.
   A `[x]`, `[~]` or `[!]` row cannot be selected as new implementation work.
2. Re-read 00_MASTER.md, CONTEXT.md, this blueprint, the target architecture section and affected
   implementation/tests.
3. Refresh the live baseline. Never trust the dated numbers in this document as current runtime
   truth.
4. Create an isolated slice branch from the actual trunk. Do not stack it on another unmerged slice.
5. Preserve the last verified implementation until the new path passes.
6. If time expires, write `[~] PAUSED`, retain the linked work order and leave no official caller
   pointing at incomplete code.
7. After verification, atomically write the closed checklist marker, exact status, receipt,
   commands, outputs, hashes, rollback result and residual risks.
8. Merge/reconcile before beginning a dependent slice.
9. After a long pause, validate prerequisite receipts and compatibility versions. If drift
   invalidated a gate, use NEEDS REVALIDATION and a new repair slice when necessary; never silently
   redo the closed slice.

### Six-month restart checklist

- verify repository/machine authority and current canonical checkout;
- run immutable-export, memory, health, maturity and root test gates;
- inspect blueprint status receipts rather than prose alone;
- exclude every `[x]` row from selection and resume `[~]` rows from their linked checkpoints;
- check whether external APIs, credentials, runtimes or platform rules changed;
- re-run the official-surface and bypass inventories;
- select the smallest READY slice whose standalone value still matters;
- mark obsolete assumptions SUPERSEDED rather than editing history.

### Normal operating cadence

1. Do current work normally.
2. Record recurring friction or concrete risk when it appears; do not manufacture architecture work.
3. When time is deliberately available, run the selection and proof-of-absence gates.
4. Choose zero or one qualifying slice and issue a bounded work order.
5. Verify, record and stop.
6. Return to normal work. Reassess later from current evidence rather than continuing a backlog by
   momentum.

# Part II — Evidence, architecture and catalogue

## 8. Current feasibility and safety baseline

The current codebase is alive enough for incremental modernization:

| Component | Current testability | Safety rung | Evidence and implication |
|---|---|---|---|
| Root twin_core library | Lit | L4 for owned unit/contract scope | 85 root tests pass. Existing event, proposal, receipt and lineage behaviour can be protected by automated gates. |
| Telegram/daily memory path | Lit but operationally stale | L3 | Transaction and replay tests pass; live Jarvis is stopped and daily success is stale. Use characterization plus disposable fault injection before rewiring. |
| Jarvis autonomous engine | Not implemented | L1 | Only proposal/design evidence exists. Build new vertical slices; do not claim legacy equivalence. |
| Constitution/kernel/router | Design-only | L1 | Architecture 10 is selected but undeployed. Shadow mode is the testability milestone. |
| Personal Vault and broker | Not implemented | L0 | Establish format, encryption and restore contracts before any live migration. |
| Market Aligner/JAA | Independent lit product | L3–L4 within its own repository gates | Integrate only through versioned adapters; never make its internal test suite a hidden root prerequisite. |
| Orchestrator-v3 | External to this checkout | UNVERIFIED | Adapter work starts read-only; execution waits for a canonical checkout and certificate contract. |
| Behavioural/LoRA twin | Research artifacts, unpromoted | L1–L2 | Keep outside production; baseline and promotion harness come first. |

Safety rungs used here:

- **L0:** documentation/inspection only;
- **L1:** frozen contracts or self-contained golden fixtures;
- **L2:** runnable smoke path and rollback scaffolding;
- **L3:** automated component/contract tests plus failure injection;
- **L4:** deployed canary/equivalence evidence and operational rollback.

The Testability Milestone for a new component is the first slice that runs it under at least one
meaningful automated test. JSR-004 establishes the local canonical gate. JSR-076 is the separate
hosted-CI milestone if an authoritative remote exists. CI enforcement remains a manual
operator/platform action.

## 9. Canonical verification inventory

Commands are current as of 2026-08-19 and must be revalidated before each slice:

| Purpose | Command |
|---|---|
| Root owned tests | python3 -m unittest discover -s tests -p 'test_*.py' |
| One root test module | python3 -m unittest tests.test_twin_store |
| Immutable exports | python3 -m twin_core verify-exports |
| Memory validity | python3 -m twin_core validate-memory |
| Runtime health | python3 -m twin_core health |
| Roadmap status | python3 -m twin_core maturity |
| Market Aligner gates | Use only commands declared by its own AGENTS.md and pyproject.toml for the affected adapter slice. |

The root currently has no canonical package manifest, dependency lock, linter, formatter,
typechecker or repository CI workflow. Slices must not invent success for missing gates.

## 10. Target dependency shape

The catalogue is a directed acyclic graph, not a numbered sequence or fixed-size programme:

| Track | Stable ID family | Primary output |
|---|---|---|
| G — Governance and truth | JSR IDs assigned to governance outcomes | Ratified target, baseline, living registry, official-surface truth and optional CI |
| F — Portable foundation | JSR IDs assigned to portability outcomes | Host-independent runtime and restorable faceless work state |
| S — State, evidence and memory | JSR IDs assigned to state outcomes | Typed epistemics, actions, receipts, projections and independently managed checkpoints |
| P — Policy and authority | JSR IDs assigned to policy outcomes | Constitution, signing, registry, kernel and bypass resistance |
| C — Context, compartments and Vault | JSR IDs assigned to context outcomes | Faceless compartments plus separately mountable and auditable Personal Vault |
| R — Routing and cognition | JSR IDs assigned to cognition outcomes | Typed routing, least-context model roles, budgets and provider resilience |
| A — Agency and continuity | JSR IDs assigned to agency outcomes | Goals, tasks, initiative, supervision, fencing, restore and safe updates |
| D — Domain integration | JSR IDs assigned to domain outcomes | Independently promoted adapters for Orchestrator, Market Aligner and other systems |
| I — Identity and learning | JSR IDs assigned to identity outcomes | Governed representation, Artiom Test and separately evaluated behavioural research |

The main dependency spine is:

**truth baseline → portable runtime/state → typed events/actions/receipts → Constitution/kernel →
context grants and route plans → bounded action → domain adapters → representation and sustained
autonomy**

Parallel work is permitted wherever explicit dependencies do not intersect. A track is not a release
train: completing Track G does not require starting Track F, and completing any slice does not
commit the operator to its neighbours.

## 11. Starter menu, not a starter sequence

These are plausible low-risk entries only when their named problem is current:

| Candidate | Select it only when | Do not select it merely because |
|---|---|---|
| JSR-015 — Repair current memory health | The red memory gate is affecting present memory trust or work | It is an open item in this document |
| JSR-004 — Canonical local verification runner | Repeated manual verification is causing mistakes or material delay | A single command would look cleaner |
| JSR-008 — Host-independent configuration/path resolver | Hard-coded paths block a real move, test or device workflow | Portability is an eventual goal |
| JSR-002 — Reproducible current-state baseline | A risky change, drift investigation or recovery exercise needs a durable baseline | Baselines are generally good practice |
| JSR-007 — Runtime manifest/dependency lock | A clean environment or device transfer is currently failing or imminent | Dependency locking is architecturally conventional |

Choose zero or one through the operating laws. There is no default first-week batch. JSR-001 may
be reviewed when the operator wants to decide the target, but ratification must not be rushed merely
to create coding work.

## 12. Slice catalogue

### Track G — Governance and truth

Track rule: these slices change planning truth, evidence or enforcement coverage. They grant no
runtime authority.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-001 | [ ] PROPOSED | — | Ratify, amend or reject the sovereign target — ADOPT | None | Bind the exact hash of proposal 11, record accepted laws, rejected clauses, supersession scope and unresolved operator decisions. Gate: named operator receipt plus refreshed architecture index; no runtime change. Basis: R-01–R-22. | Removes target ambiguity. If work stops, current canon remains intact and the signed decision tells future work what may be built. |
| JSR-002 | [ ] PROPOSED | — | Reproducible current-state baseline — ADOPT | None | Capture branch/HEAD, dirty-state manifest, file hashes, canonical commands, test/health/maturity outputs, runtime processes and state counts in a machine-readable receipt. Gate: repeated capture is stable except declared volatile fields. | Makes every later comparison honest and is useful immediately for recovery, audit and drift detection. It does not require target ratification. |
| JSR-003 | [ ] PROPOSED | — | Machine-readable slice registry — BUILD | This blueprint | Replace the manual checklist columns with one schema-backed ledger for ID, status, prerequisites, source hashes, work-order path, receipt, deployment and supersession, then generate the same catalogue view from it. Gate: every registered ID validates, cycles/unknown prerequisites, illegal marker/status pairs and checked rows without receipts fail; manual and generated states reconcile before the ledger becomes authoritative. | Prevents a six-month pause from turning the plan into archaeology without creating a second status truth. If abandoned, the existing receipt-backed checklist remains usable. |
| JSR-004 | [ ] PROPOSED | — | Canonical local verification runner — BUILD | JSR-002 | Provide one root command that runs only owned gates, emits structured results and distinguishes missing, failed and inapplicable checks. Gate: deliberate test/export/memory failures produce stable non-zero results. | Gives every local or future hosted workflow a reusable quality boundary without assuming a remote repository exists. |
| JSR-005 | [ ] PROPOSED | — | Architecture lineage and blueprint adoption — EXTEND | JSR-001, JSR-003, JSR-004 | Register proposal 11 and this blueprint through the existing append-only lineage process without falsifying them as deployed architecture. Gate: old revisions remain queryable; current hashes and sources audit cleanly. | Stops design documents drifting silently. Pausing later work leaves trustworthy design provenance. |
| JSR-006 | [ ] PROPOSED | — | Official entry-point and privileged-call inventory — BUILD | JSR-002 | Deterministically enumerate official CLIs, daemons, schedulers, provider calls, subprocesses, network writes, state writers and external adapters; classify owner and authority. Gate: known positive/negative fixtures and current tree scan reconcile. | Produces an immediate attack/bypass map and migration scope. It remains useful even if no kernel is ever built. |
| JSR-076 | [ ] PROPOSED | — | Hosted CI workflow and enforcement handoff — BUILD | JSR-004, authoritative remote decision | Run the canonical local gate on pushes/changes using a pinned supported runtime and publish structured results. Gate: a seeded failing branch is red and a clean branch is green. Required-check/branch-protection activation is recorded as a separate manual operator action. | Adds remote regression feedback when a real remote exists; absence of this slice never weakens the local gate. |

### Track F — Portable foundation

Track rule: portability slices must improve today's manual/faceless operation and must not place
secrets or Personal Vault data inside transferable bundles.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-007 | [ ] PROPOSED | — | Canonical root runtime manifest and dependency lock — BUILD | JSR-002, JSR-004 | Add the minimal supported Python/runtime manifest and reproducible dependency lock for root twin_core/scripts/tests. Gate: a clean environment installs from the lock and passes the owned root runner. | Reproducible development and fewer machine-specific failures now; useful without any Jarvis redesign. |
| JSR-008 | [ ] PROPOSED | — | Host-independent configuration and path resolver — BUILD | JSR-002 | Replace official hard-coded user/repository paths with a validated configuration resolver and explicit defaults. Preserve compatibility shims for current paths. Gate: tests run under a temporary home/root and the current Mac path still works. | Makes current scripts movable and testable. Stopping here already reduces coupling; compatibility shims can remain indefinitely. |
| JSR-009 | [ ] PROPOSED | — | Canonical work-state root contract — BUILD | JSR-008 | Define versioned directories for work ledger, caches, checkpoints, logs and local configuration; explicitly exclude secrets and personal data. Gate: resolver creates secure permissions, rejects symlinks/escape and can report all state roots. | Consolidates fragmented .giga/.jarvis state without requiring immediate migration. Existing roots remain readable through adapters. |
| JSR-010 | [ ] PROPOSED | — | Device-secret reference contract — BUILD | JSR-008, JSR-009 | Introduce opaque secret references and a local provider interface; migrate one read-only credential lookup without moving the secret. Gate: secret values never appear in config export, logs, receipts or fixtures; missing secret degrades locally. | Reduces accidental leakage today. Other credentials may continue on the legacy path until separately migrated. |
| JSR-011 | [ ] PROPOSED | — | Content-addressed runtime bundle — BUILD | JSR-007, JSR-008 | Build a deterministic manifest of code, schemas, defaults, runtime/lock identity and verification command; exclude mutable state, secrets, vault and generated noise. Gate: two builds from identical inputs have identical logical manifests. | Creates a transferable, inspectable work artifact even before signed releases or autonomy. |
| JSR-012 | [ ] PROPOSED | — | Work-state export and import — BUILD | JSR-009, JSR-022 | Export a quiescent, integrity-checked state package with schema/version/head metadata; import only into an empty or explicitly reconciled destination. Gate: round trip preserves events/receipts and rejects torn/tampered packages. | Provides backup and laptop migration for current work state. It does not require the Personal Vault or leader fencing. |
| JSR-013 | [ ] PROPOSED | — | Clean-device faceless restore — BUILD | JSR-010, JSR-011, JSR-012 | On a disposable clean account/device image, restore runtime and work state, provision a test secret locally and produce health/status without vault access. Gate: no hidden original paths, personal files or unrecorded dependencies are needed. | Achieves the first real portable faceless module. Later autonomy is optional. |
| JSR-014 | [ ] PROPOSED | — | Release identity and restore receipt — EXTEND | JSR-013, JSR-080 | Bind runtime manifest, state checkpoint, Constitution identity, schema compatibility and verification results into one release/restore receipt. Gate: mismatched runtime/state/policy refuses; previous bundle remains restorable. | Makes device transfers and rollbacks auditable. Stopping here leaves a durable manual restore system. |

### Track S — State, evidence and memory

Track rule: raw evidence is immutable; models may propose derived state, but deterministic code owns
validation, transitions and canonical writers.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-015 | [ ] PROPOSED | — | Repair current memory health — EXTEND | JSR-002 | Fix current index/provenance drift without rewriting entry meaning. Gate: all entries/index links validate and a pre-change backup verifies. | Restores trustworthy current memory immediately without coupling the repair to CI or later schema work. |
| JSR-016 | [ ] PROPOSED | — | Memory epistemic schema v2 — BUILD | JSR-015 | Add independent fields for epistemic class, confidence band, review state and evidence references; retain a compatibility projection for existing type/status entries. Gate: migrate copies deterministically; ambiguous entries remain unreviewed rather than guessed. Basis: R-05–R-06. | Makes “certain, inferred, low confidence” honest and queryable now. Old readers continue through projection. |
| JSR-017 | [ ] PROPOSED | — | Temporal precision and provenance compiler — BUILD | JSR-016, JSR-019 | Represent event time, observation time, valid interval, precision, source spans and inference method. Gate: exact, bounded and unknown times remain distinguishable; contradictory dates produce proposals, never silent overwrite. Basis: R-05. | Improves recall and prevents invented timestamp certainty, independent of Vault or routing work. |
| JSR-018 | [ ] PROPOSED | — | Authenticated literal memory-directive ledger — EXTEND | JSR-015, JSR-020 | Store “remember/forget/supersede” operator directives as append-only authenticated events separate from their derived memory projection. Gate: directives replay idempotently and cannot be silently edited/deleted by a model. Basis: R-04. | Preserves the operator's literal orders even if the memory compiler changes later. |
| JSR-019 | [ ] PROPOSED | — | Immutable raw-export evidence catalogue — EXTEND | JSR-002 | Produce a schema-backed catalogue of every sanctioned raw export, hash, owner, source, capture time, privacy class and parser version. Gate: verification is read-only and mutation/missing/unknown files fail. Basis: R-03. | Strengthens present evidence protection and supports any future memory or research system. |
| JSR-020 | [ ] PROPOSED | — | Canonical event envelope v2 — EXTEND | JSR-002 | Version the event contract with actor, source, observed/effective time, trust, sensitivity, idempotency, provenance and payload hash while preserving schema-v2 reads. Gate: current Telegram/daily events round-trip and invalid trust/time combinations refuse. | Gives every future subsystem a shared evidence language; current producers may migrate one at a time. |
| JSR-021 | [ ] PROPOSED | — | Complete action lifecycle — EXTEND | JSR-020 | Implement supported action creation and legal transitions: proposed, admitted, executing, succeeded, failed, cancelled, compensated and unknown-outcome. Gate: illegal transitions/duplicate terminal effects fail and actions remain usable without an executor. | Converts the existing unused action table into a real planning/audit primitive; manual actions can use it before autonomy. |
| JSR-022 | [ ] PROPOSED | — | Universal evidence/receipt envelope — EXTEND | JSR-020, JSR-021 | Version receipts around operation, action, policy/registry hash, inputs, affected system, external evidence, result and uncertainty. Gate: success requires the expected evidence class; legacy receipts remain readable. Basis: L-08. | Makes current and future completion claims comparable and truthful without requiring every domain adapter. |
| JSR-023 | [ ] PROPOSED | — | Deterministic replayable projections — BUILD | JSR-020, JSR-022 | Define rebuildable state projections from canonical events/receipts. Gate: an empty rebuild equals the live projection and changed projector identity invalidates stale views. | Makes operational views repairable now without requiring checkpoint or archive policy. |
| JSR-077 | [ ] PROPOSED | — | Memory health in the canonical gate — EXTEND | JSR-004, JSR-015 | Add memory validation to the canonical runner with clear hard-error versus warning policy. Gate: orphan, broken-link and malformed-frontmatter fixtures fail while allowed warnings remain visible. | Prevents repaired memory from silently regressing; the earlier repair remains useful without this automation. |
| JSR-078 | [ ] PROPOSED | — | Quiescent state checkpoint format — BUILD | JSR-023 | Define a checksummed checkpoint with event/receipt head, schema and projector identity. Gate: torn/corrupt checkpoints refuse and replay safely falls back to the last good head. | Speeds restore and audit independently of backup transport or compaction. |
| JSR-079 | [ ] PROPOSED | — | Evidence-preserving compaction and archive — BUILD | JSR-023, JSR-078 | Archive or compact only data whose projection/replay equivalence and retention policy are proven. Gate: historical receipts remain addressable and an archive round trip rebuilds the same state. | Controls long-term growth without making backup or autonomous operation a prerequisite. |

### Track P — Constitution, policy and authority

Track rule: the router may select a direction, but only deterministic policy may admit authority.
No slice in this track widens existing authority merely because enforcement code exists.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-024 | [ ] PROPOSED | — | Versioned Constitution payload — BUILD | JSR-001 | Define the machine policy root, policy precedence, schema and immutable policy identity. Gate: missing/tampered/unknown payload cannot compile or produce an official policy identity. Basis: R-17. | Establishes a stable policy reference for audits without forcing an early key-management decision. |
| JSR-025 | [ ] PROPOSED | — | Deterministic policy compiler — BUILD | JSR-024, JSR-004 | Compile human-ratified policy into closed machine decisions and explainable reason codes; reject ambiguous or unsupported clauses instead of guessing. Gate: golden cases and adversarial policy fixtures compile repeatably. | Turns doctrine into testable policy artifacts; can be used for review before it controls any caller. |
| JSR-026 | [ ] PROPOSED | — | Canonical capability registry — BUILD | JSR-006, JSR-025 | Register capability owner, handler, authority ceiling, inputs, context scopes, cost, reversibility, expected receipt and lifecycle status. Gate: duplicates, unknown owners, stale versions and competing active handlers fail. | Becomes the truthful inventory of what Jarvis could request, without granting permission to execute it. |
| JSR-027 | [ ] PROPOSED | — | Canonical operation request and kernel-decision contracts — BUILD | JSR-020, JSR-022, JSR-026 | Define versioned request/decision schemas with idempotency, target, authority, evidence, budget, reversibility and receipt expectation. Gate: unsupported/missing fields fail closed and decision replay is byte-equivalent. | Gives manual tools and future agents one inspectable admission language before enforcement begins. |
| JSR-028 | [ ] PROPOSED | — | Observe-only policy kernel — BUILD | JSR-027 | Run the deterministic kernel beside existing official paths, recording permit/proposal/replay/refuse verdicts without changing behaviour. Gate: shadow output explains all actual calls/writes/costs; mismatches stay visible. | Immediately exposes hidden authority and policy gaps. It is useful and safe even if enforcement is never enabled. |
| JSR-029 | [ ] PROPOSED | — | First enforced Telegram memory capability — EXTEND | JSR-028, JSR-030 | Route the existing transactional memory operation through request → kernel → no-tool proposal → deterministic commit → receipt while preserving its authority and schema semantics. Gate: replay makes zero provider calls; transition fault injection and rollback pass. | Delivers the first unavoidable policy path without making Jarvis more autonomous. Existing memory value remains available if all later work stops. |
| JSR-030 | [ ] PROPOSED | — | Static official-surface bypass audit — BUILD | JSR-006, JSR-026 | Detect direct provider/tool/network/subprocess/writer call sites and launch configurations not represented in the registry. Gate: seeded source/config bypass fixtures fail and every real finding is classified. | Provides immediate policy-erosion detection before runtime enforcement exists. |
| JSR-031 | [ ] PROPOSED | — | Operator-only break-glass mechanism — DEFERRED BUILD | JSR-029, demonstrated operational need | Add a separate authenticated command with exact capability, target, reason, expiry, maximum authority and immutable receipt. It cannot claim compliance or suppress bypass findings. Gate: expired/replayed/model-issued requests fail. | Provides controlled emergency recovery only if real experience proves it necessary. Safe stopping point before this slice is deliberately “no break glass.” |
| JSR-032 | [ ] PROPOSED | — | Policy update, canary and rollback lifecycle — BUILD | JSR-025, JSR-028 | Support propose → compile → negative-test → shadow → owner-ratify → canary → activate → rollback, binding every decision to policy identity. Gate: bad policy cannot strand manual operation; prior version restores in one command. | Makes policy maintainable over years without letting Jarvis rewrite its judge. |
| JSR-080 | [ ] PROPOSED | — | Owner signing and recovery mechanism — BUILD | JSR-024, operator signing/recovery decision | Bind policy/release identities to an owner-controlled key and offline recovery procedure; no model-accessible signer exists. Gate: wrong/rotated/recovered keys behave according to ratified policy and recovery cannot silently widen authority. | Adds provenance and recovery after the policy format is useful on its own. |
| JSR-081 | [ ] PROPOSED | — | Runtime bypass telemetry and negative controls — BUILD | JSR-028, JSR-029 | Instrument official execution so an admitted request/decision precedes privileged effects; add runtime fixtures that attempt direct effects. Gate: seeded bypasses are blocked or produce a hard incident and unclassified runtime effects cannot be receipted as compliant. | Detects dynamic/reflection/config paths a static audit cannot see; static protection remains valuable alone. |

### Track C — Context, compartments and Personal Vault

Track rule: information access and action authority are orthogonal. Mounting the Personal Vault may
increase available context but cannot silently widen capability permission.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-033 | [ ] PROPOSED | — | Faceless context-broker API — BUILD | JSR-009, JSR-020 | Wrap the current bounded context compiler behind a versioned request/packet interface with source, purpose, size and sensitivity metadata. Gate: existing Telegram retrieval remains equivalent and path escape/symlink/oversize requests fail. | Improves current context observability and creates a stable seam without any Vault dependency. |
| JSR-034 | [ ] PROPOSED | — | Operational compartment registry — BUILD | JSR-026, JSR-033 | Register per-source owner, retention, sensitivity, query methods and allowed context purposes for email, calendar, messages and future connectors. Gate: cross-compartment access is explicit and unknown sources refuse. Basis: R-16. | Gives current/future external data sources enforceable separation before any connector is added. |
| JSR-035 | [ ] PROPOSED | — | Read-only email compartment — BUILD | JSR-010, JSR-027, JSR-034 | Ingest/query bounded email metadata/content through a read-only adapter with source IDs, injection labelling and retention rules. Gate: no send/delete/label authority; malicious content cannot alter routing/policy; connector failure quarantines locally. | Provides useful faceless work context and daily counsel even if the Vault and autonomy are never built. |
| JSR-036 | [ ] PROPOSED | — | Read-only calendar compartment — BUILD | JSR-010, JSR-027, JSR-034 | Query events/availability through a read-only adapter with timezone, freshness and provenance. Gate: no event mutation; stale/missing credentials report unknown rather than empty; injected descriptions remain data. | Provides immediate schedule awareness in manual or faceless mode without granting calendar writes. |
| JSR-037 | [ ] PROPOSED | — | Personal Vault format and encryption boundary — BUILD | JSR-001, JSR-016, JSR-019 | Define a separately encrypted local package for raw personal evidence, directives, memory, PPP, profiler, evaluations and twin artifacts; explicitly exclude operational work state. Gate: locked Vault reveals no content/index; format/version/integrity validate offline. Basis: R-02, R-15. | Creates a real privacy and ownership boundary. It remains useful as secure storage without Jarvis mounting it. |
| JSR-038 | [ ] PROPOSED | — | Explicit Vault mount session — BUILD | JSR-037 | Implement local unlock, mount, lock and unmount with exact Vault version and bounded session identity. Gate: faceless operation continues while locked and a crashed/stale session cannot leave a silently trusted mount. | Delivers the physical ON/OFF boundary without yet releasing data to models. |
| JSR-039 | [ ] PROPOSED | — | Vault access ledger and provenance receipts — BUILD | JSR-022, JSR-082 | Record what scope/version/purpose was released to which route/model role without logging private plaintext. Gate: every Vault-derived context packet has a grant and audit record; direct file reads from official paths fail. | Makes accepted privacy risk inspectable and supports incident review without pretending Vault use is risk-free. |
| JSR-040 | [ ] PROPOSED | — | Canonical personal-memory writer — BUILD | JSR-017, JSR-018, JSR-038 | Establish one deterministic compiler as the only official writer of personal memory; accept evidence-linked proposals while preserving directives/provenance. Gate: competing writers fail and rejection cannot mutate canon. | Prevents competing memory truth without making PPP implementation a hidden dependency. |
| JSR-041 | [ ] PROPOSED | — | Encrypted Vault backup and clean restore — BUILD | JSR-037, operator decision on off-device backup | Implement client-side encrypted cold backup with owner-held recovery material, or record an explicit no-backup decision and local recovery procedure. Gate: disposable restore verifies exact versions and wrong/missing keys fail without data leakage. | Protects personal history independently of Jarvis. No cloud runtime or external action authority is included. |
| JSR-082 | [ ] PROPOSED | — | Purpose-bound ContextGrant — BUILD | JSR-025, JSR-033, JSR-038 | Issue short-lived grants naming Vault scope, purpose, consumer, versions and expiry; data access and action authority remain separate. Gate: revoked/expired/wrong-purpose grants fail and mounting alone releases no context. | Delivers controlled Vault use while the mount mechanism remains independently useful. |
| JSR-083 | [ ] PROPOSED | — | Canonical PPP writer and proposal contract — BUILD | JSR-040 | Define PPP schema, evidence-linked delta proposal and one deterministic canonical writer separate from domain profilers. Gate: competing writers and unsupported claims fail; accepted deltas retain their source receipt. | Creates stable personal-profiler truth before Market Aligner or other producers connect. |

### Track R — Stimulus routing and cognition

Track rule: models may classify, reason, draft and criticise inside deterministic envelopes. They
never select their own authority, context grant, official writer or success state.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-042 | [ ] PROPOSED | — | Canonical stimulus envelope — BUILD | JSR-020 | Normalize human messages, timers, webhooks, domain events, health signals and self-initiated candidates into typed stimuli with source/trust/urgency/idempotency. Gate: current Telegram and daily triggers round-trip; untrusted content cannot set authority fields. | Produces a useful, queryable inbox of work even before automatic routing. |
| JSR-043 | [ ] PROPOSED | — | RoutePlan schema and trace — BUILD | JSR-027, JSR-042 | Define route ID, intent, domain, goals, why-now, context scopes, capabilities, model roles, requested authority, urgency, budget, fallback, router version and Constitution hash. Gate: schema rejects route-as-permission and missing provenance. Basis: R-18. | Gives humans and diagnostics a consistent routing explanation without executing anything. |
| JSR-044 | [ ] PROPOSED | — | Universal LLM invocation envelope — EXTEND | JSR-022, JSR-027 | Record provider/model, prompt/contract/context/input hashes, adapters/datasets, output schema/hash, tokens/cost/latency, fallback and acceptance status. Gate: current Claude/Codex memory providers use it without gaining tools. | Makes model behavior, cost and reproducibility inspectable across present workloads. |
| JSR-045 | [ ] PROPOSED | — | Deterministic rules router — BUILD | JSR-026, JSR-043 | Route known exact commands, receipt replays, health events and registered domain events without an LLM. Gate: deterministic fixtures never call a provider and unknown cases stay unclassified. | Reduces latency/cost and gives immediate reliable routing for simple work. |
| JSR-046 | [ ] PROPOSED | — | Model-assisted classifier in shadow mode — BUILD | JSR-044, JSR-045 | Classify only ambiguous stimuli into typed RoutePlan proposals; compare against human/rule labels without enforcement. Gate: locked set reports accuracy, abstention, calibration, cost and dangerous-scope errors. | Supplies actionable routing data and a baseline even if model routing is never promoted. |
| JSR-047 | [ ] PROPOSED | — | Model-role registry — BUILD | JSR-026, JSR-044 | Register classifier, planner, reasoner, critic, extractor, identity renderer and judge roles with allowed inputs, outputs and authority ceiling. Gate: unknown/duplicate roles and direct tool authority fail. | Prevents one generic model call from becoming an implicit architecture. |
| JSR-048 | [ ] PROPOSED | — | Planner proposal role — BUILD | JSR-043, JSR-047 | Produce typed decision/action proposals under a fixed input contract; it cannot commit state or approve itself. Gate: locked planning fixtures preserve constraints and output schema. | Improves manual planning before critic, initiative or autonomous execution exists. |
| JSR-049 | [ ] PROPOSED | — | Provider and privacy policy registry — EXTEND | JSR-044, JSR-047 | Centralize allowed providers/models by role, local/cloud privacy class and context sensitivity. Gate: unsupported provider/privacy combinations refuse rather than silently fall back. | Hardens current provider selection without bundling budgets or outage handling. |
| JSR-050 | [ ] PROPOSED | — | Twin-pack resolver and production quarantine — BUILD | JSR-082, JSR-084 | Resolve exact versions of memory/profile/preference/voice/reasoning artifacts for a Twin session; production accepts only promoted artifacts. Gate: faceless mode uses none; unpromoted/unknown/mismatched candidates cannot load. | Makes retrieval-personalized Twin Mode reproducible while keeping LoRA research optional and safely quarantined. |
| JSR-084 | [ ] PROPOSED | — | Least-context compiler per model role — BUILD | JSR-033, JSR-044, JSR-047 | Compile only declared context scopes and evidence needed by one role, with purpose/version hashes. Gate: roles cannot inherit caller filesystem access or undeclared compartments. | Reduces privacy leakage and prompt bloat independently of planners or fallback providers. |
| JSR-085 | [ ] PROPOSED | — | Independent critic proposal role — BUILD | JSR-047, JSR-048 | Critique a bound proposal against explicit constraints without rewriting it or granting authority. Gate: disagreement and unresolved findings are retained; planner and critic identities are distinct. | Adds useful manual scrutiny while the planner remains independently usable. |
| JSR-086 | [ ] PROPOSED | — | Model cost, token and latency budgets — BUILD | JSR-044, JSR-049 | Enforce per-role/request ceilings and record actual use. Gate: over-budget requests refuse/degrade deterministically and budget accounting survives fallback. | Controls present model spend even without automated initiative. |
| JSR-087 | [ ] PROPOSED | — | Provider fallback and circuit breakers — EXTEND | JSR-049, JSR-086 | Select only policy-compatible fallback providers and quarantine repeated failure by provider/capability. Gate: no fallback weakens privacy/output/authority requirements; unrelated providers continue. | Improves current availability without requiring an extinction exercise. |
| JSR-088 | [ ] PROPOSED | — | Provider-extinction drill — BUILD | JSR-087 | Remove the primary provider in a disposable drill and prove allowed fallback or truthful degraded operation for each official role. Gate: no hidden primary dependency or silent policy downgrade remains. | Provides durable provider-independence evidence; fallback logic remains useful without periodic drills. |

### Track A — Agency and multi-year continuity

Track rule: initiative is promoted one reversible capability at a time. Long-running does not mean
unbounded; every action retains goal, why-now, authority, cost, reversibility and outcome evidence.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-051 | [ ] PROPOSED | — | Goal registry lifecycle — EXTEND | JSR-020 | Complete supported creation, priority, pause, completion and supersession for goals, separating operator-ratified goals from inferred proposals. Gate: a model cannot ratify/create authority and historical goal changes remain visible. | Gives manual and advisory work durable objectives without requiring plans, tasks or initiative. |
| JSR-052 | [ ] PROPOSED | — | Initiative candidate generator in observe-only mode — BUILD | JSR-042, JSR-048, JSR-051 | Convert due goals, domain events, anomalies and opportunities into non-executing candidate actions. Gate: every candidate links evidence and goal; recursive self-generated claims alone cannot trigger new work. Basis: R-11. | Produces a useful suggestion/inbox surface without granting autonomous execution. |
| JSR-053 | [ ] PROPOSED | — | Deterministic why-now/value/risk scorer — BUILD | JSR-022, JSR-052 | Score evidence freshness, urgency, expected value, cost, reversibility, confidence and opportunity cost with explicit unknowns. Gate: missing goal/evidence/receipt expectation blocks promotion; locked examples are repeatable. | Prioritizes manual and suggested work transparently even if the executor is never enabled. |
| JSR-054 | [ ] PROPOSED | — | One canonical scheduler owner — BUILD | JSR-042, JSR-051, JSR-053 | Inventory and reconcile launchd, Telegram-clock and shell scheduling; register one owner per recurring job while retaining manual invocation. Gate: duplicate trigger fixtures create one event; legacy schedules are disabled only after rollback proof. | Stops duplicated or silently missed daily work now. It remains useful in reactive mode. |
| JSR-055 | [ ] PROPOSED | — | Explicit Manual, observe-only and stopped modes — EXTEND | JSR-024, JSR-027 | Make runtime mode a policy-bound state independent of Vault mounting; preserve domain manual paths. Gate: modes cannot be inferred from model text and stopped/Manual states cannot emit autonomous actions. Basis: R-22. | Makes present control state truthful without waiting for a remote STOP channel. |
| JSR-056 | [ ] PROPOSED | — | First reversible autonomous capability — BUILD | JSR-029, JSR-051, JSR-053, JSR-091 | Promote one low-cost action from candidate to kernel-admitted execution with an external receipt and compensation path. Default: a bounded work-state operation, not public representation. Gate: useful real canary, zero unrelated scope, crash/retry and rollback pass. | Creates genuine but narrow initiative. If all future work stops, this one capability remains independently governed. |
| JSR-057 | [ ] PROPOSED | — | OS process supervisor — EXTEND | JSR-004, JSR-009 | Replace the bare restart loop with supervised startup, bounded restart/backoff, restart-storm protection and STOP precedence. Gate: crash/restart fixtures behave predictably and manual shutdown always wins. | Improves current Jarvis process reliability without bundling host sensors or new authority. |
| JSR-058 | [ ] PROPOSED | — | Credential lifecycle and local failure isolation — BUILD | JSR-010, JSR-022, JSR-057 | Track opaque credential health, expiry/renewal-needed state and connector-specific circuit breakers without exposing secrets. Gate: one expired credential quarantines one capability; unrelated offline work continues. | Reduces silent connector failure today and supports the six-year objective without bypassing MFA. |
| JSR-059 | [ ] PROPOSED | — | Owner-independent external heartbeat and alert — BUILD | JSR-057, JSR-092 | Deploy a minimal monitor with no Vault or action authority that detects missed heartbeat and reports through a separately owned channel. Gate: process hang, laptop offline and monitor failure are distinguishable; no personal payload leaves the host. | Makes silence distinguishable from failure even if no standby or autonomous work is added. |
| JSR-060 | [ ] PROPOSED | — | Encrypted work-state backup transport and retention — BUILD | JSR-014, JSR-078 | Copy only quiescent/checksummed state packages through an encrypted backup path with retention; never synchronize a live database blindly. Gate: latest and prior backups verify, tamper is detected and secrets/Vault are absent. | Protects current portable work state independently of automated restore drills. |
| JSR-061 | [ ] PROPOSED | — | Renewable fenced leader lease — BUILD | JSR-014, JSR-022, JSR-060 | Permit external side effects only under one renewable fencing token; stale holders cannot commit receipts as leader. Gate: dual-device race and stale-token fixtures yield one effect owner. Basis: R-13. | Prevents split-brain effects even before a polished device-handoff workflow exists. |
| JSR-062 | [ ] PROPOSED | — | Self-update candidate, canary and atomic rollback — BUILD | JSR-032, JSR-057, JSR-088, JSR-093 | Build candidate → isolated proof → canary → activate → one-command rollback while preserving the previous runtime and judge. Gate: bad update never loses the last verified release or edits acceptance policy. | Makes upgrades safe without bundling months-long endurance promotion. |
| JSR-089 | [ ] PROPOSED | — | Plan lifecycle — BUILD | JSR-051 | Add versioned plans linked to goals with assumptions, strategy, status and supersession. Gate: a plan can be paused/replaced without altering the ratified goal and model plans remain proposals. | Gives manual work durable strategy before task machinery exists. |
| JSR-090 | [ ] PROPOSED | — | Task and dependency lifecycle — BUILD | JSR-021, JSR-089 | Add tasks with owners, dependencies, readiness, retries and outcomes. Gate: dependency cycles/unknown owners fail and task completion cannot imply goal completion without evidence. | Provides useful manual execution tracking before initiative or scheduling. |
| JSR-091 | [ ] PROPOSED | — | Authenticated remote STOP — BUILD | JSR-055, JSR-057 | Add a Vault-independent, policy-bound stop command that every official worker must honour. Gate: bounded stop latency, replay protection and operator-only restart pass; STOP cannot be downgraded by a model. | Provides immediate remote recovery while runtime modes remain useful on their own. |
| JSR-092 | [ ] PROPOSED | — | Host-health envelope — BUILD | JSR-009 | Report power, disk, clock, network, thermal and process signals with freshness and unknown states. Gate: disk-full, clock-skew, missing sensor and network-loss fixtures degrade truthfully. | Improves diagnostics for current/manual operation without requiring the supervisor or external monitor. |
| JSR-093 | [ ] PROPOSED | — | Disposable restore and corruption drills — BUILD | JSR-060 | Restore latest and previous encrypted work-state backups into disposable roots and verify replay/health. Gate: corrupt/torn/wrong-key backups fail safely and the documented recovery path is repeatable. | Proves backups are usable without requiring leader handoff or autonomous operation. |
| JSR-094 | [ ] PROPOSED | — | Fenced device-handoff workflow — BUILD | JSR-061, JSR-093 | Implement quiesce → publish head → release → restore/verify → acquire → resume, with manual/read-only fallback. Gate: interrupted handoff and old-device reappearance cannot create duplicate effects. Basis: R-01. | Makes laptop transfer operational while the underlying lease remains independently protective. |
| JSR-095 | [ ] PROPOSED | — | Longevity fault-drill ladder — BUILD | JSR-059, JSR-062, JSR-093, JSR-094 | Define and run increasing 24h, 7d, 30d, 90d and annual drills for provider, credential, network, disk, snapshot, policy and bad-update failures. Gate: each horizon has explicit pass evidence and no authority violation. | Accumulates evidence toward six-year operation; every underlying durability feature remains useful if longer horizons stop. |

### Track D — Domain execution and orchestration

Track rule: each domain remains the canonical writer of its own internal truth. Jarvis exchanges
typed requests, events, proposals, certificates and receipts; it does not co-write domain databases.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-063 | [ ] PROPOSED | — | Domain-adapter SDK and conformance harness — BUILD | JSR-026, JSR-027, JSR-022 | Define read, propose, execute, cancel, reconcile, health and receipt interfaces plus version negotiation and no-authority defaults. Gate: fake adapter passes success/failure/timeout/replay/quarantine fixtures; unregistered methods refuse. | Gives every product a reusable integration seam without requiring any real domain to migrate. |
| JSR-064 | [ ] PROPOSED | — | Orchestrator read-only status adapter — BUILD | JSR-063, canonical Orchestrator checkout/contract | Consume bounded status, candidate identity and certificate metadata without submitting work. Gate: stale/malformed/self-reported success stays unknown; Orchestrator's internal Overseer remains opaque. | Improves operator visibility and validates the seam before granting Jarvis software-work authority. |
| JSR-065 | [ ] PROPOSED | — | Orchestrator bounded work-order adapter — BUILD | JSR-028, JSR-055, JSR-064 | Submit one scope/cost/time-bound work order and consume a candidate-bound Overseer certificate; support cancel/fail/rollback reconciliation. Gate: uncoupled completion and certificate replay against changed candidate fail. Basis: R-12, R-19. | Lets Jarvis delegate one governed software task. Other Orchestrator use remains manual and independent. |
| JSR-066 | [ ] PROPOSED | — | Market Aligner career-event feed — BUILD | JSR-063, Market Aligner canonical product gate | Export source-hashed opportunity, analysis, application and outcome events without exposing or sharing internal databases. Gate: one real read-only event round-trips; duplicates replay; missing provenance refuses. | Makes Market Aligner insight visible to GIGA while the product continues operating independently. |
| JSR-067 | [ ] PROPOSED | — | Market Aligner career-profiler proposal feed — BUILD | JSR-066 | Emit evidence-linked career-profiler deltas while Market Aligner remains its canonical writer. Gate: deltas bind opportunity evidence and rejected proposals cannot mutate either system. Basis: R-20. | Makes market learning portable without bundling application outcomes or personal PPP changes. |
| JSR-068 | [ ] PROPOSED | — | Dubbing Studio read-only status adapter — BUILD | JSR-063, ratified Dubbing Studio owner/contract | Consume bounded job/status/receipt metadata without submitting work. Gate: missing canonical owner or stale/self-reported completion remains unknown. | Adds useful visibility and validates a second adapter shape before execution authority. |
| JSR-069 | [ ] PROPOSED | — | Per-domain circuit breaker and quarantine — BUILD | Two verified JSR-063 adapters, JSR-058 | Isolate failing/stale adapters by domain/capability while unrelated domains continue. Gate: one broken adapter cannot halt or contaminate another and quarantine state is visible. | Provides local degradation independently of unknown-outcome reconciliation or adapter retirement. |
| JSR-096 | [ ] PROPOSED | — | JAA application-outcome receipts — BUILD | JSR-066 | Export application attempts, downstream acceptance/rejection and later outcomes as evidence-linked receipts. Gate: process exit or send claim alone cannot count as application completion. | Closes career feedback loops without requiring personal-profile mutation. |
| JSR-097 | [ ] PROPOSED | — | Market-to-PPP and personal-memory proposal loop — BUILD | JSR-083, JSR-067, JSR-096 | Emit evidence-linked PPP/personal-memory deltas; only their canonical writers commit. Gate: one real lifecycle closes with receipts and no competing biography writer. Basis: R-20. | Creates durable career learning after the independent profiler and outcome feeds already provide value. |
| JSR-098 | [ ] PROPOSED | — | Dubbing Studio bounded execution adapter — BUILD | JSR-028, JSR-068, JSR-091 | Submit one reversible job and reconcile its domain receipt/cancellation. Gate: Jarvis cannot infer success from process exit and rollback/cancel outcomes remain explicit. | Adds one governed studio action while read-only visibility remains independently useful. |
| JSR-099 | [ ] PROPOSED | — | Unknown external-outcome reconciliation — BUILD | JSR-022, JSR-063 | Standardize query/reconcile/compensate for timeouts where an external effect may or may not have happened. Gate: uncertainty never becomes automatic retry/success and duplicate-effect fixtures pass. | Prevents dangerous retries for any adapter without requiring adapter removal support. |
| JSR-100 | [ ] PROPOSED | — | Adapter version withdrawal and extinction drill — BUILD | JSR-069, JSR-099 | Withdraw/replace one adapter version while historical receipts remain readable and the domain degrades or moves to a compatible version. Gate: hidden dependencies and stale handler claims fail. | Makes adapters replaceable over years; quarantine/reconciliation keep their standalone value. |

### Track I — Identity representation and learning

Track rule: behavioural fidelity is not factual or action authority. No identity artifact reaches a
live channel until channel policy, consent/disclosure constraints, baseline evaluation and rollback
all pass.

| ID | Checklist | Evidence / work order | Slice and mode | Depends on | Deliverable and objective gate | Immediate standalone value and safe stopping point |
|---|---|---|---|---|---|---|
| JSR-070 | [ ] PROPOSED | — | Representation capability and channel policy — BUILD | JSR-024, JSR-026, operator channel-scope decision | Separate draft, private-send, public-send, voice/audio and account-control capabilities with disclosure/terms, audience, approval and retention policy. Gate: “write as Artiom” cannot imply send/account authority. Basis: R-14. | Safely improves drafting and makes future representation scope explicit without impersonating anyone online. |
| JSR-071 | [ ] PROPOSED | — | Authorised Artiom evaluation corpus — EXTEND | JSR-016, JSR-037, operator data authorisation | Create privacy-reviewed relationship/time splits for voice, preference and decision evidence; quarantine third-party/credential leakage and lock test data away from training. Gate: hashes, consent scope and exclusions are fixed. | Produces a durable comparison corpus without requiring a mode ontology or model training. |
| JSR-072 | [ ] PROPOSED | — | Private Artiom Test harness and baselines — BUILD | JSR-050, JSR-071, JSR-101 | Measure voice, memory, decision consistency, initiative judgment, relationship boundaries, longitudinal drift and policy compliance against base+retrieval baselines. Gate: blind/private scoring, abstention and failure thresholds are ratified before candidate evaluation. | Turns the “Artiom Test” into a repeatable research and quality tool without granting external representation. |
| JSR-073 | [ ] PROPOSED | — | Offline shadow representation — BUILD | JSR-049, JSR-070, JSR-072, JSR-084 | Generate candidate replies beside real/manual behavior without sending or controlling an account. Gate: outputs are captured for blind evaluation and cannot escape the offline sink. | Provides safe fidelity evidence without any live-channel authority. |
| JSR-074 | [ ] PROPOSED | — | Behavioural research and promotion harness — RESEARCH | JSR-071, JSR-072, JSR-101 | Standardize training manifests, privacy checks, locked evaluation, leakage/mode-collapse tests, artifact registry and rollback; base+retrieval remains the comparator. Gate: failed and successful candidates are retained truthfully. Basis: R-21. | Makes future experiments reproducible without assuming any LoRA dimension will succeed. |
| JSR-075 | [ ] PROPOSED | — | Autonomous utility and repair-cost benchmark — BUILD | JSR-056, JSR-108 | Compare one autonomous capability with reactive/manual baseline on useful outcomes, intervention time, repair cost, latency and authority violations. Gate: thresholds are operator-ratified and raw outcomes remain inspectable. | Measures whether autonomy is worthwhile without bundling multi-domain or identity promotion. |
| JSR-101 | [ ] PROPOSED | — | Artiom behavior-mode ontology and mapping — BUILD | JSR-071 | Define evidence-backed modes and label confidence/reviewer status separately from dataset splits. Gate: ambiguous examples remain unassigned and mode changes preserve earlier labels/version history. | Makes mode-specific evaluation possible while the underlying corpus remains independently reusable. |
| JSR-102 | [ ] PROPOSED | — | One consenting private-channel representation capability — BUILD | JSR-032, JSR-070, JSR-073, JSR-091 | Permit generation/send on one explicitly scoped private channel with audience, disclosure/terms, retention and rollback policy. Gate: offline baseline wins, policy canaries pass and account/tool scope cannot expand. | Provides bounded real-world twin value while public channels remain unavailable. |
| JSR-103 | [ ] PROPOSED | — | Speech/voice candidate research — RESEARCH | JSR-074 | Train/evaluate only speech/style artifacts against voice fidelity, mode control, leakage and policy baselines. Gate: the candidate is promoted or rejected on voice dimensions only. | Produces useful voice evidence without coupling it to preferences or reasoning. |
| JSR-104 | [ ] PROPOSED | — | Preference candidate research — RESEARCH | JSR-074 | Train/evaluate preference prediction or retrieval artifacts on held-out choices and abstention. Gate: factual memory and action authority remain separate; candidate promotion is dimension-specific. | Improves preference modelling even if voice and reasoning candidates fail. |
| JSR-105 | [ ] PROPOSED | — | Reasoning-pattern candidate research — RESEARCH | JSR-074 | Evaluate reasoning-steering artifacts on held-out constraint/decision suites, calibration and policy; do not treat chain-of-thought imitation as proof. Gate: failed clone results remain retained and production stays unchanged. | Advances the hardest twin dimension without blocking other twin value. |
| JSR-106 | [ ] PROPOSED | — | Multi-capability autonomous expansion — BUILD | JSR-069, JSR-075, JSR-099 | Add one capability at a time under its own policy, receipt, utility and rollback gate; no aggregate score overrides a failed capability. Gate: two capabilities coexist with local degradation and bounded budgets. | Expands useful autonomy while the original capability remains independently supportable. |
| JSR-107 | [ ] PROPOSED | — | Additional identity/channel promotion — BUILD | JSR-070, JSR-102, JSR-108 | Promote each new private/public/voice/account capability separately under channel-specific policy and evaluation. Gate: no channel inherits authority from another and rollback revokes only the promoted scope. | Grows digital representation without making broad impersonation a single irreversible switch. |
| JSR-108 | [ ] PROPOSED | — | Sustained canary-horizon promotion — BUILD | JSR-095, JSR-056 | Run 24h → 7d → 30d → 90d promotion for a named capability with utility, repair cost, drift and authority evidence. Gate: each horizon is independently approved; failure returns to the last proven horizon. | Supplies endurance evidence for one capability while avoiding a fictional “system complete” moment. |

## 13. Readiness waves

Waves are selection aids, not mandatory phases. A later-wave slice may begin whenever its explicit
dependencies are verified; completing a wave is never required for earlier value.

### Wave 0 — Safe today under current authority

- JSR-002 current-state baseline
- JSR-003 slice registry
- JSR-004 local verification runner
- JSR-006 official-surface inventory
- JSR-007 runtime manifest/lock
- JSR-008 configuration/path resolver
- JSR-015 current memory repair
- JSR-019 export catalogue

### Wave 1 — Contract foundations

- JSR-009–JSR-012 portable state foundations
- JSR-016–JSR-023 and JSR-077–JSR-079 memory/state/evidence contracts
- JSR-024–JSR-028 and JSR-080 Constitution/signing/kernel contracts and shadow
- JSR-033–JSR-034 faceless context/compartment contracts
- JSR-042–JSR-045, JSR-047, JSR-049 and JSR-084–JSR-086 stimulus, route and
  bounded-model contracts
- JSR-051, JSR-089 and JSR-090 goal/plan/task state

### Wave 2 — First enforced vertical systems

- JSR-013–JSR-014 clean restore and release identity
- JSR-029–JSR-030 and JSR-081 first enforced path and bypass evidence
- JSR-035–JSR-036 read-only email/calendar
- JSR-046, JSR-048, JSR-085 and JSR-087–JSR-088 routing/cognition controls
- JSR-054–JSR-055 and JSR-091 scheduler ownership, modes and remote STOP
- JSR-057–JSR-060 and JSR-092–JSR-093 supervision, host/credential health,
  heartbeat, backup and restore
- JSR-063–JSR-064 adapter SDK and read-only Orchestrator status
- JSR-076 hosted CI only if an authoritative remote is available

### Wave 3 — Vault, initiative and domain action

- JSR-037–JSR-041 and JSR-082–JSR-083 Personal Vault, grants and canonical
  personal writers
- JSR-050 Twin-pack resolver
- JSR-052–JSR-056 initiative and first reversible action
- JSR-061–JSR-062 and JSR-094–JSR-095 fencing, handoff, updates and longevity drills
- JSR-065–JSR-069 and JSR-096–JSR-100 domain execution, outcomes and quarantine
- JSR-070–JSR-072 and JSR-101 identity policy, evaluation corpus/modes and private baselines

### Wave 4 — Optional high-autonomy research and promotion

- JSR-073–JSR-075 offline shadow, behavioural harness and utility benchmark
- JSR-102 one consenting private channel
- JSR-103–JSR-105 separate voice, preference and reasoning research
- JSR-106 multi-capability expansion
- JSR-107 additional channel-by-channel identity promotion
- JSR-108 sustained per-capability canary horizons

## 14. Dependency and selection rules

1. Numeric IDs are stable opaque references, not a mandatory order or implied total.
2. Explicit dependencies are exhaustive for catalogue planning. Implementation discovery may add
   a prerequisite, but may never silently remove one.
3. If a slice contains two independently useful outcomes, assign new permanent IDs and supersede
   the compound wording. Lettered work units may organise implementation, but are not independently
   signable catalogue slices.
4. Prefer slices with immediate operator utility over architectural neatness.
5. Prefer adopting/extending proven code to parallel reimplementation.
6. A read-only adapter precedes an executing adapter.
7. Shadow precedes enforcement; canary precedes broad promotion.
8. New code fails closed once its registry/kernel prerequisites exist. Legacy paths remain named
   legacy-observed until individually migrated.
9. Never dual-write canonical truth. Migrations use proposals, replay or single-writer handoff.
10. A later schema version must retain an explicit reader/migrator for historical receipts.

## 15. Future-proofing invariants

These rules are intended to keep completed slices useful even when models, vendors, devices and
product boundaries change:

- **Contracts before implementations:** stable versioned envelopes sit between components.
- **Additive evolution:** add fields/states with defaults; remove only after usage proof and a
  migration receipt.
- **Content identity:** policy, releases, context packets, model inputs, artifacts and receipts bind
  hashes/versions rather than mutable names alone.
- **Provider neutrality:** provider-specific behavior stays behind adapters; policy states
  capability/privacy requirements, not brand assumptions.
- **One writer per truth domain:** integrations exchange events/proposals/receipts.
- **Event source plus rebuildable projections:** caches and views may be discarded and rebuilt.
- **Feature isolation:** new behavior begins off, shadow or canary and has a bounded rollback.
- **No hidden machine state:** clean restore must reveal undeclared dependencies.
- **No success by assertion:** external/system evidence closes work.
- **No policy by prompt:** prompts can explain policy, never implement its only enforcement.
- **No model-owned judge:** policies, locked evaluations and thresholds require owner-controlled
  change.
- **Manual survivability:** stopping Jarvis does not make the work or domain systems inaccessible.

## 16. Migration-hazard review

Every slice work order must record the H1–H8 review:

| Hazard | GIGA-specific application |
|---|---|
| H1 incomplete quarantine | Search all root, deployment, staged and duplicate product manifests/call sites before removing a scheduler, provider path or state root. |
| H2 mechanical major change | Treat Python/schema/provider/OS major changes as separately measured migrations; never hide them inside a feature slice. |
| H3 runtime/deployment drift | Move root lock, local launcher, launchd/systemd and clean-restore runtime pins together when a runtime changes. |
| H4 route-class gaps | Enumerate human, scheduled, webhook, health, self-initiated, replay and domain-event stimuli for router/policy work. |
| H5 stateful-store upgrade | Use SQLite backup plus explicit migration/replay; never replace a live database or cloud-sync it blindly. |
| H6 transitional weak state | Register every legacy-observed bypass or temporary permissive mode with scope, risk and closing slice. |
| H7 stacked work | Merge/reconcile one slice to the real trunk before starting a dependent slice; preserve unrelated dirty work. |
| H8 living-doc drift | Update this blueprint, architecture index, command inventory, registries and affected runbooks in the same slice. |

## 17. Residual-risk register

| Risk | Present state | Closing slice(s) | Safe behavior until closed |
|---|---|---|---|
| Proposal 11 is unratified | Open | JSR-001 | Implement only current-canon-compatible safety/portability slices. |
| Root environment is not reproducibly pinned | Open | JSR-007 | Use the current known machine; do not claim clean restore. |
| Personal data is not separated into a Vault | Open | JSR-037–JSR-041, JSR-082–JSR-083 | Treat repository memory/corpus as sensitive local data; do not claim faceless/twin isolation. |
| Official paths bypass a common kernel | Open | JSR-028–JSR-030, JSR-081 | Existing safe paths remain convention-governed; do not claim unavoidable Constitution. |
| Action lifecycle has no executor | Open | JSR-021, JSR-056 | Record events/receipts only; no autonomous-action claim. |
| Current memory index/provenance is unhealthy | Open operational issue | JSR-015, JSR-077 | Report validation findings truthfully; do not equate passing unit tests with healthy memory. |
| Jarvis runtime is stopped/stale | Open operational issue | JSR-054, JSR-057, JSR-092 | Report live state truthfully; unit-test success is not uptime. |
| No leader fencing | Open | JSR-061, JSR-094 | Run only one manually selected external-effect instance; device transfer stays quiescent/manual. |
| Orchestrator canonical checkout unknown | Open | JSR-064 prerequisite | Keep integration read-only/unimplemented; do not guess contracts. |
| Identity representation scope undecided | Open | JSR-070 | Draft-only/manual use; no autonomous public representation. |
| LoRA candidates unpromoted | Deliberate | JSR-071–JSR-074, JSR-101, JSR-103–JSR-105 | Use base+retrieval; production resolver rejects research artifacts. |

## 18. End-state coverage matrix

This table maps the current open-ended catalogue to every acceptance criterion in proposal 11:

| Proposed end-state criterion | Primary proving slices |
|---|---|
| Portable clean restore without Vault/hidden paths | JSR-007–JSR-014 |
| One fenced side-effect leader | JSR-060–JSR-061, JSR-093–JSR-094 |
| Faceless email/calendar operation | JSR-033–JSR-036 |
| Validated Twin/Vault versions | JSR-037–JSR-041, JSR-050, JSR-082–JSR-084 |
| Constitution bound to every official artifact | JSR-024–JSR-032, JSR-080–JSR-081 |
| No official provider/tool/writer bypass | JSR-006, JSR-030, JSR-081 |
| Human/event/self-initiated typed routing | JSR-042–JSR-046, JSR-052 |
| LLM proposal-only boundary | JSR-029, JSR-044, JSR-047–JSR-049, JSR-084–JSR-088 |
| Orchestrator internal Overseer/certificates | JSR-064–JSR-065 |
| Market Aligner/JAA outcome and PPP loop | JSR-066–JSR-067, JSR-083, JSR-096–JSR-097 |
| Goal/why-now/budget/authority/reversibility/outcome | JSR-021–JSR-022, JSR-051–JSR-056, JSR-086, JSR-089–JSR-091 |
| Crash/retry/replay without duplicate effect | JSR-020–JSR-023, JSR-029, JSR-056, JSR-078, JSR-099 |
| Local failure quarantine | JSR-058, JSR-069, JSR-087, JSR-099–JSR-100 |
| Restore/update/provider-extinction drills | JSR-060–JSR-062, JSR-088, JSR-093–JSR-095 |
| Artiom Test dimensions and drift | JSR-070–JSR-074, JSR-101–JSR-105, JSR-108 |
| Unpromoted twin candidates excluded | JSR-050, JSR-074, JSR-103–JSR-105 |
| Manual operation and remote STOP | JSR-055, JSR-091 |
| Autonomous utility beats baseline safely | JSR-056, JSR-075, JSR-106, JSR-108 |

## 19. Operator-requirement traceability

| Requirement | Why the architecture needs it | Owning slices |
|---|---|---|
| R-01 portable work module | The operator asked to upload/download work across devices; therefore paths, runtime, state, restore and leadership must be explicit rather than machine-local accidents. | JSR-007–JSR-014, JSR-060–JSR-061, JSR-093–JSR-094 |
| R-02 local gated personal module | Personal material must remain separately encrypted and mounted only on authorised laptops. | JSR-037–JSR-041, JSR-082–JSR-083 |
| R-03 immutable raw exports | Derived memories must remain challengeable against unchanged source evidence. | JSR-019, JSR-023, JSR-078–JSR-079 |
| R-04 literal memory orders | A direct “remember this” instruction must survive compiler/model changes as an authenticated append-only event. | JSR-018, JSR-029 |
| R-05 inferred temporal memory | Exports need evidence-linked event inference without inventing timestamp precision. | JSR-016–JSR-017, JSR-040 |
| R-06 three visible confidence tiers | The requested low/inferred/certain display needs separate epistemic class, confidence and review fields. | JSR-016–JSR-017 |
| R-07 speech/reasoning/preference learning | Behavioural dimensions need separate datasets, evaluations and promotion gates so one convincing voice model cannot gain decision authority. | JSR-050, JSR-071–JSR-074, JSR-101, JSR-103–JSR-105 |
| R-08 Jarvis is the engine | Persistence, observation, routing, initiative and recovery belong to the runtime; twin artifacts remain optional payloads. | JSR-042–JSR-062, JSR-084–JSR-095 |
| R-09 faceless autonomy | Work must continue without personal memory, so portable state and operational compartments precede the Vault. | JSR-013, JSR-033–JSR-036, JSR-056 |
| R-10 Twin mode | Mounting personal context must be explicit, version-bound and independent from action authority. | JSR-038–JSR-041, JSR-050, JSR-082–JSR-084 |
| R-11 self-initiative | Acting without a fresh prompt requires goal-linked candidates and a deterministic why-now record before execution. | JSR-051–JSR-056, JSR-089–JSR-091 |
| R-12 control of domain systems | Broad workflow control requires typed adapters and external receipts, not shared databases or model tool calls. | JSR-063–JSR-069, JSR-096–JSR-100 |
| R-13 six-year continuity | Long duration requires supervision, heartbeats, credential isolation, checkpoints, fencing, restore and update rollback. | JSR-057–JSR-062, JSR-088, JSR-092–JSR-095 |
| R-14 Artiom Test and online representation | Identity behavior must be a channel-scoped capability measured privately before promotion. | JSR-070–JSR-075, JSR-101–JSR-108 |
| R-15 accepted Vault risk | Accepted access still needs integrity, provenance and audit so use does not become silent corruption. | JSR-037–JSR-041, JSR-082–JSR-083 |
| R-16 email/calendar without Vault | Operational sources must live in independent read-only compartments. | JSR-034–JSR-036 |
| R-17 unavoidable Constitution | Policy must bind boot, requests, models, context, actions, receipts and releases through deterministic enforcement and bypass tests. | JSR-024–JSR-032, JSR-080–JSR-081 |
| R-18 initial router | Stimuli need typed classification, destination, context/model roles and candidate tools while authority remains with the kernel. | JSR-042–JSR-047, JSR-084 |
| R-19 Overseer inside Orchestrator | Jarvis consumes candidate-bound certificates rather than recreating assurance. | JSR-064–JSR-065 |
| R-20 Market Aligner owns JAA/profiler loop | Career truth remains product-owned while PPP/personal changes arrive as proposals to one canonical writer. | JSR-066–JSR-067, JSR-083, JSR-096–JSR-097 |
| R-21 LoRA later | Base+retrieval must work first; research artifacts remain quarantined until September/October evaluation and explicit promotion. | JSR-050, JSR-071–JSR-074, JSR-101, JSR-103–JSR-105 |
| R-22 manual work remains valid | Explicit manual mode, STOP and domain ownership keep work accessible if Jarvis is paused or rejected. | JSR-055, JSR-063–JSR-069, JSR-091, JSR-096–JSR-100 |

## 20. What not to build early

- Do not begin LoRA training as a dependency of Faceless or Twin Mode.
- Do not create a universal daemon/gateway before the in-process shadow kernel proves the need.
- Do not migrate all state into one database; ownership boundaries are intentional.
- Do not give email/calendar write authority in their read-only compartment slices.
- Do not make the router an authority engine.
- Do not let a model call an adapter directly.
- Do not absorb Market Aligner or Orchestrator internals into Jarvis.
- Do not put secrets or Vault content in the portable runtime/state bundle.
- Do not call process exit, model prose or a sent command “success.”
- Do not attempt broad online representation before private baselines and channel policy.
- Do not pursue six-year uptime as one enormous slice; accumulate supervised, restored and
  failure-injected evidence.

## 21. Blueprint maintenance

After each slice:

1. Until JSR-003 is DEPLOYED, update the catalogue's checklist status and evidence/work-order link
   in the same change as the work or decision it records.
2. Once JSR-003 is DEPLOYED, make its validated ledger authoritative and generate the catalogue
   checklist columns from it; never maintain two editable status sources.
3. Add the verification/rollback receipt and exact source/release identities.
4. Recompute affected architecture lineage.
5. Mark new risks and remove a risk only when its closing receipt proves it.
6. Recheck dependencies of READY slices whose prerequisites changed.
7. Keep historical catalogue wording stable; material redesigns create a superseding version.

Review the blueprint:

- at each slice sign-off;
- after any architecture/authority change;
- after a runtime/provider/platform major change;
- after a failed restore, bypass or external-effect incident;
- after 90 days without implementation;
- before September/October behavioural-model research;
- before any widening of autonomous or representation authority.

## 22. First-session instruction

The first implementation session is a maximum 20-minute selection and proof-of-absence pass, not
an automatic JSR-002 build. It may correctly end as DO NOTHING or ADOPT EXISTING. If current work is
being harmed by the known red memory gate, prepare a bounded JSR-015 work order. If a different
observed problem has higher present value, select its smallest qualifying slice. Otherwise return
to current work.

This instruction exists because the operator asked for a blueprint that can survive interruption
without turning spare compute time into unnecessary work. The baseline, runtime-lock and path
slices remain available when real recovery, clean-environment or device-transfer needs make their
value concrete; none is compulsory groundwork for unrelated current work.

## 23. Explicit non-claims

- This blueprint does not ratify proposal 11.
- It does not claim that any catalogue slice is implemented merely because predecessor code exists.
- It does not grant Jarvis permission to access the Personal Vault or represent Artiom.
- It does not promise six maintenance-free years; it decomposes the evidence needed to approach
  long-lived operation.
- It does not assign completion percentages to architectural intent.
- It does not freeze the catalogue size or make the current number of entries architecturally
  meaningful.
- It does not require every current or future slice to be built.

The architecture succeeds incrementally: each verified slice is one useful improvement—not a
broken system measured by the unbuilt remainder of an open catalogue.
