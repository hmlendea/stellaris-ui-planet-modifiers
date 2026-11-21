# Repository overview

## Purpose

The repository delivers a single Stellaris mod, *Universum Infinitum - Planet Modifiers*, which broadens the planetary modifier pool available during galaxy generation. It contributes seven new modifiers, calibrated against the vanilla v4.5 set, and re-declares the two vanilla magnetic-field modifiers so the new third state cannot coexist with them.

The delivered artefacts are entirely declarative:

- two mod descriptors that declare identity, content path, tags, and the declared compatible game version;
- planet modifier definitions that express spawn chances as conditions and weights;
- static modifier definitions that express effects and bind an icon;
- seven DDS icon textures, plus the retained GIMP source artwork;
- one English localisation file containing display names and descriptions.

## Scope

The repository owns:

- the seven `pm_ui_*` planet modifier definitions and their spawn chances;
- re-declarations of the spawn chances for `pm_weak_magnetic_field` and `pm_strong_magnetic_field`;
- the seven `ui_*` static modifier effect blocks, their icon paths, icon frames, and importance flags;
- the seven icon textures and their editable source;
- the English localisation keys and strings for the seven new modifiers;
- both mod descriptors.

## Deliberate exclusions

The repository does not own, define, or verify:

- the Stellaris engine, its parsers, or its selection policy;
- vanilla planet modifier identifiers, planet classes, star classes, or modifier effects referenced by its own conditions;
- vanilla static modifier definitions (`hazardous_weather`, `weak_magnetic_field`, `strong_magnetic_field`, `unstable_tectonics`, `tidal_locked`, `asteroid_impacts`, `wild_storms`, `low_gravity`, `high_gravity`), which the override file binds to but does not define;
- the display of modifiers, which is rendered by the host interface;
- save-game state, which is owned and serialised by the engine;
- distribution platforms and their policies.

## Entry points

| Entry point | Type | Description |
|-------------|------|-------------|
| `ui-planet-modifiers.mod` | Launcher descriptor | Root descriptor the launcher reads to register the mod |
| `ui-planet-modifiers/descriptor.mod` | Launcher descriptor | Local descriptor carrying identical metadata |
| `ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers.txt` | Definition file | Seven new planet modifier definitions |
| `ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers-vanillas.txt` | Definition file | Two re-declared vanilla spawn chances |
| `ui-planet-modifiers/common/static_modifiers/ui-planet-modifiers_static_modifiers.txt` | Definition file | Seven static modifier effect blocks |
| `ui-planet-modifiers/localisation/english/ui-planet-modifiers_l_english.yml` | Localisation file | Fourteen English keys |
| `ui-planet-modifiers/gfx/interface/icons/planet_modifiers/*.dds` | Asset directory | Seven icon textures |
| `ui-planet-modifiers/thumbnail.png` | Asset | Launcher thumbnail |

There is no executable entry point, no service, and no scheduled process. Nothing in the repository runs on its own; execution begins only when the Stellaris engine reads the files.

## Operating model

The mod is passive. It supplies data; the host performs every operation. Consequences of that model:

- no repository file depends on another repository file at run time, because the engine resolves all cross-references;
- no load order exists between the definition files, because they contribute distinct keys with the exception of the intentional redefinitions;
- failures are silent to the repository and visible only through the host engine's own logs or through in-game observation.

## What this repository is not

- It is not a library or an application programming interface. There are no endpoints, no public classes, and no callable surface other than the host's own definition formats.
- It is not testable in isolation. Verification requires the host game.
- It is not testable in isolation: verification requires the host game. The descriptors declare `supported_version="v4.5.*"`.

## Related documentation

- [repository-structure.md](./repository-structure.md) for the layout
- [architecture.md](./architecture.md) for the architectural model
- [data-model.md](./data-model.md) for the definition entities
