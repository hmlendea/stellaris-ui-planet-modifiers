# Component: planet modifier definitions

## Responsibility

Declare the four new planet modifiers and the relative probability with which the engine may assign each to a generated planet.

## Location

`ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers.txt` — a single file containing four top-level `pm_ui_*` definitions.

## Structure

Every definition has the same shape:

```text
pm_ui_<name> = {
    spawn_chance = {
        modifier = { ...conditions...; add = N }    # zero or more weight clauses
        modifier = { ...conditions...; factor = N } # zero or more multiplier clauses
        modifier = { num_modifiers > N; factor = 0 } # at most one accumulation guard
    }

    modifier = "ui_<name>"
}
```

Only the final `modifier` string and the `spawn_chance` block are structurally required; the number and order of internal clauses is free.

## Per-modifier specification

### `pm_ui_extreme_temperatures` → `ui_extreme_temperatures`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_desert`, `pc_arid`, `pc_alpine`, `pc_nuked` | Base weight 10 on the hottest classes |
| `add = 5` on `pc_barren`, `pc_barren_cold` | Base weight 5 on barren worlds |
| `factor = 2` when `has_planet_modifier = "pm_bleak"` | Doubled on bleak worlds |
| `factor = 0` when the planet already has `pm_ui_extreme_temperatures` or `pm_tidal_locked` | Excludes the duplicate and the tidal-lock overlap |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_no_magnetic_field` → `ui_no_magnetic_field`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_barren`, `pc_barren_cold` | Base weight 10 on barren worlds |
| `factor = 2` when `has_planet_modifier = "pm_low_gravity"` | Doubled on low-gravity worlds |
| `factor = 0` when any of `pm_ui_no_magnetic_field`, `pm_weak_magnetic_field`, `pm_strong_magnetic_field` is present | Excludes the magnetic-field family |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_salty` → `ui_salty`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_barren` | Base weight 10 on barren worlds |
| `add = 5` on `pc_barren_cold`, `pc_desert`, `pc_arid`, `pc_ocean` | Base weight 5 elsewhere |
| `factor = 0` when the planet already has `pm_ui_salty` or `pm_ui_swampy` | Excludes the water-quality conflict |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_swampy` → `ui_swampy`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_tropical` | Base weight 10 on tropical worlds |
| `add = 5` on `pc_ocean` | Base weight 5 on ocean worlds |
| `factor = 0` when the planet already has `pm_ui_swampy` or `pm_ui_salty` | Excludes the water-quality conflict |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

All four definitions carry an accumulation guard and at least one exclusion.

## Weight calibration

Vanilla v4.5 uses `add = 10` for a common class outcome, `add = 5` for an uncommon one, and `add = 1` for a rare one. The new definitions follow the same scale, so a `add = 10` entry competes on equal terms with `pm_low_gravity` or `pm_titanic_life` on the same class rather than dominating the pool.

## Interaction matrix

Read each cell as: the row's modifier, when already assigned, causes the column's modifier to be excluded.

| Already assigned ↓ excludes → | extreme temps | no magnetic field | salty | swampy |
|-------------------------------|---------------|-------------------|-------|--------|
| `pm_ui_extreme_temperatures` | yes | no | no | no |
| `pm_ui_no_magnetic_field` | no | yes | no | no |
| `pm_ui_salty` | no | no | yes | yes |
| `pm_ui_swampy` | no | no | yes | yes |

Every conflict is symmetric, so no pair depends on evaluation order. `pm_ui_extreme_temperatures` additionally excludes, and is excluded by, the vanilla `pm_tidal_locked` through the override file.

## Dependants

- [components/static-modifiers.md](./static-modifiers.md) — the `modifier` field of each definition must equal a static modifier key.
- [components/vanilla-spawn-overrides.md](./vanilla-spawn-overrides.md) — the magnetic exclusions here name the two vanilla keys redefined there.

## Failure behaviour

| Failure | Observable result |
|---------|-------------------|
| Definition key duplicated in the same file | One definition wins; the other never spawns |
| `modifier` field misspelled | The modifier can be assigned but has no effect; the localisation name still resolves |
| Condition references an identifier removed by a host update | The clause becomes unevaluable; the engine may treat it as false, changing weights silently |
| `factor = 0` exclusion omitted | Both mutually contradictory modifiers can be assigned to one planet |
| Malformed block syntax | The definition is rejected and the modifier never appears |

## Change impact

Editing any weight in this file changes the effective probability of every other candidate, because the engine selects from relative weights. Adding a modifier requires editing the other three data areas as well. See [invariants.md](../invariants.md) and [change-guide.md](../change-guide.md).

## Related documentation

- [flows/planet-modifier-selection.md](../flows/planet-modifier-selection.md) for evaluation order
- [components/vanilla-spawn-overrides.md](./vanilla-spawn-overrides.md) for the companion file
- [data-model.md](../data-model.md) for the record definition
