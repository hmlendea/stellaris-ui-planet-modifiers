# Component: vanilla spawn overrides

## Responsibility

Re-declare a small number of existing planet modifiers so that the mod's own modifiers remain mutually exclusive with them. Re-declaration is required because a mod cannot append conditions to a definition the host owns.

## Location

`ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers-vanillas.txt` — two redefinitions, each under its own vanilla key.

## The override mechanism

A mod file that declares a key the host already declares **replaces** that definition, because mod files load after the host's `00_planet_modifiers.txt`. The mod therefore declares `pm_weak_magnetic_field` and `pm_strong_magnetic_field`, each binding `modifier` to the matching vanilla effect key, and reproduces the v4.5 conditions while adding only the exclusion against `pm_ui_no_magnetic_field`.

An earlier revision instead introduced parallel `pm_ui_*` keys bound to the host's effect keys. That left the vanilla definition in place, so the same effect could be selected twice through two different keys. Replacing the definition removes that duplication.

## Redefinitions

| Definition key | Bound effect key | Change relative to v4.5 |
|----------------|------------------|--------------------------|
| `pm_strong_magnetic_field` | `strong_magnetic_field` | Adds `factor = 0` when `pm_ui_no_magnetic_field` is present |
| `pm_weak_magnetic_field` | `weak_magnetic_field` | Adds `factor = 0` when `pm_ui_no_magnetic_field` is present |

The two definitions are listed alphabetically, matching the file order. Every other clause in each block is a copy of the vanilla v4.5 definition, including weights, size multipliers, the moon multiplier, the Gaia exclusion, and the accumulation guard. The only behavioural change is the added exclusion.

### Magnetic family

All vanilla magnetic modifiers are mutually exclusive. The mod adds `pm_ui_no_magnetic_field` as a fourth member: both vanilla variants exclude it, and it excludes both of them, so the family stays symmetric.

### Tidal locking is not overridden

Vanilla `tidal_locked` already describes one hemisphere in permanent daylight and the other in permanent darkness. The mod adds no modifier covering that concept, so `pm_tidal_locked` is left untouched. Its own exclusion of `pm_abundant_geothermal_activity` and `pc_volcanic` therefore continues to behave exactly as in the base game.

## Deliberately not overridden

An earlier revision also redefined `hazardous_weather`, `unstable_tectonics`, `asteroid_impacts`, `wild_storms`, `low_gravity`, and `high_gravity` under parallel `pm_ui_*` keys. Those overrides were removed because:

| Modifier | Reason for removal |
|----------|--------------------|
| `hazardous_weather` | The mod adds no weather modifier, so the duplicate candidate distorted the vanilla pool for no benefit |
| `unstable_tectonics` | No mod modifier interacted with it at the time; `pm_ui_geological_instability` now excludes it one-sidedly without a re-declaration |
| `asteroid_impacts` | Same: no mod modifier interacts with it |
| `wild_storms` | Same |
| `low_gravity` | The vanilla definition already covers every habitable class; the parallel key added a second gravity candidate |
| `high_gravity` | Same |
| `tidal_locked` | Its only added exclusion referenced the removed `pm_ui_twilight` |

Gravity is now governed solely by the vanilla definition. The mod's own definitions still test `pm_low_gravity` and `pm_high_gravity` when weighting their spawns, which is the intended interaction.

### One-sided exclusions left unclosed

`pm_ui_geological_instability` excludes `pm_unstable_tectonics` and `pm_abundant_geothermal_activity` without either vanilla key being re-declared. The exclusions are therefore one-sided: the vanilla modifier does not exclude the mod's, so the vanilla one can still be assigned after the mod's. The accumulation guard (`num_modifiers > 1`) prevents the two coexisting in practice. Closing the pair would require re-declaring both vanilla keys, which would copy two more host definitions forward for a conflict the guard already prevents.

`pm_null` is not overridden. The vanilla definition applies unchanged.

## Dependants

- `pm_ui_no_magnetic_field` excludes `pm_weak_magnetic_field` and `pm_strong_magnetic_field`, and each of those excludes it in return.

Removing or renaming any key in this file breaks conditions declared in the additions file.

## Failure behaviour

| Failure | Observable result |
|---------|-------------------|
| Redefinition omits a vanilla clause | Vanilla odds change for planet classes the mod intended to leave untouched |
| Bound effect key misspelled | The definition resolves to no effect |
| Exclusion omitted | A contradictory pair can coexist |
| A later host version changes a clause the file copies | The copy silently reverts the host change |

The last row is the maintenance cost of copying host definitions, and it is why the override set is limited to what mutual exclusivity requires. Re-copy the host clause whenever the target game version changes.

## Related documentation

- [components/planet-modifier-definitions.md](./planet-modifier-definitions.md) for the additions file
- [flows/planet-modifier-selection.md](../flows/planet-modifier-selection.md) for evaluation
- [invariants.md](../invariants.md) for the cross-file constraints
