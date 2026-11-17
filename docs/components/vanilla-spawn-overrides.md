# Component: vanilla spawn overrides

## Responsibility

Re-declare the spawn chances of existing planet modifiers so that the eight new modifiers can participate in the same selection pool under conditions the mod controls. Re-declaration is necessary because the new modifiers must be able to exclude, and be excluded by, modifiers whose effects belong to the host.

## Location

`ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers-vanillas.txt` — one placeholder definition plus nine redefinitions.

## The override mechanism

A mod cannot add conditions to a host-owned definition. It can, however, declare a **new key** whose `modifier` field names the host's effect key. The engine then treats the new key as another candidate that applies that same effect, with the conditions the mod chose.

This is why every redefinition follows the pattern:

```text
pm_ui_<name> = {
    spawn_chance = { ...mod-owned conditions... }
    modifier = "<host-owned effect key>"
}
```

The consequence is that the host's original key (`pm_hazardous_weather`, for example) may still exist and still be eligible, unless the host's own definition is superseded and the vanilla condition excluded. The redefinitions do not exclude the original vanilla keys in every case, which is recorded below as an asymmetry.

## `pm_null`

`pm_null` is not a modifier in the ordinary sense. It declares `is_null = yes` and a high base weight:

| Clause | Effect |
|--------|--------|
| `value = 275` | Base weight 275 |
| `factor = 3` when `num_modifiers > 2` | Tripled |
| `factor = 3` when `num_modifiers > 1` | Tripled |
| `factor = 3` when `num_modifiers > 0` | Tripled |

All three clauses may apply simultaneously, so the effective weight rises steeply as a planet accumulates modifiers. The definition contains no effect key because `is_null = yes` marks it as occupying a slot without contributing an effect.

## Redefinitions that bind to host-owned effects

| Definition key | Bound effect key | Base `add` conditions | Extra multiplies | Exclusions |
|----------------|------------------|-----------------------|------------------|------------|
| `pm_ui_hazardous_weather` | `hazardous_weather` | `add = 10` on `pc_barren`, `pc_barren_cold` | none | self, `pm_hazardous_weather`; guard `num_modifiers > 1` |
| `pm_ui_weak_magnetic_field` | `weak_magnetic_field` | `add = 15` on `pc_barren`, `pc_barren_cold` | `factor = 2` with `pm_low_gravity` | all five magnetic identifiers; guard `num_modifiers > 1` |
| `pm_ui_strong_magnetic_field` | `strong_magnetic_field` | `add = 5` on `pc_barren`, `pc_barren_cold` | `factor = 2` with `pm_high_gravity` | all five magnetic identifiers; guard `num_modifiers > 1` |
| `pm_ui_unstable_tectonics` | `unstable_tectonics` | `add = 10` on `pc_barren`, `pc_barren_cold` | none | `pm_ui_high_gravity`, self, `pm_high_gravity`, `pm_unstable_tectonics`; guard `num_modifiers > 1` |
| `pm_ui_tidal_locked` | `tidal_locked` | `add = 1` on `pc_barren`, `pc_barren_cold` | `factor = 3` when `is_moon = yes` | self, `pm_tidal_locked`; guard `num_modifiers > 1` |
| `pm_ui_asteroid_impacts` | `asteroid_impacts` | `add = 10` on `pc_barren`, `pc_barren_cold` | none | self, `pm_asteroid_impacts`; guard `num_modifiers > 1` |
| `pm_ui_wild_storms` | `wild_storms` | `add = 10` on `pc_barren`, `pc_barren_cold` | none | self, `pm_wild_storms`; guard `num_modifiers > 1` |
| `pm_ui_low_gravity` | `low_gravity` | `add = 10` on `pc_barren`, `pc_barren_cold` | four size-based factors: 2 below 14, 2 below 12, 2 below 7, 0.5 above 19 | the four gravity identifiers; guard `num_modifiers > 1` |
| `pm_ui_high_gravity` | `high_gravity` | `add = 10` on `pc_barren`, `pc_barren_cold` | three size-based factors: 2 above 19, 2 above 22, 0.5 below 16, and 0 below 12 | the four gravity identifiers; guard `num_modifiers > 1` |

### Gravity: the only size-dependent behaviour

The two gravity definitions are the only place in the mod where `planet_size` participates:

| Definition | Weight increases | Weight decreases | Hard exclusion |
|------------|------------------|------------------|----------------|
| `pm_ui_low_gravity` | ×2 below 14, ×2 below 12, ×2 below 7 (compounding) | ×0.5 above 19 | none by size |
| `pm_ui_high_gravity` | ×2 above 19, ×2 above 22 (compounding) | ×0.5 below 16 | ×0 below 12 |

Because the multipliers are cumulative, small planets amplify low gravity up to eightfold, while large planets amplify high gravity up to fourfold, and the two definitions exclude each other and their vanilla counterparts. The `factor = 0` at `planet_size < 12` in the high-gravity definition makes high gravity impossible on the smallest worlds without a corresponding hard floor for low gravity.

### Reused exclusion sets

Two identifier sets are repeated verbatim across the file:

| Set | Identifier members | Appears in |
|-----|--------------------|------------|
| Magnetic family | `pm_ui_no_magnetic_field`, `pm_ui_weak_magnetic_field`, `pm_ui_strong_magnetic_field`, `pm_weak_magnetic_field`, `pm_strong_magnetic_field` | `pm_ui_no_magnetic_field` (additions file), `pm_ui_weak_magnetic_field`, `pm_ui_strong_magnetic_field` |
| Gravity family | `pm_ui_low_gravity`, `pm_ui_high_gravity`, `pm_low_gravity`, `pm_high_gravity` | `pm_ui_low_gravity`, `pm_ui_high_gravity` |

Neither set is factored into a shared construct, because the format offers none. Adding a member to a family therefore requires editing every list that mentions the family. This is recorded as a design constraint in [invariants.md](../invariants.md).

## Asymmetries observed

| Asymmetry | Detail |
|-----------|--------|
| Vanilla originals not always excluded | `pm_ui_unstable_tectonics` excludes the vanilla `pm_unstable_tectonics`, but `pm_ui_hazardous_weather` excludes `pm_hazardous_weather` only through its self-list, and no redefinition excludes the vanilla original of `tidal_locked`, `asteroid_impacts`, `wild_storms`, `low_gravity`, or `high_gravity` under a distinct key — the vanilla key is present in each list where it exists. |
| Gravity excludes only gravity | `pm_ui_unstable_tectonics` excludes high gravity but high gravity does not exclude unstable tectonics, so the guard runs in one direction only. |
| Magnetic variants differ in precondition | The weak variant multiplies with `pm_low_gravity`; the strong variant multiplies with `pm_high_gravity`. |
| `pm_ui_tidal_locked` is moon-specific | It is the only definition using `is_moon = yes`, and its base weight of 1 is the lowest in either file. |

## Dependants

- `pm_ui_twilight` and `pm_ui_extreme_temperatures` reference `pm_ui_tidal_locked` in their exclusion lists.
- `pm_ui_no_magnetic_field` references `pm_ui_weak_magnetic_field` and `pm_ui_strong_magnetic_field`.
- `pm_ui_giant_earthworms` and `pm_ui_sea_monsters` reference `pm_ui_high_gravity`.
- `pm_ui_microorganisms` excludes `pm_ui_no_magnetic_field`.

Removing or renaming any key in this file therefore breaks conditions declared in the additions file.

## Failure behaviour

| Failure | Observable result |
|---------|-------------------|
| Redefinition omitted | The new modifier's exclusions in the additions file reference a non-existent key; the exclusion never fires and contradictory modifiers can coexist |
| Bound effect key misspelled | The definition resolves to no effect; the host's original modifier is unaffected |
| `pm_null` clause removed | The engine's modifier budget behaves differently; more or fewer slots remain available |

## Related documentation

- [components/planet-modifier-definitions.md](./planet-modifier-definitions.md) for the additions file
- [flows/planet-modifier-selection.md](../flows/planet-modifier-selection.md) for evaluation
- [invariants.md](../invariants.md) for the cross-file constraints
