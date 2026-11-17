# Change guide

Procedures for safe modification, organised by area. Each entry records the invariants that must hold, the files that must change together, and how to verify the result.

## Preparatory reading

- [invariants.md](./invariants.md) — what must remain true
- [data-model.md](./data-model.md) — the entity relationships
- [testing.md](./testing.md) — how to verify a change

## Adding a planet modifier

Five coordinated changes are required; omitting any one produces a specific, known defect.

| Step | File | Change | If omitted |
|------|------|--------|-----------|
| 1 | `common/planet_modifiers/ui-planet-modifiers.txt` | Add a `pm_ui_<name>` block with its `spawn_chance` and `modifier` field | The modifier never appears |
| 2 | `common/static_modifiers/ui-planet-modifiers_static_modifiers.txt` | Add a `ui_<name>` block with effects, `icon`, and `icon_frame` | The modifier appears with no effect |
| 3 | `gfx/interface/icons/planet_modifiers/` | Add `pm_ui_<name>.dds` | The icon is missing |
| 4 | `localisation/english/ui-planet-modifiers_l_english.yml` | Add `ui_<name>` and `ui_<name>_desc` entries | The raw key is displayed |
| 5 | Every affected exclusion list | Add the new identifier where it must be excluded | Contradictory modifiers can coexist |

Conventions to preserve:

- The definition key carries the `pm_` prefix; the effect key and localisation keys do not.
- The `modifier` field equals the effect key exactly.
- The icon file is named `pm_ui_<name>.dds` and referenced without the content-directory prefix.
- The description key is the effect key with `_desc` appended.

## Removing a planet modifier

1. Remove the definition, the effect block, the icon file, and both localisation entries together.
2. Remove the identifier from every exclusion list in both definition files.
3. Accept that existing saves retain the identifier and will show a dangling reference.

Removing an identifier is a breaking change for saves even though no code breaks. See [state-and-persistence.md](./state-and-persistence.md).

## Renaming a planet modifier

Renaming is the highest-risk change in the repository because the identifier is referenced from four directions simultaneously.

| Referenced by | Must be updated |
|---------------|-----------------|
| Cross-file conditions | Every exclusion list that names the old identifier |
| Definition `modifier` field | Must match the renamed effect key |
| Effect block key | The definition resolves to it |
| Icon file name and `icon` value | Must agree |
| Localisation name key and `_desc` key | Resolved by convention from the effect key |
| Existing saves | Cannot be updated; old identifiers persist |

Verify by searching the entire repository for the old identifier and confirming zero matches remain, excluding intentional historical references in documentation.

## Changing a spawn weight

1. Edit the relevant `add` or `factor` value.
2. Remember that weights are relative, so the change shifts every other candidate's effective probability.
3. Verify by generating planets across several new games and comparing outcomes subjectively.

This is the lowest-risk functional change: no binding is involved.

## Changing an effect value

1. Edit the numeric value in the effect block.
2. Confirm the sign matches the intent; only positive values carry an explicit plus sign.
3. Confirm the effect key exists for the target game version.
4. Note that the change applies retroactively to existing saves, because effects are resolved at load time.

## Adding an exclusion

1. Add a `modifier = { factor = 0; <condition> }` clause to the definition.
2. Where the exclusion conflicts with another modifier the mod also declares, add the reverse exclusion to that definition as well.
3. Confirm that the condition names a valid identifier.
4. Verify in-game that the pair no longer co-occurs.

Order symmetry matters because selection may evaluate either candidate first; see [concurrency-and-scheduling.md](./concurrency-and-scheduling.md).

## Replacing an icon

1. Export the new texture at 60 x 60 in 32-bit ARGB8888 DDS from `src/planet_modifier.xcf`.
2. Overwrite the existing file without renaming it, or rename it and update the `icon` value in lockstep.
3. Confirm the file still resolves and renders correctly.

## Adding a locale

1. Create `localisation/<language>/` beside `english/`.
2. Add a file named following the same convention with a matching document root, such as `l_french:`.
3. Translate all sixteen keys, retaining the byte-order mark.
4. Verify by selecting the language and confirming no raw keys appear.

No existing file requires modification, because localisation is additive.

## Updating the declared game version

1. Edit `supported_version` in both descriptors to the same value.
2. Verify by loading the mod on the target version and confirming that all four modifiers generate, present correctly, and apply their effects.
3. Confirm that no referenced vanilla identifier has been renamed; a rename manifests as an unevaluable condition rather than a load error.
4. Publish a release; `.github/workflows/github-release.yml` builds the archive from the tagged revision automatically.

## Changing the mod name, path, or content directory

1. Change `name` in both descriptors together if only the display name changes.
2. If the content directory is renamed, update `path` in both descriptors and the delivery layout in [build-and-deployment.md](./build-and-deployment.md).
3. Verify by confirming the launcher lists the mod and resolves its content.

## Pre-change checklist

Before committing any change, confirm:

- [ ] Both descriptors still agree on every field.
- [ ] Every definition's `modifier` field equals an existing effect key.
- [ ] Every `icon` value resolves, and every icon file is referenced.
- [ ] Every effect key has a name key and a `_desc` key.
- [ ] Every conflict introduced is excluded in both directions.
- [ ] No exclusion list names an identifier that no longer exists.
- [ ] The change has been loaded in-game and the affected behaviour observed.

## Related documentation

- [invariants.md](./invariants.md) for the full invariant list
- [testing.md](./testing.md) for verification procedures
- [troubleshooting.md](./troubleshooting.md) for diagnosing a failed change
