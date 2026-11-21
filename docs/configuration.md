# Configuration

## What "configuration" means here

The repository has no configuration files, no environment variables, no command-line options, and no settings API. Everything that determines behaviour is the definition content itself. This document enumerates those sources so that a reader can locate the lever for any observable behaviour.

## Configuration areas

| Area | Source | Controls | Superseded by | Secrets |
|------|--------|----------|---------------|---------|
| Mod identity | `ui-planet-modifiers.mod`, `ui-planet-modifiers/descriptor.mod` | Name, content path, tags, declared game version | Nothing; both must agree | None |
| Spawn eligibility | `common/planet_modifiers/ui-planet-modifiers.txt` | Whether and how strongly each of the five new modifiers enters the candidate pool | Nothing | None |
| Host-modifier eligibility | `common/planet_modifiers/ui-planet-modifiers-vanillas.txt` | Re-declared spawn chances for the two vanilla magnetic-field modifiers | The host's own definitions, which the mod's versions replace | None |
| Effects and icon binding | `common/static_modifiers/ui-planet-modifiers_static_modifiers.txt` | Effect values, icon path, icon frame, importance | Nothing | None |
| Display strings | `localisation/english/ui-planet-modifiers_l_english.yml` | Names and descriptions | Nothing | None |
| Icon appearance | `gfx/interface/icons/planet_modifiers/*.dds` | Rendered artwork | Nothing | None |

## Precedence

| Situation | Winner | Reason |
|-----------|--------|--------|
| Two descriptors disagree | Undefined | The launcher reads one of them depending on installation method; the repository does not enforce agreement |
| The override file and the host's own definition both declare the same key | Undefined | Key merging behaviour is host-owned; the repository cannot express precedence |
| The override file declares a new key bound to a host effect | The new key | Keys are distinct, so both candidates can exist |
| Mod vs mod | Undefined | Driven by load order or key merging in the host |

The repository deliberately avoids relying on precedence: the override file chooses new key names precisely so that it does not have to shadow host definitions.

## The definition content as a tuning surface

Weights are relative, so "configuring" generation means choosing a distribution across the candidate pool rather than setting absolute probabilities.

| If you want | Adjust |
|-------------|--------|
| A modifier to appear more often | Raise its `add` values or add a reinforcing `factor` |
| A modifier to appear less often without removing it | Lower its `add` values or add a `factor` below 1 |
| A modifier to be impossible under a condition | Add a `factor = 0` clause with that condition |
| A modifier to stop accumulating | Add or lower its `num_modifiers` guard |
| A modifier to stop existing | Remove its definition, effect block, icon, and localisation keys together |

Because weights are relative, any adjustment shifts every other candidate's effective probability. A change intended to affect one modifier therefore affects the whole generation profile.

## Configuration that cannot be expressed here

| Concern | Reason |
|---------|--------|
| Absolute spawn probability | The engine samples from a normalised distribution |
| Deterministic seeds | Host-owned |
| Which planets are generated at all | Host-owned galaxy generation |
| Interface appearance | Host-owned, beyond the icon asset |
| Load order between mods | Host-owned |
| Compatibility enforcement | Host-owned |

## Compatibility configuration

The only compatibility lever in the repository is `supported_version` in both descriptors. Raising it asserts that the definitions were validated against a newer game version; nothing verifies the assertion. See [change-guide.md](./change-guide.md).

## Localisation configuration

The active locale is selected by the directory name and the YAML document root, not by a setting. Adding a locale is additive and requires no configuration change. See [components/localisation.md](./components/localisation.md).

## Validation

| Concern | How to validate |
|---------|----------------|
| Descriptor agreement | Diff the two descriptor files |
| Binding correctness | Cross-check each definition's `modifier` field against the effect keys |
| Icon path resolution | Cross-check each `icon` value against the icon directory listing |
| Localisation coverage | Cross-check effect keys against name and `_desc` keys |
| Actual behaviour | Load the mod in the game and inspect generated planets |

No command or tool in the repository performs any of the first four checks. They are described as procedures in [testing.md](./testing.md).

## Related documentation

- [dependencies.md](./dependencies.md) for the sources of referenced identifiers
- [change-guide.md](./change-guide.md) for safe modification procedures
- [components/static-modifiers.md](./components/static-modifiers.md) for the effect vocabulary
