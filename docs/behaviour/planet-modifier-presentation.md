# Behaviour: planet modifier presentation

This document describes what a player observes when inspecting a planet that carries one of the mod's modifiers.

## Observation: modifiers appear with a name, a description, and an icon

Each of the eight new modifiers presents a short name, a one-sentence description, and a distinct icon.

Why: each effect block declares an `icon` path to a DDS texture, and the engine resolves a name and a `_desc` description from the effect key by convention.

## Observation: names are short and descriptions are narrative

| Modifier | Display name | Description theme |
|----------|--------------|-------------------|
| `ui_extreme_temperatures` | Extreme Temperatures | Sub-zero nights, blazing days |
| `ui_no_magnetic_field` | No Magnetic Field | Exposed to cosmic radiation |
| `ui_salty` | Salty | Saline soils and waters hostile to growth |
| `ui_swampy` | Swampy | Dense swamps render areas unworkable |
| `ui_thin_atmosphere` | Thin Atmosphere | A whisper of an atmosphere, too thin to buffer the surface |
| `ui_dense_atmosphere` | Dense Atmosphere | A heavy, crushing atmosphere that resists construction |
| `ui_toxic_soil` | Toxic Soil | Toxic compounds poison the ground and leave it barren |
| `ui_geological_instability` | Geological Instability | A restless crust whose tremors hinder construction |

Why: descriptions are written as flavour text rather than as a restatement of numeric effects, so they remain accurate when an effect value is rebalanced.

## Observation: no modifier is emphasised

None of the new modifiers is presented in the prominent form.

Why: no effect block sets `important = yes`. The flag is reserved by vanilla for modifiers that must show a planet nameplate, such as terraforming candidates.

## Observation: icons are visually distinct but share a frame style

All eight modifiers use icon frame 3. Each has its own texture.

Why: `icon_frame` selects a presentation frame independently of the texture, and frame 3 is the vanilla marker for a net penalty.

## Observation: tooltips show consistent effect wording

The tooltips read consistently because every effect is expressed through a standard host-provided key.

Why: `pop_environment_tolerance`, `pop_happiness`, `logistic_growth_mult`, `planet_jobs_food_produces_mult`, `planet_jobs_energy_produces_mult`, `planet_jobs_physics_research_produces_mult`, `planet_jobs_society_research_produces_mult`, `district_farming_max_add`, and `planet_structures_cost_mult` are host-provided effect keys with host-provided wording. The mod adds no custom tooltip and no `custom_tooltip` override.

## Observation: modifiers integrate with the existing interface

No new interface element, screen, menu, or control appears. The modifiers are presented exactly as vanilla modifiers are.

Why: the repository contributes no interface code and defines no new interface element. Presentation is entirely host-owned.

## Degraded observations and their causes

| What the player sees | Cause |
|----------------------|-------|
| A raw key such as `ui_salty` instead of a name | The name key is missing, misspelled, or in an unparseable locale file |
| A correct name with a raw description key | Only the `_desc` key is missing |
| A missing or placeholder icon beside a correct name | The `icon` path does not resolve |
| A visual icon that does not match the modifier | `icon_frame` or the referenced texture is wrong |
| A modifier with a name and description but no effect on the planet | The `modifier` binding is broken or the effect block is missing |
| No modifier at all on a qualifying planet | The definition was rejected, or a condition or exclusion removed it from the pool |

## Related documentation

- [components/localisation.md](../components/localisation.md) for key resolution
- [components/static-modifiers.md](../components/static-modifiers.md) for effects and icon binding
- [flows/modifier-presentation.md](../flows/modifier-presentation.md) for the mechanism
- [troubleshooting.md](../troubleshooting.md) for diagnosis
