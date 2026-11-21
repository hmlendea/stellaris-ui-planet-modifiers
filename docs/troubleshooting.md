# Troubleshooting

Symptom-organised diagnosis. Each entry names the probable cause and the check that confirms it.

## The mod does not appear in the launcher

| Probable cause | Check |
|----------------|-------|
| Descriptor not in the location the launcher scans | Confirm `ui-planet-modifiers.mod` is in the user mod directory |
| `path` does not resolve | Confirm the value `mod/ui-planet-modifiers` matches the delivered content directory |
| Content directory name changed | Confirm the directory name equals the final element of `path` |

## The mod is listed but its modifiers never appear

| Probable cause | Check |
|----------------|-------|
| A definition block failed to parse | Read the engine log for the definition file |
| Class conditions do not match the observed planets | Inspect the `is_planet_class` clause for the modifier in question |
| An exclusion fires | Inspect the definition's `factor = 0` clauses |
| An accumulation guard prevents it | Inspect the `num_modifiers` guard; the new definitions require one modifier or fewer |
| The modifier is expected on a large planet but high gravity forbids it | High gravity is forbidden below size 12 by design; low gravity is not |

## The modifier appears with a raw key instead of a name

| Probable cause | Check |
|----------------|-------|
| The name key is missing or misspelled | Confirm a `ui_<name>:0 "..."` entry exists |
| The description key is missing | Confirm a `ui_<name>_desc:0 "..."` entry exists |
| The localisation file failed to parse | Confirm the byte-order mark is present and the document root is `l_english` |
| The effect key was renamed without renaming the entries | Compare effect keys with localisation keys |

## The modifier appears with a placeholder or wrong icon

| Probable cause | Check |
|----------------|-------|
| The `icon` value does not resolve | Confirm the path matches the file exactly, without the `ui-planet-modifiers/` prefix |
| The file was renamed without updating the effect block | Confirm no orphaned or missing file remains |
| `icon_frame` is wrong | Confirm the frame value matches the intended presentation |
| The texture format or dimensions are incompatible | Confirm a 60 x 60 32-bit ARGB8888 DDS |

## The modifier appears but has no effect

| Probable cause | Check |
|----------------|-------|
| The definition's `modifier` field does not equal the effect key | Cross-check the two values character for character |
| The effect block was removed | Confirm the effect key exists in the effect file |
| A host-bound override key is unrecognised | Confirm the host still defines the bound effect key |

## Two contradictory modifiers appear on one planet

| Probable cause | Check |
|----------------|-------|
| An exclusion was omitted from one side of the pair | Confirm both definitions exclude each other |
| An exclusion list is incomplete for its family | Compare the magnetic and gravity lists against their full membership |
| A key was renamed and an exclusion silently stopped firing | Search every exclusion list for the old identifier |

## Existing saves behave unexpectedly

| Probable cause | Check |
|----------------|-------|
| The mod was removed while saves referenced its identifiers | Expect raw keys and missing effects; this is inherent, not fixable |
| An effect value changed | Saves re-resolve effects at load time, so a rebalance alters existing planets |
| An identifier was renamed | Saves retain the old identifier, which now resolves to nothing |

## Compatibility warning in the launcher

| Probable cause | Check |
|----------------|-------|
| `supported_version` excludes the running game | The descriptors declare `v4.5.*`; a warning means the game is a different release line |
| One descriptor was updated and the other was not | Diff the two descriptor files |

## Multiplayer inconsistency

| Probable cause | Check |
|----------------|-------|
| A client is missing the mod | Confirm every participant has it activated |
| Mod versions differ | Confirm identical files across participants |

## Diagnostic order

1. Read the engine log with raised verbosity; it distinguishes a load failure from a silent exclusion.
2. If the log is clean, suspect conditions and exclusions rather than parsing.
3. Check the binding chain in order: definition `modifier` field, effect key, icon path, localisation keys.
4. Confirm descriptor agreement last, since it affects whether anything loads at all.

## Related documentation

- [error-handling.md](./error-handling.md) for the failure taxonomy
- [logging.md](./logging.md) for where evidence appears
- [testing.md](./testing.md) for the verification procedure
- [faq.md](./faq.md) for related inquiries
