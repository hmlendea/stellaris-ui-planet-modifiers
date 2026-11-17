# Design decisions

Rationale for the shape of the definition set, as reconstructed from the artefacts themselves. Each entry records the decision, the evidence for it, and the alternative it displaced.

## Decision: express eligibility declaratively through `spawn_chance`

**Decision.** Modifier eligibility is expressed entirely as condition-and-weight clauses rather than as procedural logic.

**Evidence.** All eighteen definitions use `spawn_chance` with `add`, `factor`, and condition tests.

**Why.** The format offers no procedural alternative. The consequence, however, is architectural: every exclusion is multiplication by zero, so the repository's correctness depends on authors remembering to write exclusions rather than on any guard.

## Decision: split the definitions across two files

**Decision.** New modifiers live in `ui-planet-modifiers.txt`; redefinitions of host-owned modifiers live in `ui-planet-modifiers-vanillas.txt`.

**Evidence.** The `-vanillas` suffix distinguishes overrides from additions, and the override file contains only keys that bind to host-owned effect keys, plus `pm_null`.

**Why.** The split makes the mod's interaction with host data auditable in one place. A contributor reviewing compatibility risk can read one file rather than hunting for host-key bindings throughout the additions.

**Alternative displaced.** A single combined file, which would have obscured which definitions are additions and which are overrides.

## Decision: bind overrides to host effect keys rather than restating effects

**Decision.** Redefinitions declare only `spawn_chance` and set `modifier` to the host's effect key.

**Evidence.** `pm_ui_low_gravity` binds to `low_gravity` and declares no effect block; `low_gravity` is not defined anywhere in the repository.

**Why.** A mod cannot add conditions to a host definition, but it can introduce a new candidate that applies the same effect. Restating the effects would have risked diverging from the host's balance values and duplicating maintenance. The trade-off accepted is that a host-side rename of an effect key breaks the binding invisibly.

## Decision: write mutually exclusive exclusions symmetrically

**Decision.** Where the mod introduces a conflict, both conflicting definitions exclude each other.

**Evidence.** `pm_ui_twilight` excludes `pm_ui_extreme_temperatures` and vice versa; the magnetic family shares an identical five-identifier list in all three members.

**Why.** Selection can evaluate one candidate before or after another, so a one-sided exclusion would fire inconsistently. Symmetry removes the dependence on evaluation order. See [concurrency-and-scheduling.md](./concurrency-and-scheduling.md).

**Where the decision was not applied.** `pm_ui_unstable_tectonics` excludes high gravity, but high gravity does not exclude unstable tectonics, and giant earthworms and sea monsters do not exclude one another. Accumulation guards prevent the combinations in practice.

## Decision: cap accumulation per modifier

**Decision.** Seven definitions carry `num_modifiers > 1`; microorganisms uses `num_modifiers > 2`; swampy carries no guard.

**Evidence.** Read directly from the guard clauses.

**Why.** The guard prevents a planet from accumulating an unbounded number of modifiers, which would compound the penalties. Microorganisms is permitted a third slot because it has no effect and is meant to be visible; swampy is unguarded, which reads as an oversight or a deliberate soft limit rather than a considered exception, and is recorded in [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md).

## Decision: make the new modifiers predominantly penalties

**Decision.** All eight new modifiers are neutral or negative, with two carrying an offsetting research bonus.

**Evidence.** The effect-block inventory shows mainly negative habitability, happiness, growth, and food values, plus two positive society-research values.

**Why.** The mod's stated purpose is diversity of planetary generation. Adding powerful positive modifiers would change game balance rather than flavour. Giant earthworms and sea monsters carry a society bonus because their theme — large fauna — plausibly yields biological research.

## Decision: differentiate modifiers by eligibility rather than by effect

**Decision.** Giant earthworms and sea monsters have numerically identical effect blocks and differ only in their spawn conditions and icons.

**Evidence.** Both declare `pop_environment_tolerance = -0.10`, `pop_happiness = -0.05`, and `planet_jobs_society_research_produces_mult = 0.20`; the eligibility tier separates them by planet class.

**Why.** Two thematically distinct phenomena were desired for arid and oceanic worlds respectively. Sharing an effect block avoids implying a balance difference that does not exist. The cost is that a player sees two differently named modifiers with identical tooltips.

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

**Decision.** The repository contains no build step, packaging script, or release automation.

**Evidence.** No workflow directory, no manifest, no script; the published GitHub release carries a single pre-built archive.

**Why.** A mod of static data files requires no compilation. The archive is produced manually and uploaded. The cost is that the release archive can drift from the repository, since nothing regenerates it automatically.

## Related documentation

- [architecture.md](./architecture.md) for the architectural model
- [invariants.md](./invariants.md) for the constraints these decisions produce
- [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) for the outstanding items
