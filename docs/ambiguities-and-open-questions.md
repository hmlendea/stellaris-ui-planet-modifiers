# Ambiguities and open questions

Unresolved items identified during documentation, with the evidence that leaves each open. None is presented as a defect; each is a place where the repository is silent or where a reasonable reader could reach two conclusions.

## Why `pm_ui_swampy` has no guard or exclusion clauses

**Observation.** Every other new definition carries a `num_modifiers` guard and at least one exclusion or self-exclusion. `pm_ui_swampy` declares only two `add` clauses.

**Consequences.** It can be assigned to a planet that already carries one or more modifiers, and it has no self-exclusion, so duplication depends entirely on whether the engine prevents re-selecting an already-assigned modifier.

**Possible readings.** A deliberate soft limit, allowing swamps to layer onto otherwise-modified worlds; or an omission introduced when the definition was written.

**Evidence needed.** The engine's own re-selection behaviour, which is not observable from the repository.

## Why accumulated modifiers are capped inconsistently

**Observation.** Seven definitions use `num_modifiers > 1`, `pm_ui_microorganisms` uses `num_modifiers > 2`, and `pm_ui_swampy` uses none, while `pm_null` treats increasing counts as a reason to *increase* its own weight through three escalating `factor = 3` clauses.

**Consequences.** The effective cap depends on which modifier is being considered, and `pm_null` becomes a progressively more likely outcome as a planet accumulates modifiers.

**Possible readings.** Microorganisms is permitted a third slot because it has no effect and is meant to be visible. `pm_null` may function as a budget-balancer. Neither reading is stated anywhere.

## Whether `pm_null` should carry all three `factor = 3` clauses

**Observation.** The three clauses test `num_modifiers > 2`, `> 1`, and `> 0` respectively, and all three can apply simultaneously.

**Consequence.** On a planet with three or more modifiers, the effective weight is $275 \times 3 \times 3 \times 3 = 7425$, which dwarfs every other candidate.

**Possible readings.** Intentional, to make slot-filling outcomes dominate on crowded planets; or the clauses were written incrementally and their compounding was not intended.

## Whether the vanilla originals are still reachable

**Observation.** The override file declares new `pm_ui_*` keys bound to host effect keys, and several redefinitions exclude the vanilla original (`pm_hazardous_weather`, `pm_tidal_locked`, `pm_asteroid_impacts`, `pm_wild_storms`, `pm_low_gravity`, `pm_high_gravity`, `pm_unstable_tectonics`, plus the magnetic pair). But whether the host's own definitions remain in the candidate pool at all, and whether the exclusions therefore matter, depends on key-merging behaviour the repository cannot observe.

**Consequence.** If the host definitions remain eligible, the mod reduces their effective probability rather than replacing them, which changes the observed distribution in a way the repository does not document.

## Which modifier actually wins for `pm_hazardous_weather` and its siblings

**Observation.** Both `pm_ui_hazardous_weather` and a hypothetical host-side `pm_hazardous_weather` would apply the same effect if both are eligible, and both are excluded by the mod's own definition. Whether the host's definition is superseded, shadowed, or coexists is not determinable from the repository.

## `pm_ui_microorganisms` configures no effect but is marked important

**Observation.** The block sets `important = yes` and declares no effect keys, so it consumes a modifier slot and presents prominently while changing nothing.

**Possible reading.** A narrative element, analogous to `pm_null` in structure but visible. Its relaxed accumulation guard is consistent with that reading.

## Naming diverges between the mod title and its identifiers

**Observation.** The mod is titled *Universum Infinitum - Planet Modifiers*, while identifiers use a `ui_` element and the content directory is `ui-planet-modifiers`, which originate from the repository name.

**Possible reading.** Inherited from the earlier combined repository, and deliberately retained because renaming identifiers would break save compatibility. Recorded in [design-decisions.md](./design-decisions.md).

## `supported_version` is unverified

**Observation.** Both descriptors declare `2.6.*`, which predates the current Stellaris release line by many versions.

**Consequence.** The compatibility claim is an assertion, not a verified fact. A contributor cannot tell from the repository whether the definitions still behave as intended on any given version, nor which vanilla identifiers may since have been renamed.

## Distribution status is inconsistent across channels

**Observation.** The Steam Workshop listing is titled with a `[DISCONTINUED]` prefix, while the repository remains public with a published GitHub release and an active funding configuration.

**Consequence.** A reader cannot determine from the repository whether the mod is actively maintained, superseded, or merely no longer updated on one channel.

## Exclusions reference other mods' identifiers

**Observation.** Conditions reference `pm_titanic_life`, `pm_lush`, and `pm_bleak`, which are not vanilla identifiers defined in this repository.

**Consequence.** If those identifiers belong to other mods, then some conditions silently never fire for players without those mods. Whether they are host-provided, other-mod-provided, or originally vanilla and since removed is not determinable from the repository.

## Whether `is_planet_class` with unquoted identifiers is intentional

**Observation.** Early definitions quote the class identifier; the gravity redefinitions use the bare form.

**Consequence.** None observed; the parser accepts both. Recorded because a future contributor might "normalise" the inconsistency without knowing it is harmless.

## The gravity exclusions are one-directional against unstable tectonics

**Observation.** `pm_ui_unstable_tectonics` excludes `pm_ui_high_gravity` and `pm_high_gravity`, but high gravity does not exclude unstable tectonics, and tectonics therefore has no counterpart in the gravity lists.

**Consequence.** The combination is prevented by the accumulation guard rather than by a declared conflict, so the intent is satisfied but through a different mechanism than the other families use.

## Related documentation

- [design-decisions.md](./design-decisions.md) for the decisions these items follow from
- [invariants.md](./invariants.md) for the constraints they affect
- [concurrency-and-scheduling.md](./concurrency-and-scheduling.md) for the re-selection question
