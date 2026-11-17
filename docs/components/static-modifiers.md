# Component: static modifiers

## Responsibility

Define what each new modifier does to a planet, and bind it to the icon that represents it in the interface.

## Location

`ui-planet-modifiers/common/static_modifiers/ui-planet-modifiers_static_modifiers.txt` — eight top-level `ui_*` blocks.

## Structure

```text
ui_<name> = {
    <effect key> = <numeric value>   # zero or more
    icon = "gfx/interface/icons/planet_modifiers/pm_ui_<name>.dds"
    icon_frame = <integer>
    important = yes                  # optional
}
```

`icon` and `icon_frame` appear in every block. Effect keys and `important` are optional.

## Per-modifier specification

### `ui_twilight`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.15 |
| `pop_happiness` | −0.05 |

Presentation: `icon_frame = 3`, no `important`.

### `ui_extreme_temperatures`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.10 |
| `pop_happiness` | −0.05 |

Presentation: `icon_frame = 3`, no `important`.

### `ui_no_magnetic_field`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.15 |
| `pop_growth_speed` | −0.15 |

Presentation: `icon_frame = 3`, no `important`.

### `ui_salty`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.15 |
| `planet_jobs_food_produces_mult` | −0.30 |

Presentation: `icon_frame = 3`, no `important`. This block carries the largest negative modifier in the set.

### `ui_swampy`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.05 |
| `planet_jobs_food_produces_mult` | −0.10 |

Presentation: `icon_frame = 3`, no `important`. This block carries the mildest penalties in the set.

### `ui_microorganisms`

| Effect | Value |
|--------|-------|
| *(none)* | — |

Presentation: `icon_frame = 1`, `important = yes`. This is the only block with no effect keys and the only one flagged `important`. It is therefore a pure presentation entry: the modifier exists in the pool, is presented prominently, and changes no planetary statistic.

### `ui_giant_earthworms`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.10 |
| `pop_happiness` | −0.05 |
| `planet_jobs_society_research_produces_mult` | +0.20 |

Presentation: `icon_frame = 2`, no `important`.

### `ui_sea_monsters`

| Effect | Value |
|--------|-------|
| `pop_environment_tolerance` | −0.10 |
| `pop_happiness` | −0.05 |
| `planet_jobs_society_research_produces_mult` | +0.20 |

Presentation: `icon_frame = 2`, no `important`.

`ui_giant_earthworms` and `ui_sea_monsters` are numerically identical: both trade habitability and happiness for society research. Their differentiation lives entirely in the eligibility tier, not here.

## Effect coverage

| Effect key | Blocks | Values in use |
|-----------|-------|---------------|
| `pop_environment_tolerance` | 7 | −0.15, −0.10, −0.05 |
| `pop_happiness` | 4 | −0.05 |
| `pop_growth_speed` | 1 | −0.15 |
| `planet_jobs_food_produces_mult` | 2 | −0.30, −0.10 |
| `planet_jobs_society_research_produces_mult` | 2 | +0.20 |

Sign convention: every value is expressed as a decimal fraction, and the sign is explicit only where the value is positive. All effects except society research are penalties, so the mod's contribution is net-negative in aggregate and is intended as a flavour and difficulty layer rather than a power increase.

## Icon binding

Each `icon` value is a repository-relative path beginning `gfx/interface/icons/planet_modifiers/`. The path is resolved against the mod content directory, not the repository root, so the `ui-planet-modifiers/` prefix is absent from the value.

| Block | Icon reference | File exists |
|-------|----------------|-------------|
| `ui_twilight` | `pm_ui_twilight.dds` | Yes |
| `ui_extreme_temperatures` | `pm_ui_extreme_temperatures.dds` | Yes |
| `ui_no_magnetic_field` | `pm_ui_no_magnetic_field.dds` | Yes |
| `ui_salty` | `pm_ui_salty.dds` | Yes |
| `ui_swampy` | `pm_ui_swampy.dds` | Yes |
| `ui_microorganisms` | `pm_ui_microorganisms.dds` | Yes |
| `ui_giant_earthworms` | `pm_ui_giant_earthworms.dds` | Yes |
| `ui_sea_monsters` | `pm_ui_sea_monsters.dds` | Yes |

Every reference resolves, and every file in the icon directory is referenced, so the mapping is currently bijective.

## Must-reference host-owned effects

The override file binds nine definitions to host-owned effect keys. Those blocks are **not** declared here, which means the mod relies on the host to define:

`hazardous_weather`, `weak_magnetic_field`, `strong_magnetic_field`, `unstable_tectonics`, `tidal_locked`, `asteroid_impacts`, `wild_storms`, `low_gravity`, `high_gravity`.

If a host update renames any of these, the corresponding definition resolves to nothing. The repository would show no error; the modifier would simply have no effect.

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
