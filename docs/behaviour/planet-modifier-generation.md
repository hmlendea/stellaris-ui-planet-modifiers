# Behaviour: planet modifier generation

This document describes what a player observes, and why, when the mod is active during galaxy generation.

## Observation: additional modifiers appear on generated planets

The most direct behavioural change is that planets can carry any of four modifiers the base game does not provide: extreme temperatures, absence of a magnetic field, salinity, and swampland.

Why: the additions file declares a `spawn_chance` for each identifier, so each enters the candidate pool for every generated planet that satisfies its conditions.

## Observation: three new modifiers also cover barren worlds

The class conditions of three new modifiers include barren or cold-barren worlds:

| Modifier | Barren weight contribution |
|----------|---------------------------|
| `pm_ui_no_magnetic_field` | `add = 10` |
| `pm_ui_salty` | `add = 10` on barren, `add = 5` on cold barren |
| `pm_ui_extreme_temperatures` | `add = 5` |

Why: barren worlds have no habitability to protect, so these effects are thematically appropriate there. The weights match vanilla's `add = 10` common and `add = 5` uncommon tiers.

## Observation: modifiers cluster around thematically compatible worlds

| Modifier | Reinforcing condition | Multiplier |
|----------|----------------------|-----------|
| `pm_ui_extreme_temperatures` | Bleak | ×2 |
| `pm_ui_no_magnetic_field` | Low gravity | ×2 |

Why: the multipliers encode narrative compatibility, and because multipliers compound a bleak low-gravity barren world favours both.

## Observation: contradictory modifiers do not appear together

A planet does not carry both extreme temperatures and a tidal lock, nor two magnetic-field conditions, nor both salinity and swampland.

Why: each conflicting pair is separated by `factor = 0` exclusion clauses written symmetrically, so the outcome does not depend on assignment order. The tidal-lock pair is one-sided: `pm_ui_extreme_temperatures` excludes `pm_tidal_locked`, but the vanilla definition is no longer re-declared, so a tidal lock can still be rolled after extreme temperatures. The magnetic pair is the only one closed in both directions, through the override file.

## Observation: new modifiers never join an already-modified planet

All four new definitions carry a `num_modifiers > 1` guard, matching the vanilla convention.

Why: the guard limits how many modifiers a planet accumulates. An earlier revision exempted swampy and relaxed the guard for microorganisms; both exceptions were removed so the cap is uniform.

## Observation: gravity keeps its vanilla behaviour

Low gravity favours small worlds and high gravity favours large worlds, with high gravity impossible below size 12. The mod no longer changes these odds.

Why: an earlier revision re-declared gravity under parallel `pm_ui_*` keys, which could add a second gravity candidate. The mod now leaves the vanilla definitions untouched and only reads `pm_low_gravity` and `pm_high_gravity` when weighting its own spawns.

## Observation: tidal locking is untouched

Tidally locked worlds remain three times more likely among moons, exactly as in vanilla.

Why: `pm_tidal_locked` is no longer re-declared. Its vanilla definition, including the `factor = 3` for `is_moon = yes` and the `add = 1` base weight, applies unchanged.

## Observation: the new modifiers are all penalties

Every new modifier reduces planetary statistics; none grants a bonus.

Why: the effect blocks use negative `pop_environment_tolerance`, `pop_happiness`, `logistic_growth_mult`, food, physics, and building-cost values only. The mod is a difficulty and flavour layer rather than a power increase, and every magnitude stays inside the vanilla band for a comparable modifier.

## Non-observations

- The mod does not change any planet during an already-generated game's existing save beyond the modifiers already recorded; new evaluations follow the mod's rules.
- The mod does not add any UI screen, menu, or control.
- The mod does not change planet classes, star classes, or any non-modifier planetary property.

## Related documentation

- [behaviour/planet-modifier-presentation.md](./planet-modifier-presentation.md) for what the player sees
- [flows/planet-modifier-selection.md](../flows/planet-modifier-selection.md) for the mechanism behind these observations
- [change-guide.md](../change-guide.md) for how editing a weight alters these observations
