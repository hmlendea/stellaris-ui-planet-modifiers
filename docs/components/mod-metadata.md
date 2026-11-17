# Component: mod metadata

## Responsibility

Declare the mod's identity, content path, content tags, and the game version against which it was validated, in the form the Stellaris launcher consumes.

## Location

| Path | Role |
|------|------|
| `ui-planet-modifiers.mod` | Root descriptor; the file a launcher reads from the user mod directory |
| `ui-planet-modifiers/descriptor.mod` | Local descriptor inside the content directory; the copy shipped in archives |

## Content

Both files are byte-identical in their field values:

```text
name="Universum Infinitum - Planet Modifiers"
path="mod/ui-planet-modifiers"
tags={
    "Galaxy Generation"
}
supported_version="2.6.*"
```

| Field | Value | Meaning |
|-------|-------|---------|
| `name` | `Universum Infinitum - Planet Modifiers` | Display name in the launcher and mod browser |
| `path` | `mod/ui-planet-modifiers` | Relative content location; must name the content directory |
| `tags` | `Galaxy Generation` | Single classification tag shown in the mod browser |
| `supported_version` | `2.6.*` | Declared compatible game version range |

## Why two descriptors exist

The launcher resolves a mod by reading a `.mod` file whose `path` field points at the content directory. The same metadata also needs to travel inside a distributable archive so that an unpacked archive is self-describing. Keeping an identical copy inside the content directory satisfies both uses without a generation step, at the cost of a manual duplication that nothing enforces.

## Invariants

1. `name`, `path`, `tags`, and `supported_version` must be identical in both descriptors. Divergence produces two different registrations of the same mod depending on how it was installed.
2. `path` must resolve to the directory that actually contains `common/`, `gfx/`, and `localisation/`. The `mod/` prefix is relative to the user mod location, not to the repository.
3. `supported_version` is the only field that gates the launcher's compatibility verdict.

## Failure behaviour

| Failure | Observable result |
|---------|-------------------|
| `path` does not resolve | The mod is listed but its content is not loaded, or the mod is not listed at all |
| Descriptors disagree | Behaviour differs between a manual install and an archive install of the same release |
| `supported_version` excludes the running game | The launcher flags the mod as incompatible; loading it may still be permitted |
| Content directory renamed without updating `path` | Every definition becomes invisible to the engine |

## Change impact

Changing `name` alters the launcher entry and any documentation referencing the display name. Changing `path` or renaming the content directory invalidates the distribution layout described in [build-and-deployment.md](../build-and-deployment.md). Raising `supported_version` asserts validation against a newer game version and should follow an actual in-game verification pass. See [change-guide.md](../change-guide.md).

## Related documentation

- [components/planet-modifier-definitions.md](./planet-modifier-definitions.md) for the content the path resolves to
- [flows/game-startup-and-loading.md](../flows/game-startup-and-loading.md) for how the descriptors are consumed
