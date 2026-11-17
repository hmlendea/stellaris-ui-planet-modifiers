# Component: localisation

## Responsibility

Supply the human-readable display name and description for each of the four new planet modifiers.

## Location

`ui-planet-modifiers/localisation/english/ui-planet-modifiers_l_english.yml` — the only locale present.

## File characteristics

| Property | Value |
|----------|-------|
| Encoding | UTF-8 with byte-order mark (`EF BB BF`) |
| Document root | `l_english:` |
| Entries | 8 (four names, four descriptions) |
| Language directory | `english/` |
| Naming | `<mod>_l_<locale>.yml` |

The byte-order mark is present and is a requirement of the host localisation parser for this file type, not an artefact.

## Key resolution

Localisation keys are not referenced by any field. The engine derives them from the static modifier key:

| Derived key | Content |
|-------------|---------|
| `<effect key>` | Display name, for example `UI Salty` rendered from `ui_salty` |
| `<effect key>_desc` | Description text |

Because resolution is by convention, a key typo produces no parse error. The interface simply shows the raw identifier in place of the translated text.

## Inventory

| Name key | Display name | Description key |
|----------|--------------|-----------------|
| `ui_extreme_temperatures` | Extreme Temperatures | `ui_extreme_temperatures_desc` |

| `ui_no_magnetic_field` | No Magnetic Field | `ui_no_magnetic_field_desc` |
| `ui_salty` | Salty | `ui_salty_desc` |
| `ui_swampy` | Swampy | `ui_swampy_desc` |

All four effect keys have both entries, so key-derived assumptions hold for every display name in the file.

## Ordering

Entries are grouped in pairs and the file order is alphabetical by key, not by the order the definitions appear in the eligibility file. The engine does not depend on localisation ordering.

## Description content

| Key | Description theme |
|-----|-------------------|
| `ui_extreme_temperatures_desc` | Blistering days and freezing nights, with almost no atmosphere to buffer the surface |

| `ui_no_magnetic_field_desc` | No magnetic field, leaving the world exposed to cosmic radiation |
| `ui_salty_desc` | Extremely saline soils and waters, hostile to growth |
| `ui_swampy_desc` | Dense swamps rendering large areas dangerous and unworkable |

Descriptions are thematic narrative rather than a statement of numeric effects, so they do not need to be revised when an effect value changes in the [static modifier component](./static-modifiers.md). They were revised alongside the v4.5 rebalance wherever the concept sharpened, and they should be revised again if a modifier's concept changes.

## Adding a locale

A new locale is additive: create a sibling directory under `localisation/` named after the language and add a file following the same naming convention with a matching document root (`l_french:`, for example). No other file in the repository references the locale, so no existing file requires modification. The vocabulary used by the existing English strings is consistent with the project's own terminological preferences; a new locale should express the same concepts rather than transliterate the English wording.

## Failure behaviour

| Failure | Observable result |
|---------|-------------------|
| Missing name key | The raw effect key is displayed, for example `ui_salty` |
| Missing description key | The raw description key is displayed |
| Byte-order mark removed | The host parser may fail to read the file, producing untranslated output for every key |
| Key misspelled | The typo'd key is unreachable and the intended key falls back to its raw form |
| Wrong document root for the directory | The file is ignored entirely |

## Change impact

Renaming an effect key in the static modifier component silently orphans both localisation entries unless they are renamed to match. Adding a modifier requires a name key and a description key. See [invariants.md](../invariants.md) and [change-guide.md](../change-guide.md).

## Related documentation

- [components/static-modifiers.md](./static-modifiers.md) for the keys that drive resolution
- [configuration.md](../configuration.md) for the display-strings configuration area
