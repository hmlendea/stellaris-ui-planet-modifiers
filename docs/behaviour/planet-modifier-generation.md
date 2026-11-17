# Behaviour: planet modifier generation

This document describes what a player observes, and why, when the mod is active during galaxy generation.

## Observation: additional modifiers appear on generated planets

The most direct behavioural change is that planets can carry any of eight modifiers the base game does not provide: perpetual twilight, extreme temperatures, absence of a magnetic field, salinity, swampland, microorganisms, giant earthworms, and sea monsters.

Why: the additions file declares a `spawn_chance` for each identifier, so each enters the candidate pool for every generated planet that satisfies its conditions.

## Observation: barren and cold-barren worlds dominate the new appearances

Five of the eight new modifiers favour barren worlds through class conditions:

| Modifier | Barren weight contribution |
|----------|---------------------------|
| `pm_ui_no_magnetic_field` | `add = 20` |
| `pm_ui_salty` | `add = 30` on barren, `add = 5` on cold barren |
| `pm_ui_microorganisms` | `add = 5` |
| `pm_ui_twilight` | `add = 2` |
| `pm_ui_extreme_temperatures` | `add = 5` |

Why: barren and cold-barren worlds have no habitability penalty to protect, so the mod's uniformly negative effects are thematically appropriate there, and the author weighted the definitions accordingly. The highest single weight in the set, `add = 30` for salinity on a barren world, makes salty barren planets the most common new outcome.

## Observation: modifiers cluster around thematically compatible worlds

Several definitions reward conditions that reinforce their theme:

| Modifier | Reinforcing condition | Multiplier |
|----------|----------------------|-----------|
| `pm_ui_twilight` | Already tidally locked | ×4 |
| `pm_ui_twilight` | Bleak | ×2 |
| `pm_ui_extreme_temperatures` | Bleak | ×2 |
| `pm_ui_no_magnetic_field` | Low gravity | ×2 |
| `pm_ui_giant_earthworms` | Lush, or high gravity | ×2 each |
| `pm_ui_sea_monsters` | High gravity | ×2 |

Why: the multipliers encode narrative compatibility. A tidally locked world plausibly has a permanent twilight zone; a lush world plausibly supports large burrowing fauna. Because multipliers compound, a lush high-gravity arid world quadruples giant earthworm eligibility.

## Observation: contradictory modifiers do not appear together

A planet does not carry both perpetual twilight and extreme temperatures, nor two magnetic-field conditions, nor both gravity variants.

Why: each conflicting pair is separated by `factor = 0` exclusion clauses. Where a conflict exists among the new modifiers, both sides exclude each other, so the outcome does not depend on assignment order.

## Observation: new modifiers mostly stop at one modifier per planet

Seven of the eight new definitions carry a `num_modifiers > 1` guard, so a planet that already has one modifier cannot receive them.

Why: the guard limits how many modifiers a planet accumulates. `pm_ui_swampy` omits the guard, and `pm_ui_microorganisms` relaxes it to `num_modifiers > 2`, so those two are the exceptions that can join an already-modified planet.

## Observation: gravity depends on planet size

Low gravity favours small worlds and high gravity favours large worlds, with high gravity impossible below size 12.

Why: both redefinitions use `planet_size` conditions with compounding multipliers. The low-gravity definition triples a ×2 factor on progressively smaller worlds for a cumulative ×8 below size 7, while the high-gravity definition applies ×2 twice above size 19 and 22 and forbids the modifier entirely below size 12.

## Observation: moon-specific tidal locking

Tidally locked worlds are three times more likely among moons.

Why: `pm_ui_tidal_locked` is the only definition using `is_moon = yes` with a `factor = 3`. Its base weight is also the lowest in the set, at `add = 1`, so the moon multiplier is what makes the modifier observable at all.

## Observation: modifiers are absent near hostile stellar remnants

Microorganisms cannot appear in a system whose primary star is a black hole, neutron star, or pulsar.

Why: `pm_ui_microorganisms` declares a system-scope exclusion through `solar_system = { OR = { is_star_class = ... } }`. This is the only system-scope condition in the mod.

## Observation: microorganisms is presented prominently but confers no statistical effect

The microorganisms modifier appears with emphasis yet changes nothing numerically.

Why: its effect block contains no effect keys and sets `important = yes`. It is a narrative and presentation entry; its presence occupies a modifier slot, which is why its accumulation guard is the more permissive `num_modifiers > 2`.

## Observation: the new modifiers are mostly penalties

Seven of the eight new modifiers reduce planetary statistics; the exception is the microorganisms entry, which changes nothing. Giant earthworms and sea monsters mitigate their habitability penalty with a society research bonus.

Why: the effect blocks are dominated by negative `pop_environment_tolerance`, `pop_happiness`, and food-production values, with two positive society-research values. The mod is a difficulty and flavour layer rather than a power increase.

## Observation: existing modifier behaviour is altered, not merely extended

Vanilla modifiers such as hazardous weather, tidal locking, gravity, magnetic field, asteroid impacts, and wild storms still function, but their spawn chances are now governed by the mod's redefinitions.

Why: the override file declares `pm_ui_*` keys bound to the host's effect keys. Because the mod's versions add the new identifiers to their exclusion lists, a planet cannot receive both the vanilla-flavoured and the mod-flavoured variant of a magnetic field or gravity modifier.

## Non-observations

- The mod does not change any planet during an already-generated game's existing save beyond the modifiers already recorded; new evaluations follow the mod's rules.
- The mod does not add any UI screen, menu, or control.
- The mod does not change planet classes, star classes, or any non-modifier planetary property.

## Related documentation

- [behaviour/planet-modifier-presentation.md](./planet-modifier-presentation.md) for what the player sees
- [flows/planet-modifier-selection.md](../flows/planet-modifier-selection.md) for the mechanism behind these observations
- [change-guide.md](../change-guide.md) for how editing a weight alters these observations
