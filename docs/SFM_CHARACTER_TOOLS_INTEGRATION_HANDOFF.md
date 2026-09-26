# Character Preset Manager → SFM Character Tools Integration Handoff

**Status:** Controlling CPM convergence handoff  
**Checkpoint purpose:** Documentation-only architecture checkpoint before external adversarial review  
**Implementation status:** No CPM integration code has been written at this checkpoint

This document supersedes earlier CPM → SFM Character Tools handoff drafts and review addenda. It is intended to stand alone.

## 1. Product architecture and migration objective

The target product is **SFM Character Tools**:

- Control Group Normalizer
- Character Preset Manager (CPM)
- one shared Animation Groups Master / compiled sidecar authority
- one canonical shared broker/runtime
- separate consumer projections and lifetimes
- SFM Model Scanner as an optional contribution utility

The shared-authority/Normalizer foundation is treated as settled through:

- **F — CLOSED**
- **G — PASS / CLOSED**
- **I — PASS / CLOSED**
- **J — PASS / CLOSED**

The migration objective is deliberately narrow:

> **unchanged canonical shared broker + CPM-owned `cpm_compat_v1` fold-family projection + CPM-owned exact-literal interpretation + existing CPM product semantics**

CPM should join the settled shared foundation without redesigning qualified CPM mechanics or unnecessarily changing the shared package already used by the Normalizer.

## 2. Governing identities

### CPM

- Repository: `chadchan3d/sfm-character-preset-manager`
- Branch: `main`
- Migration baseline: `src/SFM_CSP_G18AN_SaveNewCopy.py`
- G18AN SHA-256: `3326024ddecd544ad1e10659bbf7b98420b5f147fca775433878c19fd9e66b3e`
- G18AN Git blob: `e8c1bceecdd2d3569168a35b27b8e49d7a38b911`
- Internal version: `0.2.0-rc7-g18an-save-new-copy`

No later runnable CPM candidate is authoritative. This checkpoint must not create one.

### Shared authority / Normalizer

The currently accepted shared runtime identity is:

- API: `1.0.0-b2a`
- build: `package-boundary-corrected-2026-09-22`

The shared runtime is imported canonically as:

`sfm_master_authority_productionized.runtime`

The production Normalizer already uses the canonical runtime and broker, including origin/build checks, `runtime.get_broker(...)`, `broker.acquire_or_reuse_views(...)`, `expected_generation`, explicit view leases, and deterministic lease release.

Historical material that says the shared authority is not yet wired into a production consumer is superseded by the current production Normalizer and current F/G/I/J evidence.

## 3. What remains qualified and should not be redesigned

The authority migration sits above already-qualified CPM mechanics. Absent contradictory new evidence, do not reopen:

- RC7 native mutation sequencing;
- preflight before Undo creation;
- no-op-before-Undo behavior;
- Abort/Finish/rollback discipline;
- same-time refresh and independent readback boundaries;
- the prohibition on unsafe native access patterns already excluded by qualification;
- generic complete bone scaling;
- operation-scoped indexed Body capture;
- normalized model path + checksum as durable model identity;
- Animation Set name as mutable metadata;
- fresh live DME/native binding resolution per operation;
- exact-set preset compatibility;
- miss-only Review;
- authority positive/conflict precedence over Review;
- structural/native Clothing Fit source-target mapping;
- one changed Fit target = one native transaction;
- one Fit target per zero-delay Qt event turn;
- foreign-modal yielding;
- scalar Qt row identifiers with complex records kept Python-owned;
- no silent TXT fallback;
- the conclusion that the old multi-second Body Save/Update cost was caused by repeated native bone-map traversal and was already addressed by indexed capture.

The authority migration must preserve these mechanics rather than treating them as open architecture questions.

## 4. Current G18AN authority behavior to preserve

G18AN's current development-era provider path is not the final production seam, but its consumer semantics are the migration oracle.

For each exact queried flex literal, the existing answer contract carries:

- `query_literal`;
- `status`;
- `match_kind`;
- `resolved_path`;
- `destinations`;
- `spellings`;
- `occurrence_count`.

Current semantic status values include:

- `resolved`;
- `conflict`;
- `absent`;
- authority-unavailable as a separate failure state.

Current match values are:

- `exact`;
- `ascii-fold`;
- `none`.

G18AN uses `match_kind` to maintain folded-resolution counts. It also stores `match_kind`, destinations, and Master spellings in its semantic snapshot, and they participate in `semantic_snapshot_signature()`.

Therefore these semantics must not disappear incidentally during the shared-authority migration.

### Path representation

G18AN strips the sidecar's synthetic outer wrapper before CPM sees semantic paths.

CPM therefore expects paths such as:

- `Face/...`
- exact `Body Morphs`
- `Clothing/...`

and not lower-level forms such as:

- `groupFile/Face/...`
- `groupFile/Body Morphs`

The migration must preserve this wrapper-stripped representation exactly.

## 5. Shared authority remains unchanged

CPM will **not** add a production CPM adapter module to `sfm_master_authority_productionized` for this migration.

The accepted shared package remains the authority owner for:

- effective Master observation;
- sidecar selection and source-generation validation;
- provider lifetime;
- generation identity;
- cache ownership;
- cache admission and eviction;
- aggregate resource accounting;
- requested-vocabulary admission;
- authorization tokens;
- leases;
- generation invalidation and retire/drain behavior.

CPM uses the broker's already-supported consumer-supplied projection-builder seam.

This preserves the currently qualified shared package and the Normalizer's currently pinned shared-build identity rather than creating a new shared-package build solely to add CPM.

If a later design materially changes the shared package or adds API surface consumers depend upon, the package's build-identity policy still applies: the build ID must change and all production consumers must converge on that same newly identified build. Do not silently retain the old build string after materially changing what it identifies.

## 6. CPM owns the projection adapter

CPM owns the production consumer projection code.

Use:

`consumer_kind = "cpm_compat_v1"`

The version suffix is part of cache identity. Until the shared authority has an independent consumer-projection contract-version dimension, a future incompatible projection must use a new consumer identity such as:

`cpm_compat_v2`

A v2 consumer must not reuse a v1 cache entry.

### Builder contract

The CPM-owned adapter is passed to the canonical broker through `request_specs`.

It:

- receives a temporary provider only through the broker-owned builder callback;
- never opens a sidecar itself;
- never selects a generation;
- never owns or retains a provider;
- never creates another broker;
- never bypasses broker admission;
- declares the exact requested folded vocabulary;
- declares the accurate request scale required by the accepted broker contract;
- returns detached payload + shared `CoverageDescriptor` + conservative `estimated_bytes`;
- returns before cohort/provider teardown;
- retains no provider or provider-backed state.

The adapter may use the pinned shared package's core coverage/view types and broker-supplied provider callback protocol. That is an intentional dependency on the exact API/build CPM verifies at startup, not a second authority implementation.

## 7. Broker-cached CPM projection is fold-family based

The accepted broker cache identifies a detached view using the effective combination of:

- Master generation;
- projection contract version;
- covered folded-key set;
- consumer kind.

Exact request spelling is not part of that identity.

Therefore the cached CPM projection must contain only facts invariant for a folded family.

It must **not** cache:

- exact `query_literal`;
- exact-query-dependent `match_kind`.

### `cpm_compat_v1` payload

Conceptually:

```text
{
    "contract": "cpm-compat-v1",

    "families_by_fold": {
        <folded key>: {
            "status": "resolved" | "conflict" | "absent",
            "resolved_path": <wrapper-stripped unique path> | None,
            "destinations": [<wrapper-stripped path>, ...],
            "spellings": [<actual Master literal spelling>, ...],
            "occurrence_count": <integer>
        }
    }
}
```

Every family row must be a property of the Master fold family itself.

### Unique resolution

For a family with one destination:

- `status = "resolved"`
- `resolved_path` is the unique wrapper-stripped destination;
- `destinations` contains that destination;
- `spellings` contains the actual Master spellings in the family;
- `occurrence_count` records the family occurrences.

### Conflict

Whatever provider-native representation denotes a fold family with multiple destinations must normalize to:

- `status = "conflict"`
- `resolved_path = None`
- complete normalized `destinations`;
- complete actual Master `spellings`;
- family `occurrence_count`.

CPM must not depend on whether the provider internally represented this as a conflict object or as occurrence rows spanning multiple destinations.

### Healthy absence

For a genuinely searched fold with no Master match:

- `status = "absent"`
- `resolved_path = None`
- `destinations = []`
- `spellings = []`
- `occurrence_count = 0`

Healthy absence is distinct from authority failure and from Uncovered.

## 8. Exact-literal interpretation remains CPM-owned

When materializing the pure CPM semantic scope, CPM starts from the **current exact live literal**.

For each literal:

1. apply the canonical ASCII-fold rule;
2. find its fold-family row in `families_by_fold`;
3. reconstruct the existing G18AN answer contract.

The exact answer is:

```text
{
    "query_literal": <current exact live literal>,
    "status": <family status>,
    "match_kind": <reconstructed below>,
    "resolved_path": <family resolved path>,
    "destinations": <family destinations>,
    "spellings": <family Master spellings>,
    "occurrence_count": <family occurrence count>
}
```

`match_kind` is:

- `none` if the healthy family result is absent;
- `exact` if the current exact live literal occurs in the family's actual Master spellings;
- otherwise `ascii-fold`.

The reconstructed exact answer then feeds the existing G18AN-style semantic snapshot and product classification.

This split is deliberate:

```text
broker cached view
    = fold-family authority facts

CPM scope materialization
    = current exact live literal
    + fold-family facts
    -> G18AN-compatible exact answer

CPM product policy
    = Body / Expression / Other / Review / Conflict
```

## 9. Coverage and resource-accounting contract

The CPM projection participates normally in the existing broker contract.

The request uses:

`consumer_kind = "cpm_compat_v1"`

and declares the exact folded request set.

The builder must expose the accepted-build equivalents of:

- `declared_request_folds`;
- `declared_request_scale`.

The broker remains responsible for:

- packed per-family admission;
- cumulative requested-vocabulary accounting;
- aggregate retained/transient accounting;
- provider open/close;
- H0/H1 generation validation;
- atomic view publication;
- eviction;
- authorization;
- leases.

The CPM adapter's `estimated_bytes` covers only the detached fold-family payload retained by the broker.

Later exact-literal answers, live-binding data, semantic classes, Review overlays, and semantic snapshot/signature state belong to CPM's pure scope and are not part of broker retained-view accounting.

### Coverage distinctions

Maintain the shared distinction between:

- Known;
- MasterUnknown;
- Uncovered;
- AuthorityUnavailable at acquisition level.

For CPM:

- a uniquely resolved family can be broker coverage Known;
- a conflict family can also be broker coverage Known because the requested family was successfully materialized;
- a healthy missing family maps to broker MasterUnknown and CPM `absent`;
- a requested fold that is Uncovered is a projection/coverage contract failure;
- AuthorityUnavailable means no trustworthy CPM view exists.

Only a genuine healthy Master absence may enter Review.

Uncovered and AuthorityUnavailable must never be converted into `absent`.

## 10. Shared authority versus CPM policy

### Shared broker/runtime owns

- canonical package identity;
- one process broker;
- Master observation;
- sidecar/source admission;
- provider lifetime;
- generation;
- requested-fold resource accounting;
- cache;
- authorization;
- leases;
- retire/invalidate/drain.

### CPM projection adapter owns

Neutral fold-family materialization:

- unique resolution;
- provider-native conflict normalization;
- genuine Master absence;
- wrapper stripping;
- normalized destinations;
- actual Master spellings;
- occurrence counts;
- complete requested coverage;
- conservative detached payload estimate.

### CPM scope materialization owns

- current exact `query_literal`;
- exact versus ASCII-fold `match_kind`;
- reconstruction of the full G18AN answer.

### CPM product layer owns

- Body classification;
- Expression classification;
- Other;
- Review;
- Exclude/Reclassify;
- semantic snapshot/signature;
- preset compatibility;
- generic bone scaling;
- Clothing Fit structural/native mapping;
- native scene mutation and Undo.

The shared broker never needs to understand “Body Preset.”

## 11. Long-lived pure scope, short broker lease

CPM retains its pure selected-character semantic scope during UI idle time.

It does **not** retain a broker lease merely because the character remains selected.

### Long-lived CPM state may include

- model identity;
- exact semantic literals;
- Body/Expression/Other classifications;
- genuine misses;
- conflicts;
- reconstructed `match_kind`;
- Master spellings;
- normalized destinations;
- Review metadata;
- vocabulary signature;
- representation signature;
- semantic snapshot/signature;
- semantic policy revision;
- authority generation SHA.

This is detached Python state.

### Short broker leases

A lease exists only while CPM is consuming a shared view for a bounded authority operation, such as:

- initial scope construction;
- action-boundary freshness/reauthorization;
- a Clothing Fit target boundary.

Leases are deterministically released.

No CPM lease should survive ordinary UI idle time.

## 12. Initial scope construction

The intended sequence is:

```text
live model vocabulary
        ↓
folded requested vocabulary
        ↓
canonical broker
        ↓
acquire_or_reuse_views(...)
        ↓
cpm_compat_v1 DetachedView
        ↓
short ViewLease
        ↓
materialize exact G18AN-compatible answers
        ↓
build pure CPM semantic scope
        ↓
release lease
        ↓
publish scope into CPM UI
```

The broker owns the provider throughout the cohort. The resulting CPM scope retains no provider/native backing.

## 13. Action-boundary freshness proof

Before a later semantic-dependent operation, CPM performs a fresh:

`broker.acquire_or_reuse_views(... expected_generation=<scope generation>)`

using the same requested folded vocabulary and `cpm_compat_v1` consumer identity.

This is the generation proof.

The accepted broker performs a fresh Master observation even on a fully reusable cached path, so this boundary can reject an old scope generation without requiring an independent CPM filesystem watcher.

### Successful reauthorization

If acquisition succeeds under the same expected generation:

- take a short lease;
- verify authorization and requested coverage;
- perform the bounded semantic planning required by the action;
- release the lease;
- continue using detached CPM state.

A complete scope rebuild is not required merely to prove the same generation is still current.

### Generation mismatch

If the old generation no longer matches:

- no mutation begins;
- do not reinterpret the mismatch as Review;
- do not silently replay the old action under the new generation;
- invalidate/discard the selected semantic scope;
- acquire current authority;
- rebuild the pure scope under the new generation;
- release its build lease;
- republish Body / Expression / Review state;
- require a new user action.

## 14. Astra challenge point: lease release before native mutation

The proposed migration boundary is:

> After successful fresh expected-generation acquisition and all semantic planning required by the operation, CPM may release the short broker lease and allow the already-planned bounded native/storage mutation to finish from detached state.

The rationale is that such a mutation should not reread authority after planning, and the next semantic boundary performs another fresh generation proof.

This boundary is **not declared conclusively proven by this handoff**.

Astra must independently attack it operation by operation.

Specifically, determine whether after the proposed lease-release point any path:

- performs another semantic lookup;
- depends on provider/view lifetime;
- schedules a callback that still requires authority;
- can reinterpret pending semantic work after generation replacement;
- lacks all required detached planning state.

If any migrated operation still needs authority after the proposed release point, move that operation's release boundary later. Do not expand lease lifetime globally without evidence.

## 15. Clothing Fit generation rule

One user-initiated Clothing Fit owns one semantic generation, `Gfit`.

Before every queued target begins, CPM re-proves that same generation through:

`acquire_or_reuse_views(... expected_generation=Gfit)`

The existing Fit mechanics remain:

- one target per zero-delay Qt event turn;
- one changed target per native transaction/Undo;
- structural/native target correspondence.

### If generation remains Gfit

For the target:

- acquire/reuse `cpm_compat_v1`;
- take a short lease;
- verify coverage/authorization;
- perform target semantic planning under Gfit;
- release the lease at the boundary ultimately accepted by Astra review;
- run the existing target mutation;
- queue the next target.

### If generation changes between targets

If target 1 committed under G1 and G2 becomes current before target 2:

- target 1 remains committed;
- target 2 does not begin;
- remaining targets do not switch to G2 inside the same Fit;
- terminate that Fit;
- rebuild selected CPM semantic scope under current authority;
- a later user-initiated Fit may begin under G2.

One Fit must never silently span multiple semantic generations.

## 16. Canonical bootstrap/import

CPM uses the same canonical package-bootstrap contract as the qualified Normalizer.

Before package import, use the same `sys.executable`-derived MAINMENU location formula already established by the Normalizer.

Then import only the canonical package identity:

- `sfm_master_authority_productionized.runtime`
- shared errors/bootstrap modules as required.

CPM must verify:

- expected package origin;
- runtime API `1.0.0-b2a`;
- runtime build `package-boundary-corrected-2026-09-22`;
- canonical module identity;
- broker acquisition through `runtime.get_broker(...)`.

Do not:

- create a second authority owner;
- vendor another runtime copy into CPM;
- `execfile()` the shared runtime;
- use G18AN's development sidecar discovery path in the final production seam;
- introduce a second provider-discovery system.

The tiny pre-import locator formula may be duplicated intentionally because the package path must be established before its bootstrap module can itself be imported. Add an offline equivalence check against the Normalizer formula rather than redesigning bootstrap merely to remove those few lines.

## 17. Saved presets across authority generations

Authority generation is capture provenance, not a permanent preset compatibility lock.

If a preset was saved under G1 and current authority is G2:

### Compatible semantic set

If the current accepted semantic mutation-key set and the existing model/representation/bone compatibility requirements remain equal, Apply may continue.

Generation difference alone does not reject the preset.

### Membership change

If current authority changes the accepted Body or Expression mutation-key set, existing exact-set compatibility rejects the old preset.

Do not partially apply it.

### Authority unavailable

If current authority cannot establish the accepted semantic set, semantic-dependent Apply fails closed before mutation.

## 18. Shared-authority migration hazards

### Real hazards that migration must close

- AuthorityUnavailable being mistaken for healthy absence.
- Uncovered being mistaken for healthy absence.
- conflict being flattened into resolved/Known at the CPM product layer.
- stale pure scope after authority generation change.
- exact-query-dependent data being placed in the fold-keyed shared cache.
- a future projection contract reusing the same consumer-kind cache identity.
- duplicate runtime/broker module ownership.
- a long-lived CPM lease delaying retire/drain.
- Clothing Fit switching generations between targets.
- old development-provider paths remaining accidentally reachable as silent production fallback.

### Not migration hazards

Do not reopen:

- scale graph mechanics;
- indexed Body capture;
- exact-set compatibility;
- native Undo design;
- Animation Set rename identity;
- Fit structural mapping;
- old Body Save performance work.

## 19. Historical dead ends / do-not-repeat work

Do not re-propose these superseded approaches without new evidence:

- mandatory per-character semantic profiles instead of shared Master authority;
- hardcoded Head Scale as the production scaling architecture;
- repeated full bone-map traversal for every Body validation step;
- provider failure converted into Unknown/Review;
- local Review overriding positive authority;
- local Review overriding conflicts;
- silent TXT fallback;
- long-lived DME/native objects in semantic scope;
- persistent cross-character Body Match tables as the current Fit architecture;
- Animation Set name as durable identity;
- generic same-name sidecar imports vulnerable to SFM module-path collisions;
- complex nested preset dictionaries retained in Qt `UserRole`;
- reopening the already-resolved multi-second Body Save investigation.

## 20. CPM convergence qualification gates

The following gates belong to CPM convergence before K.

### Gate C1 — fold-family projection parity

For representative:

- unique family;
- fold-only query family;
- healthy absence;
- multi-destination conflict;

compare the new cached family facts against the G18AN authority facts.

Require parity for:

- family status;
- wrapper-stripped resolved path;
- destinations;
- actual Master spellings;
- occurrence count.

### Gate C2 — exact-answer reconstruction parity

For each exact live literal:

- obtain its folded family;
- reconstruct `query_literal`;
- reconstruct `match_kind`;
- materialize the full G18AN-compatible answer.

Require parity for:

- query literal;
- status;
- match kind;
- resolved path;
- destinations;
- spellings;
- occurrence count.

### Gate C3 — wrapper/path parity

Prove no synthetic outer wrapper leaks to CPM.

Representative Face, `Body Morphs`, Clothing, nested Other, and conflict paths must match G18AN's representation exactly.

### Gate C4 — provider-native conflict normalization

Exercise the current shared provider's conflict representation and prove it becomes one CPM fold-family conflict with:

- `resolved_path = None`;
- complete normalized destinations;
- complete Master spellings;
- correct occurrence count.

The later exact query reconstructs exact/ASCII-fold match kind independently.

### Gate C5 — final semantic snapshot/signature parity

Using equivalent live binding fixtures, require migrated CPM semantic scope to match G18AN for:

- semantic status;
- reconstructed match kind;
- resolved path;
- destinations;
- Master spellings;
- semantic class;
- operation;
- folded-resolved count;
- accepted Body literals;
- accepted Expression literals;
- final semantic snapshot signature.

### Gate C6 — broker requested-vocabulary/resource-accounting proof

Prove:

- consumer kind is `cpm_compat_v1`;
- exact folded request set is declared;
- request scale is accurate;
- broker admission receives the real requested vocabulary;
- projection coverage covers every requested fold;
- missing requested coverage fails closed;
- Uncovered never becomes absent;
- no production path opens authority outside the broker.

### Gate C7 — AuthorityUnavailable boundary

Exercise controlled:

- no valid authority;
- source-generation mismatch;
- expected-generation rejection;
- canonical-runtime/build rejection;
- adapter contract failure.

Require:

- no synthetic absent rows;
- no false Review population;
- no semantic-dependent mutation.

### Gate C8 — lease lifecycle / no-idle-lease

Prove:

- scope build uses a short lease;
- pure scope survives lease release;
- ordinary UI idle holds no CPM lease;
- action reauthorization uses another short lease;
- normal paths return outstanding CPM lease count to baseline;
- release failure uses the broker's durable unreleased-lease mechanism rather than GC dependence.

### Gate C9 — expected-generation reauthorization

Build G1 scope.

Require:

- unchanged G1 passes;
- current G2 rejects the stale G1 expected generation;
- old user action performs no mutation;
- G2 scope rebuilds;
- old Review/classification data cannot silently override current positive/conflict authority.

### Gate C10 — canonical package/broker identity

Prove:

- CPM bootstrap resolves the current qualified package;
- expected origin matches;
- API/build identities match;
- runtime is canonical;
- CPM and Normalizer obtain the same process broker.

### Gate C11 — mandatory same-fold/different-exact-query cache reuse

This gate is decisive.

1. Choose exact literal A and fold F.
2. Acquire `cpm_compat_v1` for F.
3. Lease the view.
4. Materialize A's exact G18AN-compatible answer.
5. Release the lease.
6. Request a different exact literal B where `fold(A) == fold(B) == F`.
7. Require the broker to be able to reuse the same cached fold-family view.
8. Lease that reused view.
9. Materialize B independently from B + the cached family facts.
10. Release.

Require for B:

- `query_literal == B`;
- `match_kind` is recomputed correctly for B;
- status is correct;
- paths/destinations are correct;
- Master spellings are correct;
- occurrence count is correct;
- resulting G18AN semantic snapshot/signature matches the old provider.

The gate must fail if exact-query-dependent information has leaked into the cached projection.

### Gate C12 — projection-version cache separation

Using the same generation and folded coverage, prove:

- `cpm_compat_v1` cannot satisfy a `cpm_compat_v2` request.

No real v2 implementation is required; a fixture consumer identity is sufficient.

## 21. Minimum real-SFM CPM convergence tests

Do not repeat the whole historical CPM qualification campaign.

Run the smallest real-SFM set that exercises the new seam.

### Startup / scope

- open CPM;
- select representative model;
- build scope through canonical broker;
- confirm expected semantic results;
- confirm development provider path is unused;
- confirm no CPM lease remains after scope publication.

### Representative downstream path

Exercise enough existing functionality to prove the seam reaches qualified mechanics:

- Body Save or Update;
- changed/no-op Body Apply;
- representative Expression Apply;
- native Undo.

### Review

Exercise:

- healthy Master absence;
- local classification;
- Reclassify;
- positive authority;
- conflict;
- AuthorityUnavailable.

Only healthy absence may be reviewable.

### Open-window generation replacement

- build G1 scope;
- keep CPM open;
- activate controlled G2;
- invoke semantic-dependent action;
- G1 expected-generation proof rejects before mutation;
- CPM rebuilds G2 scope;
- original action does not replay automatically.

### Clothing Fit generation transition

- start Fit under G1;
- commit target 1;
- change to G2 before target 2;
- target-2 G1 proof fails;
- target 2 does not mutate;
- Fit stops;
- target 1 Undo remains valid;
- later new Fit may use G2.

### Minimal Normalizer coexistence sanity

During CPM convergence prove only:

- both consumers use the same canonical runtime;
- both obtain the same broker;
- CPM leaves no inappropriate lease;
- CPM does not destroy Normalizer state;
- one already-qualified representative Normalizer operation remains usable.

Broader cross-consumer workflow qualification belongs to K.

## 22. Remaining CPM blockers before K

After the authority seam is implemented, remaining CPM convergence work includes:

1. pass all adapter/cache/signature parity gates;
2. pass broker accounting/coverage gates;
3. pass canonical import/broker identity gate;
4. pass short-lease/freshness/generation-transition gates;
5. pass Fit generation-interruption test;
6. deliberately qualify rollback-verification failure for Body/Expression Apply;
7. deliberately qualify rollback-verification failure for Clothing Fit;
8. remove or isolate historical development authority machinery only after the new seam passes;
9. remove unnecessary diagnostic/development logging;
10. run focused post-cleanup regression.

Do not clean the old authority machinery in the same change that first proves the new seam.

## 23. K — SFM Character Tools product/workflow qualification

CPM is ready to enter K when:

- the production CPM authority path uses only the canonical broker;
- `cpm_compat_v1` parity is established;
- generation reauthorization is established;
- open-window replacement behavior is established;
- no idle CPM lease exists;
- Fit generation behavior is established;
- rollback failure gates are closed;
- historical development authority paths no longer present release ambiguity;
- no known CPM-specific convergence blocker remains.

K then qualifies the multi-consumer product, including:

- Normalizer first → CPM;
- CPM first → Normalizer;
- repeated alternation;
- CPM remaining open while Normalizer operates;
- close-order behavior;
- separate consumer view lifetimes;
- joint generation transitions;
- shared failure/recovery behavior;
- aggregate resource behavior;
- representative workflows from each tool.

## 24. L — installation/coexistence/release qualification

Leave for L:

- final installation layout;
- clean install;
- update install;
- exact installed shared package;
- exact installed CPM and Normalizer bytes;
- no duplicate authority package;
- no development deployment paths;
- no unintended TXT fallback;
- restart/reopen behavior;
- exact release hashes;
- final release-artifact CPM regression;
- required exact release-artifact Normalizer regression;
- coexistence from shipping bytes;
- packaging/documentation/provenance.

K proves the product workflow.

L proves the actual installed release.

## 25. Evidence and reading order for Astra

Astra should establish current governing state before reading historical architecture prose.

### CPM repository

Read:

1. `README.md`
2. `docs/ENGINEERING_NOTES.md`
3. this handoff
4. `src/SFM_CSP_G18AN_SaveNewCopy.py`

For G18AN, focus on:

- semantic status/match constants;
- current provider result shape;
- wrapper normalization;
- semantic snapshot construction/signature;
- pure-scope validity;
- Review;
- Body Save/Update;
- Apply/Fit;
- long-lived window behavior.

### SFM Animation Groups Master repository

Read current governing:

1. real-SFM qualification ledger;
2. production Normalizer;
3. canonical runtime;
4. broker;
5. cohort;
6. views/view cache;
7. bootstrap;
8. production Normalizer compatibility adapter;
9. F/G/I/J closeout evidence relevant to ownership, later vocabulary, generation replacement, and native protection.

Treat older prose saying the package is not yet used by production as historical when current production source/ledger contradicts it.

### Private evidence

Use private CPM evidence only for disputed historical qualification claims. Raw logs, handoff archives, workstation paths, and unrelated development evidence should not be added to the public repository merely for completeness.

## 26. Historical evidence boundary

The project predates Git.

Do not invent retrospective Git commits for earlier G/RC/Q checkpoints.

Git history begins with the surviving audited G18AN baseline. Earlier checkpoint identifiers remain development-history identifiers, not Git commits.

Raw development evidence is intentionally excluded from the public repository.

## 27. Two remaining Astra challenge points

The architecture review loop closes with exactly two deliberate adversarial questions still open.

### A. Consumer-owned projection boundary

Astra should independently determine whether any hidden broker/cohort/package invariant makes a CPM-owned projection builder unsafe despite the documented builder extension point.

The default is to preserve the unchanged J-qualified shared package unless evidence shows a correctness reason not to.

### B. Lease-release-before-mutation boundary

Astra should trace each migrated semantic-dependent CPM operation and determine whether all authority-dependent planning is truly complete before the proposed short lease is released.

If an operation still requires authority after that point, move that operation's release boundary later.

Do not globally retain long leases without evidence.

## 28. Controlling implementation statement

The controlling migration seam is:

> **unchanged canonical shared broker + CPM-owned `cpm_compat_v1` fold-family projection + CPM-owned exact-literal interpretation + existing CPM product semantics**

More explicitly:

```text
shared broker cache
    = generation
    + folded vocabulary
    + cpm_compat_v1
    + fold-family authority facts

CPM pure semantic scope
    = current exact live literals
    + reconstructed exact/ascii-fold match semantics
    + G18AN-compatible semantic snapshot

CPM product behavior
    = Body / Expression / Other / Review / Conflict
```

The migration should preserve valid broker cache reuse rather than preventing it, preserve G18AN's existing semantic/signature contract, keep the shared package byte-stable, and avoid reopening already-qualified CPM mechanics.

## Final checkpoint verdict

**A. CPM current-state verdict**

G18AN remains the correct behavioral migration baseline. CPM's qualified mutation, storage, identity, scale, Review, modal, and Clothing Fit mechanics should remain intact.

**B. Minimum migration plan**

Implement one CPM-owned, broker-mediated, fold-family `cpm_compat_v1` projection; reconstruct exact G18AN answers in CPM; retain pure scopes without idle leases; reauthorize generation before semantic-dependent actions; pin one Fit to one generation.

**C. Release blockers after migration**

Close the CPM-specific projection/cache/freshness/rollback gates, clean historical authority and diagnostic machinery after the seam is proven, then enter K. Exact installation/release qualification remains L.

**D. Evidence Astra should inspect**

Current CPM G18AN + Engineering Notes + this handoff, and current Animation Groups Master production Normalizer + runtime/broker/cohort/views/bootstrap + F/G/I/J evidence.

**E. Things Astra should explicitly not reopen**

Do not reopen settled CPM mutation sequencing, generic scaling, indexed capture, identity, exact-set compatibility, miss-only Review, structural Fit mapping, per-target transaction staging, modal behavior, or the old Body Save performance investigation without contradictory evidence.

The only intentional architecture challenges remaining are the consumer-owned projection boundary and the lease-release-before-mutation boundary.
