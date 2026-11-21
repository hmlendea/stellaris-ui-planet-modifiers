# Component: icon assets

## Responsibility

Provide the visual representation of each new planet modifier in the interface, and retain the editable source from which those representations were produced.

## Location

| Path | Role |
|------|------|
| `ui-planet-modifiers/gfx/interface/icons/planet_modifiers/` | Six DDS textures consumed by the engine |
| `src/planet_modifier.xcf` | Editable GIMP source, outside the content directory |

## Inventory

| File | Size | Format | Dimensions | Referenced by |
|------|------|--------|-----------|---------------|
| `pm_ui_extreme_temperatures.dds` | 14,528 bytes | DDS, 32-bit ARGB8888 | 60 x 60 | `ui_extreme_temperatures` |
| `pm_ui_no_magnetic_field.dds` | 14,528 bytes | DDS, 32-bit ARGB8888 | 60 x 60 | `ui_no_magnetic_field` |
| `pm_ui_salty.dds` | 14,528 bytes | DDS, 32-bit ARGB8888 | 60 x 60 | `ui_salty` |
| `pm_ui_swampy.dds` | 14,528 bytes | DDS, 32-bit ARGB8888 | 60 x 60 | `ui_swampy` |
| `pm_ui_thin_atmosphere.dds` | 14,528 bytes | DDS, 32-bit ARGB8888 | 60 x 60 | `ui_thin_atmosphere` |
| `pm_ui_dense_atmosphere.dds` | 14,528 bytes | DDS, 32-bit ARGB8888 | 60 x 60 | `ui_dense_atmosphere` |

Every file is the same size and format, and all six are referenced, so the asset set and the effect set are in exact correspondence.

## Source artwork

| Property | Value |
|----------|-------|
| Path | `src/planet_modifier.xcf` |
| Format | GIMP XCF version 003 |
| Dimensions | 60 x 60 |
| Colour model | RGB |

The source matches the delivered icon dimensions, and it is deliberately placed outside the mod content directory so that the engine never loads it. The source is tracked in Git and is the only editable form of the artwork; the DDS files are derived artefacts with no generation script in the repository, so regenerating them is a manual editor workflow.

## Format constraints

| Property | Value | Reason |
|----------|-------|--------|
| Container | Microsoft DirectDraw Surface | The format the host interface loader accepts |
| Pixel format | 32-bit ARGB8888 | Preserves an alpha channel for icon shaping |
| Dimensions | 60 x 60 | Matches the host icon slot and the source artwork |

No build step converts the source into DDS, and no validation checks that the dimensions or pixel format remain compatible. A replacement produced with a different format or size may be rejected or may render incorrectly with no repository-visible signal.

## Launcher thumbnail

| Property | Value |
|----------|-------|
| Path | `ui-planet-modifiers/thumbnail.png` |
| Format | PNG, 8-bit RGB, non-interlaced |
| Dimensions | 400 x 400 |

The thumbnail is addressed at the launcher's mod browser rather than at the engine's icon loader, which is why it uses PNG rather than DDS and a larger canvas.

## Failure behaviour

| Failure | Observable result |
|---------|-------------------|
| File missing | The modifier's `icon` path resolves to nothing; a missing or placeholder icon is rendered |
| File renamed without updating the effect block | Same as missing, because the reference is a literal string |
| Effect block renamed without renaming the file | The file becomes unreferenced and the declaration points at a non-existent path |
| Incompatible format or dimensions | The texture may fail to load or render incorrectly |
| Thumbnail missing or malformed | The launcher shows a placeholder for the mod |

## Change impact

Renaming an icon requires renaming the file and the `icon` value in the effect block in lockstep. Replacing the artwork without changing names is safe with respect to every other file. See [invariants.md](../invariants.md).

## Related documentation

- [components/static-modifiers.md](./static-modifiers.md) for the referencing declarations
- [change-guide.md](../change-guide.md) for the safe procedure
