# Component: static modifiers

## Responsibility

Define what each new modifier does to a planet, and bind it to the icon that represents it in the interface.

## Location

`ui-planet-modifiers/common/static_modifiers/ui-planet-modifiers_static_modifiers.txt` — four top-level `ui_*` blocks.

## Structure

```text
ui_<name> = {
    <effect key> = <numeric value>   # one or more
    icon = "gfx/interface/icons/planet_modifiers/pm_ui_<name>.dds"
    icon_frame = <integer>
}
```

`icon` and `icon_frame` appear in every block, and so does at least one effect key: every new modifier now has a mechanical effect.

`icon_frame` follows the vanilla convention: `3` for a net penalty, `2` for a mixed block with an offsetting bonus.

## Per-modifier specification

### `ui_extreme_temperatures`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.10 |
| `pop_happiness` | −0.05 |
| `planet_jobs_food_produces_mult` | −0.10 |

Presentation: `icon_frame = 3`.

### `ui_no_magnetic_field`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.10 |
| `logistic_growth_mult` | −0.10 |
| `planet_jobs_physics_research_produces_mult` | −0.10 |

Presentation: `icon_frame = 3`. The growth effect uses `logistic_growth_mult`, the current replacement for the removed `pop_growth_speed`, which the previous revision set to no effect.

### `ui_salty`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.05 |
| `planet_jobs_food_produces_mult` | −0.15 |
| `district_farming_max_add` | −2 |

Presentation: `icon_frame = 3`. This block carries the largest food penalty in the set.

### `ui_swampy`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.05 |
| `planet_jobs_food_produces_mult` | −0.10 |
| `planet_structures_cost_mult` | +0.10 |

Presentation: `icon_frame = 3`.

## Effect coverage

| Effect key | Blocks | Values in use |
|-----------|-------|---------------|
| `pop_environment_tolerance` | 4 | −0.10, −0.05 |
| `pop_happiness` | 1 | −0.05 |
| `logistic_growth_mult` | 1 | −0.10 |
| `planet_jobs_food_produces_mult` | 3 | −0.15, −0.10 |
| `planet_jobs_physics_research_produces_mult` | 1 | −0.10 |
| `district_farming_max_add` | 1 | −2 |
| `planet_structures_cost_mult` | 1 | +0.10 |

Sign convention: every value is expressed as a decimal fraction, and the sign is explicit only where the value is positive. Every magnitude sits inside the vanilla band for the comparable modifier: `hazardous_weather` and `bleak_planet` use a −0.10 habitability penalty, and `weak_magnetic_field` uses −0.05 on tolerance with −0.05 growth.

## Icon binding

Each `icon` value is a repository-relative path beginning `gfx/interface/icons/planet_modifiers/`. The path is resolved against the mod content directory, not the repository root, so the `ui-planet-modifiers/` prefix is absent from the value.

| Block | Icon reference | File exists |
|-------|----------------|-------------|
| `ui_extreme_temperatures` | `pm_ui_extreme_temperatures.dds` | Yes |
| `ui_no_magnetic_field` | `pm_ui_no_magnetic_field.dds` | Yes |
| `ui_salty` | `pm_ui_salty.dds` | Yes |
| `ui_swampy` | `pm_ui_swampy.dds` | Yes |

Every reference resolves, and every file in the icon directory is referenced, so the mapping is bijective.

## Host-owned effects referenced through redefinitions

The definition file redefines two vanilla keys and therefore relies on the host to keep their effect keys: `weak_magnetic_field` and `strong_magnetic_field`. Those effects are resolved from the host's static modifier set, not from this repository.

If the host renames any of them, the corresponding definition resolves to nothing. The repository shows no error; the modifier simply has no effect.

## Host-owned effects referenced by other means

The new definitions also test host keys that the mod neither defines nor binds, such as `pm_low_gravity`, `pm_high_gravity`, `pm_bleak`, and `pm_tidal_locked`. A rename changes eligibility silently and shifts the weights of every candidate that references them.

## Localisation dependency

No block names its localisation key. The engine resolves `ui_<name>` and `ui_<name>_desc` by convention, so each key in this file must have both entries in the localisation file. See [components/localisation.md](./localisation.md).

## Failure behaviour

| Failure | Observable result |
|---------|-------------------|
| Effect key misspelled | The effect is ignored; the modifier still appears and is still localised |
| `icon` path wrong | A missing or placeholder icon is rendered; the modifier remains active |
| `icon_frame` out of range | The wrong frame of the atlas is shown, or nothing is shown |
| Block key diverges from the definition's `modifier` field | Either the definition resolves to no effect, or the block becomes unreachable |
| Block omitted | The modifier can be assigned with no effect and possibly no name |

## Change impact

Changing an effect value changes gameplay balance for every game in which the modifier spawns. Changing a key breaks the binding from the definition file and the name convention from the localisation file simultaneously. Removing a block leaves its definition orphaned. See [invariants.md](../invariants.md) and [change-guide.md](../change-guide.md).

## Related documentation

- [components/planet-modifier-definitions.md](./planet-modifier-definitions.md) for the entries that bind here
- [components/icon-assets.md](./icon-assets.md) for the referenced textures
- [data-model.md](../data-model.md) for the record definition
