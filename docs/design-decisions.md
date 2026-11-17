# Design decisions

Rationale for the shape of the definition set, as reconstructed from the artefacts themselves. Each entry records the decision, the evidence for it, and the alternative it displaced.

## Decision: express eligibility declaratively through `spawn_chance`

**Decision.** Modifier eligibility is expressed entirely as condition-and-weight clauses rather than as procedural logic.

**Evidence.** All six definitions use `spawn_chance` with `add`, `factor`, and condition tests.

**Why.** The format offers no procedural alternative. The consequence, however, is architectural: every exclusion is multiplication by zero, so the repository's correctness depends on authors remembering to write exclusions rather than on any guard.

## Decision: split the definitions across two files

**Decision.** New modifiers live in `ui-planet-modifiers.txt`; redefinitions of host-owned modifiers live in `ui-planet-modifiers-vanillas.txt`.

**Evidence.** The `-vanillas` suffix distinguishes overrides from additions, and the override file contains only vanilla keys whose own definitions gain an exclusion.

**Why.** The split makes the mod's interaction with host data auditable in one place. A contributor reviewing compatibility risk can read one file rather than hunting for host-key bindings throughout the additions.

**Alternative displaced.** A single combined file, which would have obscured which definitions are additions and which are overrides.

## Decision: re-declare vanilla keys rather than introduce parallel keys

**Decision.** Overrides use the vanilla key (`pm_weak_magnetic_field`), not a parallel `pm_ui_weak_magnetic_field`.

**Evidence.** The override file declares `pm_weak_magnetic_field` and `pm_strong_magnetic_field`, and binds each `modifier` to the matching vanilla effect key.

**Why.** Mod files load after the host's `00_planet_modifiers.txt`, so re-declaring a key replaces the definition instead of adding a second candidate that applies the same effect. The earlier parallel-key approach let one effect be selected twice, distorting the pool and making the effective probability of the vanilla modifier depend on engine key-merging behaviour the repository cannot observe.

**Trade-off accepted.** Each override must copy the vanilla conditions forward, so a later host balance change is not inherited automatically. The set is therefore kept as small as mutual exclusivity allows.

## Decision: do not override modifiers the mod does not interact with

**Decision.** Hazardous weather, unstable tectonics, asteroid impacts, wild storms, and both gravity modifiers are no longer overridden.

**Evidence.** No definition in the additions file excludes or is excluded by those modifiers under a conflicting theme, and the mod reads `pm_low_gravity` and `pm_high_gravity` only as weighting signals.

**Why.** A parallel candidate for a modifier the mod does not touch adds a second way to roll the same effect without any exclusivity benefit, inflating that outcome's probability. Removing those overrides restores vanilla odds.

## Decision: bind overrides to host effect keys rather than restating effects

**Decision.** Redefinitions declare only `spawn_chance` and set `modifier` to the host's effect key.

**Evidence.** `pm_weak_magnetic_field` binds to `weak_magnetic_field` and declares no effect block; `weak_magnetic_field` is not defined anywhere in the repository.

**Why.** Restating the effects would risk diverging from the host's balance values and duplicating maintenance. The trade-off accepted is that a host-side rename of an effect key breaks the binding invisibly.

## Decision: write mutually exclusive exclusions symmetrically

**Decision.** Where the mod introduces a conflict, both conflicting definitions exclude each other.

**Evidence.** `pm_ui_salty` and `pm_ui_swampy` exclude each other; the magnetic family shares an identical identifier list, closed from the vanilla side by the override file.

**Why.** Selection can evaluate one candidate before or after another, so a one-sided exclusion would fire inconsistently. Symmetry removes the dependence on evaluation order. See [concurrency-and-scheduling.md](./concurrency-and-scheduling.md).

**Where symmetry requires an override.** A vanilla modifier cannot exclude a mod modifier unless the mod re-declares it. The two magnetic-field keys are therefore re-declared with the reciprocal exclusion added.

## Decision: cap accumulation per modifier

**Decision.** Every definition carries `num_modifiers > 1`, matching the vanilla convention.

**Evidence.** Read directly from the guard clauses.

**Why.** The guard prevents a planet from accumulating an unbounded number of modifiers, which would compound the penalties. An earlier revision exempted swampy and relaxed the guard for microorganisms on the reasoning that an effect-less modifier was worth a third slot; once microorganisms gained an effect, both exceptions lost their justification and were removed.

## Decision: make the new modifiers predominantly penalties

**Decision.** Every new modifier is negative; none carries a bonus.

**Evidence.** The effect-block inventory shows negative habitability, happiness, growth, food, physics, farming-district, and building-cost values only.

**Why.** The mod's stated purpose is diversity of planetary generation. Adding powerful positive modifiers would change game balance rather than flavour. Every magnitude stays inside the vanilla band for a comparable modifier so the new entries do not dominate the pool.

## Decision: drop modifiers that duplicate a vanilla concept

**Decision.** Perpetual Twilight, Microorganisms, Giant Earthworms, and Sea Monsters were removed.

**Evidence.** Vanilla v4.5 already provides `pm_tidal_locked` (a twilight terminator with an unusable half-planet), `pm_rich_mircoflora` (microbial soil with farming and society research), `pm_subterranean_wildlife` (burrowing fauna with mining and society research), and `pm_dangerous_wildlife` / `pm_titanic_life` (large fauna with a habitability penalty and society research).

**Why.** A new modifier that only duplicates an existing one consumes a pool slot, dilutes the weights of genuinely new outcomes, and forces a long exclusion list to keep the two apart. Removing them leaves four modifiers with no vanilla equivalent, and the exclusion lists shrink to what the magnetic family and the salinity/swampland conflict require.

**Cost.** Removing an identifier is save-breaking for a planet that already carries it; the affected saves lose the modifier and its localisation.

## Decision: keep the editable artwork outside the content directory

**Decision.** The GIMP source lives in `src/`, while the DDS outputs live under `ui-planet-modifiers/gfx/`.

**Evidence.** The tree confirms the separation, and the descriptor `path` excludes `src/`.

**Why.** Placing the source inside the content directory would ship it to players and would expose an editor-format file to the engine's asset enumeration.

## Decision: retain the `ui_` identifier element

**Decision.** Keys and file names continue to use the `ui_` element inherited from the original repository layout, even though the mod title uses the *Universum Infinitum* series name.

**Evidence.** The mod title is *Universum Infinitum - Planet Modifiers*, while keys are `pm_ui_*` and the content directory is `ui-planet-modifiers`.

**Why.** The project was moved from an earlier combined repository, and renaming the identifiers would have broken the key bindings and every existing save that stores assigned modifier identifiers. The cost is a naming mismatch between the project title and its identifiers, recorded in [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md).

## Decision: ship a single locale

**Decision.** Only English localisation is present.

**Evidence.** `localisation/` contains only `english/`.

**Why.** The original release target was the English-speaking Steam Workshop audience. The format is additive, so a new locale requires no change to any existing file — see [components/localisation.md](./components/localisation.md).

## Decision: distribute through platforms rather than a build pipeline

**Decision.** The repository contains no build step and no packaging script; release packaging is delegated to `.github/workflows/github-release.yml`.

**Evidence.** The only workflow is `.github/workflows/github-release.yml`, which zips the descriptor and content directory on release publication; no manifest or script compiles anything.

**Why.** A mod of static data files requires no compilation, so the workflow only archives the committed content. The archive is produced from the tagged revision, which removes the drift that a manually uploaded archive allowed; it does not validate the content, so in-game verification remains a manual prerequisite for a release.

## Related documentation

- [architecture.md](./architecture.md) for the architectural model
- [invariants.md](./invariants.md) for the constraints these decisions produce
- [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) for the outstanding items
