# Ambiguities and open questions

Unresolved items identified during documentation, with the evidence that leaves each open. None is presented as a defect; each is a place where the repository is silent or where a reasonable reader could reach two conclusions.

## Why `pm_ui_swampy` used to have no guard or exclusion clauses

**Observation.** An earlier revision gave `pm_ui_swampy` only two `add` clauses, with no `num_modifiers` guard, no self-exclusion, and no conflict exclusion. Every other definition carried both.

**Resolution.** The definition now declares the standard `num_modifiers > 1` guard and a mutual exclusion against `pm_ui_salty`. The asymmetry is therefore resolved rather than open.

## Why accumulated modifiers were capped inconsistently

**Observation.** An earlier revision used `num_modifiers > 1` for seven definitions, `num_modifiers > 2` for `pm_ui_microorganisms`, and no guard at all for `pm_ui_swampy`.

**Resolution.** Every definition now uses the vanilla `num_modifiers > 1` guard, so the cap no longer depends on which modifier is considered. Vanilla `pm_null` still treats increasing counts as a reason to increase its own weight through three escalating `factor = 3` clauses; that behaviour belongs to the host and is unchanged.

## Whether the vanilla originals are still reachable

**Observation.** An earlier revision declared parallel `pm_ui_*` keys bound to host effect keys, leaving the host's own definitions in the pool. Whether the host definitions remained eligible, and whether the exclusions therefore mattered, depended on key-merging behaviour the repository cannot observe.

**Resolution.** The override file now re-declares the vanilla keys themselves (`pm_weak_magnetic_field`, `pm_strong_magnetic_field`). Because mod files load after `00_planet_modifiers.txt`, the mod's definition replaces the host's, so no duplicate candidate exists. The remaining uncertainty is only whether a future game version changes the load order, which a single generation test will reveal.

## Naming diverges between the mod title and its identifiers

**Observation.** The mod is titled *Universum Infinitum - Planet Modifiers*, while identifiers use a `ui_` element and the content directory is `ui-planet-modifiers`, which originate from the repository name.

**Possible reading.** Inherited from the earlier combined repository, and deliberately retained because renaming identifiers would break save compatibility. Recorded in [design-decisions.md](./design-decisions.md).

## `supported_version` is unverified

**Observation.** Both descriptors declare `v4.5.*`, matching the release line against which the definitions were recalibrated.

**Consequence.** The compatibility claim is an assertion, not a verified fact. A contributor cannot tell from the repository whether the definitions still behave as intended on any given version, nor which vanilla identifiers may since have been renamed.

## Distribution status is inconsistent across channels

**Observation.** The Steam Workshop listing is titled with a `[DISCONTINUED]` prefix, while the repository remains public with a published GitHub release and an active funding configuration.

**Consequence.** A reader cannot determine from the repository whether the mod is actively maintained, superseded, or merely no longer updated on one channel.

## Exclusions reference host identifiers that are not defined here

**Observation.** Conditions reference `pm_titanic_life`, `pm_lush`, and `pm_bleak`, which the repository does not define.

**Consequence.** None. `pm_lush`, `pm_bleak`, `pm_titanic_life`, `pm_dangerous_wildlife`, `pm_docile_fauna`, `pm_rich_mircoflora`, `pm_low_gravity`, and `pm_high_gravity` are all vanilla v4.5 planet modifiers, verified against the installed game's `common/planet_modifiers/00_planet_modifiers.txt`. They are referenced but not defined here, which is the normal way a mod reads host state.

## Identifiers were once quoted inconsistently

**Observation.** Earlier revisions quoted class identifiers in some blocks and used the bare form in others.

**Resolution.** Every identifier is now quoted uniformly, including in the re-declared vanilla blocks. The parser accepts both forms, so this was cosmetic rather than functional.

## Related documentation

- [design-decisions.md](./design-decisions.md) for the decisions these items follow from
- [invariants.md](./invariants.md) for the constraints they affect
- [concurrency-and-scheduling.md](./concurrency-and-scheduling.md) for the re-selection question
