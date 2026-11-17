# Behaviour: planet modifier presentation

This document describes what a player observes when inspecting a planet that carries one of the mod's modifiers.

## Observation: modifiers appear with a name, a description, and an icon

Each of the eight new modifiers presents a short name, a one-sentence description, and a distinct icon.

Why: each effect block declares an `icon` path to a DDS texture, and the engine resolves a name and a `_desc` description from the effect key by convention.

## Observation: names are short and descriptions are narrative

| Modifier | Display name | Description theme |
|----------|--------------|-------------------|
| `ui_twilight` | Perpetual Twilight | No clear day or night |
| `ui_extreme_temperatures` | Extreme Temperatures | Sub-zero nights, blazing days |
| `ui_no_magnetic_field` | No Magnetic Field | Exposed to cosmic radiation |
| `ui_salty` | Salty | Saline soils and waters hostile to growth |
| `ui_swampy` | Swampy | Dense swamps render areas unworkable |
| `ui_microorganisms` | Microorganisms | Primitive microorganisms present |
| `ui_giant_earthworms` | Giant Earthworms | Giant earthworms lurking |
| `ui_sea_monsters` | Sea Monsters | Dangerous fauna in the waters |

Why: descriptions are written as flavour text rather than as a restatement of numeric effects, so they remain accurate when an effect value is rebalanced. The single exception worth noting is that the display name *Perpetual Twilight* does not match its key stem `twilight`, which demonstrates that display text is independent of key naming.

## Observation: microorganisms is emphasised

The microorganisms modifier is presented in the prominent form, unlike the others.

Why: its effect block sets `important = yes`, which is the only occurrence of that flag in the mod.

## Observation: icons are visually distinct but share a frame style

Five modifiers use icon frame 3, two use frame 2, and one uses frame 1. Each has its own texture.

Why: `icon_frame` selects a presentation frame independently of the texture, so the mod mixes a common frame with per-modifier artwork.

## Observation: tooltips show consistent effect wording

Because all eight blocks reuse the same five effect keys, the tooltips read consistently regardless of which modifier is inspected.

Why: `pop_environment_tolerance`, `pop_happiness`, `pop_growth_speed`, `planet_jobs_food_produces_mult`, and `planet_jobs_society_research_produces_mult` are host-provided effect keys with host-provided wording.

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
