# Testing

## Current state

The repository contains no test suite, no validation script, no linting configuration, and no continuous integration. Verification is manual and requires the host game.

| Verification type | Present |
|-------------------|---------|
| Unit tests | No |
| Integration tests | No |
| Architecture tests | No |
| Definition schema validation | No |
| Cross-file consistency check | No |
| Static analysis or linting | No |
| Continuous integration | No |
| Manual in-game verification | Yes, by the contributor |

The absence is not anomalous for a static-data mod, but it has a specific cost: every cross-file invariant in [invariants.md](./invariants.md) relies entirely on contributor discipline.

## Coverage gaps and their consequences

| Gap | Consequence |
|-----|-------------|
| Spawn-weight changes cannot be verified deterministically | Only repeated in-game generation reveals a distribution shift, and even then only subjectively |
| Exclusion conditions are unverified against vanilla identifiers | A host-side rename produces no repository-visible failure |
| Localisation key coverage has no automated check | A missing key appears only as a raw identifier in the interface |
| Icon path resolution has no automated check | A broken path appears only as a missing icon |
| The definition-to-effect binding has no automated check | A broken binding appears only as an effectless modifier |
| `supported_version` is unverified against the current engine | The compatibility claim may be stale |

## Manual verification procedure

### Load verification

1. Activate the mod in a playset and start the game.
2. Inspect the engine log for parse errors relating to the mod's files.
3. Confirm that the mod is listed with the expected name, tag, and compatibility verdict.

### Generation verification

1. Start a new game and generate planets across several classes, including barren, cold barren, arid, desert, ocean, tropical, continental, and savage-arid classes to exercise each definition's class conditions.
2. Confirm that each new modifier appears on at least one planet, given enough generated planets.
3. Confirm that a planet never carries both perpetual twilight and extreme temperatures.
4. Confirm that a planet never carries two members of the magnetic family.
5. Confirm that a planet never carries both gravity variants.
6. Confirm that a planet below size 12 never carries high gravity.
7. Confirm that microorganisms never appears in a black hole, neutron star, or pulsar system.

### Presentation verification

1. Inspect a planet carrying each new modifier.
2. Confirm that the display name is narrative text rather than a raw key.
3. Confirm that the description is present and is narrative rather than a raw key.
4. Confirm that the icon is the intended artwork rather than a placeholder.
5. Confirm that microorganisms is presented prominently.

### Effect verification

1. Inspect a colony on a planet carrying a new modifier.
2. Confirm that the effect values match the block: habitability, happiness, growth, food production, or society research as applicable.
3. Confirm that microorganisms confers no statistical change.

### Change-focused verification

| Change | Additional verification |
|--------|------------------------|
| New or removed definition | Confirm the modifier appears or never appears, and that no exclusion list still names a removed key |
| Changed weight | Compare generation outcomes before and after, using several new games |
| Renamed key | Confirm the effect applies, the name resolves, and no orphaned icon or localisation key remains |
| New icon | Confirm the file resolves, the frame is correct, and the artwork renders without distortion |
| New locale | Confirm the locale's strings appear when the language is selected, with no raw keys |
| Descriptor change | Confirm the launcher still lists the mod and still resolves its content |

## Recommended consistency checks a contributor can perform manually

These replace an automated validator and take only a minute each.

| Check | Procedure |
|-------|-----------|
| Descriptor agreement | Compare the two descriptor files field by field |
| Binding agreement | For each definition, confirm its `modifier` value appears as an effect key |
| Icon bijection | Confirm each `icon` value names an existing file and each existing file is named by exactly one block |
| Localisation coverage | Confirm each effect key has a name key and a `_desc` key |
| Exclusion symmetry | For each conflicting pair, confirm both definitions exclude each other |
| Identifier hygiene | Search the repository for every identifier referenced in a condition and confirm whether it is defined here or expected from the host |

## Why the gaps are tolerated

| Reason | Assessment |
|--------|------------|
| The format has no schema the repository could validate against locally | A host-format validator would have to be written from scratch |
| The engine is the only authority on semantics | Offline validation could confirm syntax but not behaviour |
| The mod is small | Eight new definitions and ten overrides are tractable to review by eye |
| The cost of a defect is low | A wrong weight is a flavour defect, not a correctness failure |

The gaps are therefore acceptable in practice while remaining the repository's principal quality exposure. A future contributor implementing a cross-file consistency script would remove most of them without needing the host.

## Related documentation

- [invariants.md](./invariants.md) for what must be verified
- [troubleshooting.md](./troubleshooting.md) for diagnosing a failure that verification uncovers
- [logging.md](./logging.md) for where host-side error evidence appears
