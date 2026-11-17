# Component: planet modifier definitions

## Responsibility

Declare the eight new planet modifiers and the relative probability with which the engine may assign each to a generated planet.

## Location

`ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers.txt` — a single file containing eight top-level `pm_ui_*` definitions.

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

### `pm_ui_twilight` → `ui_twilight`

| Clause | Effect |
|--------|--------|
| `add = 2` when planet class is any of `pc_arctic`, `pc_tundra`, `pc_alpine`, `pc_desert`, `pc_frozen`, `pc_barren`, `pc_barren_cold` | Base weight 2 on those classes |
| `factor = 2` when `has_planet_modifier = "pm_bleak"` | Doubled on bleak worlds |
| `factor = 4` when the planet already has `pm_ui_tidal_locked` or `pm_tidal_locked` | Quadrupled on tidally locked worlds |
| `factor = 0` when `num_modifiers > 1` | Excluded once more than one modifier is present |

### `pm_ui_extreme_temperatures` → `ui_extreme_temperatures`

| Clause | Effect |
|--------|--------|
| `add = 5` on `pc_alpine`, `pc_desert`, `pc_arid`, `pc_nuked`, `pc_barren`, `pc_barren_cold` | Base weight 5 on those classes |
| `factor = 2` when `has_planet_modifier = "pm_bleak"` | Doubled on bleak worlds |
| `factor = 0` when the planet already has `pm_ui_twilight`, `pm_ui_tidal_locked`, or `pm_tidal_locked` | Excludes the twilight and tidal-lock combinations |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

Note the asymmetry with `pm_ui_twilight`: twilight multiplies when a tidal lock is present, while extreme temperatures excludes the tidal-lock pair outright.

### `pm_ui_no_magnetic_field` → `ui_no_magnetic_field`

| Clause | Effect |
|--------|--------|
| `add = 20` on `pc_barren`, `pc_barren_cold` | High base weight on barren worlds |
| `factor = 2` when `has_planet_modifier = "pm_low_gravity"` | Doubled on low-gravity worlds |
| `factor = 0` when any of `pm_ui_no_magnetic_field`, `pm_ui_weak_magnetic_field`, `pm_ui_strong_magnetic_field`, `pm_weak_magnetic_field`, `pm_strong_magnetic_field` is present | Excludes the whole magnetic-field family, both new and vanilla |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_salty` → `ui_salty`

| Clause | Effect |
|--------|--------|
| `add = 30` on `pc_barren` | Highest single base weight in the file |
| `add = 5` on `pc_barren_cold` | Moderate weight |
| `add = 10` on `pc_desert`, `pc_arid`, `pc_ocean` | Moderate weight on arid and wet classes |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

`pm_ui_salty` declares no exclusion conditions at all, so its only interaction constraint is the accumulation guard.

### `pm_ui_microorganisms` → `ui_microorganisms`

| Clause | Effect |
|--------|--------|
| `add = 5` on `pc_barren`, `pc_barren_cold` | Base weight 5 |
| `factor = 0` when `has_planet_modifier = "pm_ui_no_magnetic_field"` | Excluded when the planet has no magnetic field |
| `factor = 0` when the system's star class is `sc_black_hole`, `sc_neutron_star`, or `sc_pulsar` | Excluded around hostile stellar remnants |
| `factor = 0` when `num_modifiers > 2` | Relaxed accumulation guard permitting a third modifier |

This is the only definition with a system-scope condition, expressed as `solar_system = { OR = { ... } }`, and the only one whose accumulation guard uses the threshold `2` rather than `1`.

### `pm_ui_giant_earthworms` → `ui_giant_earthworms`

| Clause | Effect |
|--------|--------|
| `add = 10` on `pc_desert`, `pc_arid`, `pc_savannah` | Base weight 10 |
| `factor = 2` when `has_planet_modifier = "pm_lush"` | Doubled on lush worlds |
| `factor = 2` when the planet has `pm_ui_high_gravity` or `pm_high_gravity` | Doubled under high gravity |
| `factor = 0` when the planet has `pm_ui_giant_earthworms` or `pm_titanic_life` | Excludes itself and titanic life |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_sea_monsters` → `ui_sea_monsters`

| Clause | Effect |
|--------|--------|
| `add = 5` on `pc_continental` | Base weight 5 |
| `add = 10` on `pc_ocean` | Heavier weight on ocean worlds |
| `factor = 2` when the planet has `pm_ui_high_gravity` or `pm_high_gravity` | Doubled under high gravity |
| `factor = 0` when the planet has `pm_ui_sea_monsters` or `pm_titanic_life` | Excludes itself and titanic life |
| `factor = 0` when `num_modifiers > 1` | Accumulation guard |

### `pm_ui_swampy` → `ui_swampy`

| Clause | Effect |
|--------|--------|
| `add = 3` on `pc_ocean` | Base weight 3 |
| `add = 6` on `pc_tropical` | Base weight 6 |

`pm_ui_swampy` has **no** `factor` clauses: no exclusions, no multipliers, and no accumulation guard. It is therefore the only new modifier that can be assigned alongside an already-present modifier without limit, and the only one that never excludes a conflicting candidate.

## Interaction matrix

Read each cell as: the row's modifier, when already assigned, causes the column's modifier to be excluded.

| Already assigned ↓ excludes → | twilight | extreme temps | no magnetic field | microorganisms | earthworms | sea monsters |
|-------------------------------|----------|---------------|-------------------|----------------|------------|--------------|
| `pm_ui_twilight` | — | yes | no | no | no | no |
| `pm_ui_extreme_temperatures` | yes | — | no | no | no | no |
| `pm_ui_no_magnetic_field` | no | no | yes (self, family) | yes | no | no |
| `pm_ui_microorganisms` | no | no | no | no | no | no |
| `pm_ui_giant_earthworms` | no | no | no | no | yes (self, plus titanic life) | no |
| `pm_ui_sea_monsters` | no | no | no | no | no | yes (self, plus titanic life) |

| Already assigned ↓ excludes → | tidal locked | vanilla magnetic family | titanic life | vanilla magnetics |
|-------------------------------|--------------|-------------------------|--------------|-------------------|
| `pm_ui_twilight` | no (multiplies instead) | no | no | no |
| `pm_ui_extreme_temperatures` | yes | no | no | no |
| `pm_ui_no_magnetic_field` | no | yes | no | yes |
| `pm_ui_giant_earthworms` | no | no | yes | no |
| `pm_ui_sea_monsters` | no | no | yes | no |

The matrix reveals three properties worth noting:

- **No self-exclusion for the vanilla counterparts.** `pm_ui_no_magnetic_field` excludes itself and both new siblings, but the *vanilla* keys it excludes are `pm_weak_magnetic_field` and `pm_strong_magnetic_field`. Since the override file declares `pm_ui_weak_magnetic_field` and `pm_ui_strong_magnetic_field` as the spawn-chance owners, the vanilla keys may never be assigned at all; the exclusion then operates on the new counterparts, which do not exclude `pm_ui_no_magnetic_field` in return.
- **Asymmetry in the magnetic family.** `pm_ui_no_magnetic_field` excludes the weak and strong variants, and both weak and strong variants exclude all three, so the family is mutually exclusive in the relevant directions. The two gravity and two magnetic-variant groups are the only places where exclusion lists repeat a five-identifier set.
- **`pm_ui_twilight` multiplies on a tidal lock while `pm_ui_extreme_temperatures` excludes it.** The two therefore cannot coexist, but the reason differs per branch rather than being a symmetric exclusion.

## Dependants

- [components/static-modifiers.md](./static-modifiers.md) — the `modifier` field of each definition must equal a static modifier key.
- [components/vanilla-spawn-overrides.md](./vanilla-spawn-overrides.md) — several exclusion lists here name keys declared in the override file.

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
