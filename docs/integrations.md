# Integrations

## Integration surfaces

The mod integrates with the host and with distribution platforms. There are no third-party service integrations, no network calls, and no external data sources.

| Integration | Direction | Contract | Owner of the contract | Failure semantics |
|-------------|-----------|----------|-----------------------|-------------------|
| Stellaris launcher mod loading | Inbound | `.mod` descriptor with `name`, `path`, `tags`, `supported_version` | Stellaris launcher | Mod is unlisted, or flagged incompatible, or resolves to no content |
| Planet modifier spawn system | Inbound | `spawn_chance` block with `add`, `factor`, and condition tests | Stellaris engine | A definition that fails to parse is discarded; the modifier never spawns |
| Static modifier system | Inbound | Effect keys with numeric values, plus `icon`, `icon_frame`, and optional `important` | Stellaris engine | The effect is not applied, or the block is rejected |
| Icon asset loader | Inbound | DDS texture whose path matches the declaration literally | Stellaris engine | A missing or placeholder icon is rendered; the modifier remains active |
| Localisation system | Inbound | `l_english` keys matching the effect key, with `_desc` variants | Stellaris engine | The raw identifier is displayed |
| Vanilla data space | Outbound | `pc_*`, `sc_*`, and `pm_*` identifiers referenced in conditions | Stellaris game data | A renamed or removed identifier makes the condition unevaluable |
| Steam Workshop | Outbound | Published workshop item for the mod | `.github/workflows/steam-workshop-release.yml` | Players cannot obtain or update the mod through that channel |
| GitHub Releases | Outbound | Release archive | `.github/workflows/github-release.yml` | The archive reflects the tagged revision, not uncommitted or untagged changes |

## Host integration in detail

### Launcher integration

The launcher is the only consumer of the descriptors. It reads `name` for display, `path` to locate content, `tags` for classification, and `supported_version` for the compatibility verdict. Nothing in the descriptors affects in-game behaviour beyond whether content is found.

### Definition-parser integration

Both definition files are parsed by the engine's Clausewitz parser. The parser is permissive about quoting, which is why the repository mixes `is_planet_class = "pc_barren"` with `is_planet_class = pc_barren`, but it is not permissive about block structure: an unbalanced brace invalidates the enclosing definition.

### Effect and icon integration

The effect file's `icon` values are resolved relative to the mod content directory. The value must therefore omit the `ui-planet-modifiers/` prefix even though the file resides inside that directory. This is the single most easily misread path in the repository.

### Localisation integration

The localisation file's byte-order mark and `l_english` document root are both required. The engine resolves keys by convention from the effect key, so no field links a modifier to its display text.

## Vanilla-data integration in detail

The mod references vanilla identifiers without defining them, which makes the following load-bearing and outside the repository's control:

| Referenced identifier category | Used for | Count |
|-------------------------------|----------|-------|
| Planet modifiers | Exclusion and reinforcement conditions | 12 distinct identifiers |
| Planet classes | Spawn gating | 13 distinct identifiers |
| Star classes | System-scope exclusion | 3 distinct identifiers |
| Static modifier effect keys | Override bindings | 9 distinct identifiers |

A change in any of these on the host side is invisible in the repository and manifests only as altered generation behaviour. Identifying such a change requires reading the engine log or observing generation.

## Distribution integrations

| Channel | Evidence | Status |
|---------|----------|--------|
| Steam Workshop | Item identifier `2072966604` for Stellaris app `281990`, titled `[DISCONTINUED] Universum Infinitum - Planet Modifiers`; `.github/workflows/steam-workshop-release.yml` uploads the tagged content on every `v*.*.*` tag push | Published but marked discontinued by its author, and reaches a Stellaris-specific audience through the Steam client |
| GitHub Releases | Release `v1.0` with archive `ui-planet-modifiers_1.0.zip`; `.github/workflows/github-release.yml` builds the archive for every published release | Published |

The Steam Workshop listing's title carries a `[DISCONTINUED]` prefix, which is an author-declared status rather than a repository artefact. It is a material signal for users and is recorded in [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md).

## Integrations that do not exist

- No Discord or bot integration.
- No telemetry or analytics endpoint.
- No mod-manager-specific metadata.
- No compatibility patch for another mod, despite conditions that reference other mods' identifiers such as `pm_titanic_life` and `pm_lush`. Those references are opportunistic and do not oblige the referenced mod's presence.

## Related documentation

- [dependencies.md](./dependencies.md) for the dependency inventory
- [configuration.md](./configuration.md) for the configuration sources
- [flows/game-startup-and-loading.md](./flows/game-startup-and-loading.md) for how the host integration executes
