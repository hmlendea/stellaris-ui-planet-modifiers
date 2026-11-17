# Error handling

## Principle

The repository handles nothing. It contains no code and therefore performs no validation, no recovery, and no fallback. Every failure is detected, reported, or silently absorbed by the host. This document classifies those failures so that a symptom can be traced to the file responsible.

## Failure taxonomy

| Class | Where detected | Severity | Observable result |
|-------|---------------|----------|-------------------|
| Descriptor unreadable or path unresolvable | Launcher | Fatal to the mod | Mod unlisted or listed with no content |
| Version incompatibility | Launcher | Advisory | Compatibility warning |
| Definition syntax error | Engine parser | Local to the definition | The definition is discarded; the modifier never spawns |
| Effect block syntax error | Engine parser | Local to the block | The effect is not applied |
| Binding miss (`modifier` names no key) | Engine at presentation | Silent | The modifier is selectable but effectless |
| Effect key unrecognised | Engine at application | Silent | The effect is ignored |
| Icon path unresolvable | Engine asset loader | Cosmetic | A missing or placeholder icon; the modifier remains active |
| Localisation key absent | Engine resolver | Cosmetic | The raw identifier is displayed |
| Localisation file unparseable | Engine parser | Broad | Every key in the file displays raw |
| Vanilla identifier renamed | Undetectable in-repository | Silent | Conditions become unevaluable and weights change |

## Failure semantics by boundary

### Launcher boundary

A descriptor with an invalid `path` leaves the mod registered with no content. A `supported_version` that excludes the running game produces a warning rather than a hard failure, so a contributor may load an incompatible mod and observe subtly wrong behaviour rather than an error.

### Definition-parser boundary

Parsing is per-definition. A malformed block invalidates its own definition without discarding siblings, so a syntax error in one modifier does not remove the other seven. This segmentation is helpful: a missing modifier usually means exactly one definition failed.

Duplicate keys behave differently. If two files declare the same key, the later file wins and the other is unobservable, with no error. The repository relies on this deliberately: the three keys in the override file are vanilla keys, re-declared so the mod's exclusions replace the host's definition.

### Effect boundary

An unrecognised effect key is ignored, not fatal. A block whose `icon` is unresolvable still yields an active, correctly named modifier with a broken icon. This is the reason icon and localisation defects are cosmetic while binding defects are functional.

### Presentation boundary

Raw identifier display is the canonical symptom of a missing localisation key. Because resolution is by convention, a key mismatch never produces an error — only the wrong text.

## Why silence is the dominant failure mode

Every binding in the repository is a name-based reference resolved by the host:

- a definition's `modifier` field must equal an effect key;
- a condition must name a real modifier, class, or star identifier;
- an `icon` value must name a real file;
- an effect key must have matching localisation keys.

None of these is checked by a compiler or by the repository. Consequently the default outcome of a mistake is not a crash but a quiet behavioural difference, which is why [troubleshooting.md](./troubleshooting.md) is symptom-organised rather than error-organised.

## Degradation behaviour

| Failure | Effect applied | Name shown | Icon shown | Modifier occupies a slot |
|---------|---------------|-----------|-----------|--------------------------|
| All correct | Yes | Correct | Correct | Yes |
| Icon path wrong | Yes | Correct | Placeholder | Yes |
| Localisation key missing | Yes | Raw key | Correct | Yes |
| Localisation file unparseable | Yes | Raw key | Correct | Yes |
| Effect key unrecognised | No (that effect ignored) | Correct | Correct | Yes |
| Binding miss | No | Raw key | Placeholder | Yes |
| Definition syntax error | n/a | n/a | n/a | No; never selectable |

The third column is the important one: a broken binding costs a modifier slot while conferring nothing, which is worse for a player than the modifier being absent.

## Recovery

| Failure | Recovery |
|---------|----------|
| Descriptor problem | Correct the `path`, or restore descriptor agreement |
| Definition syntax error | Repair the block and reload |
| Binding miss | Correct the `modifier` field or the effect key |
| Effect key unrecognised | Correct the key name, or confirm it exists for the target version |
| Icon problem | Correct the path or restore the file |
| Localisation problem | Correct the key, or restore the byte-order mark and document root |
| Host identifier rename | Update every condition naming the renamed identifier |

There is no automatic recovery and no retry. Every correction is a file edit followed by a game reload.

## Related documentation

- [troubleshooting.md](./troubleshooting.md) for symptom-based diagnosis
- [logging.md](./logging.md) for where host-side evidence appears
- [invariants.md](./invariants.md) for the invariants whose violation produces these failures
