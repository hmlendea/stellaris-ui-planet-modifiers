# Component: planet modifier definitions

## Responsibility

Declare the ten new planet modifiers and the relative probability with which the engine may assign each to a generated planet.

## Location

`ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers.txt` — a single file containing ten top-level `pm_ui_*` definitions, ordered alphabetically by key.

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

### `pm_ui_dense_atmosphere` → `ui_dense_atmosphere`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_continental`, `pc_ocean`, `pc_savannah`, `pc_tropical` | Base weight 10 on the wettest, most habitable classes |
| `factor = 2` when `has_planet_modifier = "pm_ui_swampy"` | Doubled on swampy worlds |
| `factor = 0` when the planet already has `pm_ui_dense_atmosphere`, `pm_ui_extreme_temperatures`, or `pm_ui_thin_atmosphere` | Excludes the duplicate and the atmosphere-composition overlaps |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_extreme_temperatures` → `ui_extreme_temperatures`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_alpine`, `pc_arid`, `pc_desert`, `pc_nuked` | Base weight 10 on the hottest classes |
| `add = 5` on `pc_barren`, `pc_barren_cold` | Base weight 5 on barren worlds |
| `factor = 2` when `has_planet_modifier = "pm_bleak"` | Doubled on bleak worlds |
| `factor = 0` when the planet already has `pm_tidal_locked`, `pm_ui_dense_atmosphere`, `pm_ui_extreme_temperatures`, or `pm_ui_thin_atmosphere` | Excludes the duplicate, the atmosphere-composition overlaps, and the tidal-lock overlap |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_geological_instability` → `ui_geological_instability`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_volcanic` | Base weight 10 on volcanic worlds |
| `add = 5` on `pc_alpine`, `pc_arid`, `pc_desert` | Base weight 5 elsewhere |
| `factor = 0` when the planet already has `pm_abundant_geothermal_activity`, `pm_ui_geological_instability`, `pm_ui_superdense_core`, or `pm_unstable_tectonics` | Excludes the duplicate, the core/instability conflict, and the vanilla tectonic and geothermal concepts |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_no_magnetic_field` → `ui_no_magnetic_field`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_barren`, `pc_barren_cold` | Base weight 10 on barren worlds |
| `factor = 2` when `has_planet_modifier = "pm_low_gravity"` | Doubled on low-gravity worlds |
| `factor = 0` when any of `pm_strong_magnetic_field`, `pm_ui_no_magnetic_field`, `pm_ui_radioactive`, `pm_weak_magnetic_field` is present | Excludes the magnetic-field family |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_radioactive` → `ui_radioactive`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_nuked` | Base weight 10 on irradiated worlds |
| `add = 5` on `pc_barren`, `pc_barren_cold` | Base weight 5 on barren worlds |
| `factor = 0` when the planet already has `pm_ui_no_magnetic_field`, `pm_ui_radioactive`, or `pm_ui_toxic_soil` | Excludes the duplicate, the magnetic overlap, and the soil-concept overlap |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_salty` → `ui_salty`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_barren` | Base weight 10 on barren worlds |
| `add = 5` on `pc_arid`, `pc_barren_cold`, `pc_desert`, `pc_ocean` | Base weight 5 elsewhere |
| `factor = 0` when the planet already has `pm_ui_salty`, `pm_ui_swampy`, or `pm_ui_toxic_soil` | Excludes the soil-quality trio |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_superdense_core` → `ui_superdense_core`

| Clause | Effect |
|--------|--------|
| `add = 1` on `pc_barren`, `pc_molten`, `pc_volcanic` | Rare base weight 1 on heavy, dense worlds |
| `factor = 0` when the planet already has `pm_ui_geological_instability` or `pm_ui_superdense_core` | Excludes the duplicate and the core/instability conflict |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_swampy` → `ui_swampy`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_tropical` | Base weight 10 on tropical worlds |
| `add = 5` on `pc_ocean` | Base weight 5 on ocean worlds |
| `factor = 0` when the planet already has `pm_ui_salty`, `pm_ui_swampy`, or `pm_ui_toxic_soil` | Excludes the soil-quality trio |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_thin_atmosphere` → `ui_thin_atmosphere`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_barren`, `pc_barren_cold`, `pc_nuked` | Base weight 10 on airless-world classes |
| `add = 5` on `pc_arid`, `pc_desert` | Base weight 5 on dry worlds |
| `factor = 0` when the planet already has `pm_bleak`, `pm_ui_dense_atmosphere`, `pm_ui_extreme_temperatures`, or `pm_ui_thin_atmosphere` | Excludes the duplicate, the atmosphere-composition overlaps, and the vanilla bleak concept |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_toxic_soil` → `ui_toxic_soil`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_barren`, `pc_nuked` | Base weight 10 on irradiated, lifeless worlds |
| `add = 5` on `pc_volcanic` | Base weight 5 on volcanic worlds |
| `factor = 0` when the planet already has `pm_ui_radioactive`, `pm_ui_salty`, `pm_ui_swampy`, or `pm_ui_toxic_soil` | Excludes the duplicate, the soil-quality trio, and the radioactive overlap |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

All ten definitions carry an accumulation guard and at least one exclusion.

## Weight calibration

Vanilla v4.5 uses `add = 10` for a common class outcome, `add = 5` for an uncommon one, and `add = 1` for a rare one. The new definitions follow the same scale, so a `add = 10` entry competes on equal terms with `pm_low_gravity` or `pm_titanic_life` on the same class rather than dominating the pool.

## Interaction matrix

Read each cell as: the row's modifier, when already assigned, causes the column's modifier to be excluded.

| Already assigned ↓ excludes → | dense atm. | extreme temps | geo. instability | no magnetic field | radioactive | salty | superdense core | swampy | thin atm. | toxic soil |
|-------------------------------|-----------|---------------|--------------------|-------------------|-------------|-------|-----------------|--------|-----------|-----------|
| `pm_ui_dense_atmosphere` | yes | yes | no | no | no | no | no | no | yes | no |
| `pm_ui_extreme_temperatures` | yes | yes | no | no | no | no | no | no | yes | no |
| `pm_ui_geological_instability` | no | no | yes | no | no | no | yes | no | no | no |
| `pm_ui_no_magnetic_field` | no | no | no | yes | yes | no | no | no | no | no |
| `pm_ui_radioactive` | no | no | no | yes | yes | no | no | no | no | yes |
| `pm_ui_salty` | no | no | no | no | no | yes | no | yes | no | yes |
| `pm_ui_superdense_core` | no | no | yes | no | no | no | yes | no | no | no |
| `pm_ui_swampy` | no | no | no | no | no | yes | no | yes | no | yes |
| `pm_ui_thin_atmosphere` | yes | yes | no | no | no | no | no | no | yes | no |
| `pm_ui_toxic_soil` | no | no | no | no | yes | yes | no | yes | no | yes |

Several mutually exclusive groups exist. `pm_ui_dense_atmosphere`, `pm_ui_extreme_temperatures`, and `pm_ui_thin_atmosphere` form the atmosphere-composition trio; `pm_ui_salty`, `pm_ui_swampy`, and `pm_ui_toxic_soil` form the soil-quality trio. `pm_ui_geological_instability` and `pm_ui_superdense_core` form a symmetric pair, since a stable and a restless crust are incompatible. `pm_ui_radioactive` joins the magnetic family through `pm_ui_no_magnetic_field` and additionally excludes `pm_ui_toxic_soil`. Every mod-to-mod conflict is symmetric. Four exclusions against vanilla keys are one-sided, because those vanilla keys are not re-declared: `pm_ui_extreme_temperatures` against `pm_tidal_locked`, `pm_ui_thin_atmosphere` against `pm_bleak`, and `pm_ui_geological_instability` against `pm_unstable_tectonics` and `pm_abundant_geothermal_activity`. Each of those vanilla modifiers can still be assigned after the mod's modifier occupies the slot, which the accumulation guard prevents in practice.

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
