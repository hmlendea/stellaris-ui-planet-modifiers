# Build and deployment

## Build

There is no build step. The repository contains no compiler input, no packaging script, and no task runner; the workflows only package and publish the committed content at release time. Every shipped artefact is either authored text or an authored texture.

| Activity | Status |
|----------|--------|
| Compilation | Not applicable |
| Bundling | Not applicable |
| Minification | Not applicable |
| Asset generation | Manual: DDS icons are exported from `src/planet_modifier.xcf` by an editor |
| Dependency restoration | Not applicable |
| Continuous integration | Release packaging only: `.github/workflows/github-release.yml` builds the archive on release publication, `.github/workflows/nexus-mods-release.yml` uploads the mod to Nexus Mods on `v*.*.*` tag pushes or manual dispatch, and `.github/workflows/steam-workshop-release.yml` uploads the mod to the Steam Workshop on `v*.*.*` tag pushes; no validation runs on changes |

## Delivery layout

The runtime delivery unit is the root descriptor plus the content directory:

```text
ui-planet-modifiers.mod        # Root descriptor; declares path="mod/ui-planet-modifiers"
ui-planet-modifiers/           # Content directory
├── descriptor.mod             # Self-describing copy for archives
├── common/
├── gfx/
├── localisation/
└── thumbnail.png
```

The `path` field is relative to the user mod location, so the layout above must be preserved exactly: the descriptor at the root of the delivered unit, and the content directory named `ui-planet-modifiers` beside it.

## Installation routes

| Route | Artefact | Notes |
|-------|----------|-------|
| Steam Workshop subscription | Published workshop item | The launcher manages placement and updates; the workshop listing is marked discontinued by its author |
| Nexus Mods download | `ui-planet-modifiers.zip` uploaded by `.github/workflows/nexus-mods-release.yml` | The platform manages placement and updates; the upload archives the previous version and sets the mod version from the tag |
| GitHub release archive | `ui-planet-modifiers_1.0.zip` from release `v1.0` | Verified to contain `ui-planet-modifiers.mod` and `ui-planet-modifiers/descriptor.mod` at the archive root plus the content directory, 25 entries totalling about 252 KiB uncompressed |
| Manual from source | Repository clone | The clone can be placed directly where the launcher looks, or the content directory can be copied into an existing mod location |

The verified archive layout matches the delivery layout above exactly: both descriptors ship at the archive root, the content directory ships alongside them, and `src/` is excluded.

## Installation procedure (manual)

1. Locate the Stellaris user mod directory.
2. Place `ui-planet-modifiers.mod` there, or place the release archive's equivalent.
3. Place the `ui-planet-modifiers/` content directory beside the descriptor.
4. Activate the mod in the launcher's playset.
5. Start a new game to observe the modifiers.

## Verification after installation

| Check | Expected result |
|-------|-----------------|
| Launcher listing | *Universum Infinitum - Planet Modifiers* appears with the Galaxy Generation tag |
| Compatibility verdict | Consistent with the declared `v4.5.*` version against the running game |
| In-game inspection | Generated planets show the new modifiers with names, descriptions, and icons rather than raw keys |
| Engine log | No parse errors relating to the mod's files |

## Release process

The archive is produced by `.github/workflows/github-release.yml`, which runs when a GitHub release is published, checks out the tagged revision, derives the version from the tag with the leading `v` removed, zips `ui-planet-modifiers.mod` and `ui-planet-modifiers/`, and uploads the result as `ui-planet-modifiers_<version>.zip` to that release. The Nexus Mods copy is published by `.github/workflows/nexus-mods-release.yml`, which runs when a `v*.*.*` tag is pushed or the workflow is manually dispatched with a tag: it checks out that tag, sets `version` in both descriptors, zips the descriptor and content directory into `release.zip`, and uploads it to file `8417199` as `ui-planet-modifiers.zip` with the changelog link. The workshop item is published by `.github/workflows/steam-workshop-release.yml`, which runs when a `v*.*.*` tag is pushed, zips and extracts the same descriptor and content directory, and uploads the extracted content directory to the Steam Workshop item with a changenote linking the matching release tag. All therefore match the tagged repository state, but nothing prevents a tag or release from being created on a revision that was never validated in game.

## Deployment topology

| Concern | Design |
|---------|--------|
| Processes | None; the host loads files in-process |
| Services | None |
| Persistent state | None owned by the mod; assigned modifiers persist in the engine's save file |
| Scaling | Inherits the host model; multiplayer requires identical mod activation on every client |
| Resource footprint | Static text plus eight 15 KiB textures and a 400 x 400 thumbnail |
| Migration mechanism | None; see [state-and-persistence.md](./state-and-persistence.md) |
| Rollback | Replace the installed content with an earlier release, or deactivate the mod; existing saves retain any identifiers already recorded |

## Operational risks

| Risk | Impact | Mitigation available within the repository |
|------|--------|------------------------------------------|
| Declared version drift | The mod may behave differently on a newer engine | None; only in-game verification |
| Archive divergence | Players receive content that differs from the repository | None; the release workflow archives the tagged revision, so the archive matches the tag |
| Identifier removal | Existing saves retain dangling identifiers | Avoid removing or renaming identifiers |
| Host identifier rename | Conditions become unevaluable | None; requires a mod update |

## Related documentation

- [state-and-persistence.md](./state-and-persistence.md) for the save-file implications
- [integrations.md](./integrations.md) for the distribution channels
- [components/mod-metadata.md](./components/mod-metadata.md) for the descriptor contract
