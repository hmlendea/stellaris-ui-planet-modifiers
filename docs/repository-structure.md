# Repository structure

## Layout

```text
stellaris-ui-planet-modifiers/
├── .github/
│   ├── FUNDING.yml                     # Funding platforms consumed by GitHub
│   └── workflows/
│       ├── github-release.yml          # Packages and publishes the release archive
│       ├── nexus-mods-release.yml      # Uploads the mod to Nexus Mods
│       └── steam-workshop-release.yml  # Uploads the mod to the Steam Workshop
├── src/
│   └── planet_modifier.xcf             # Editable GIMP source for the icon set
├── description/
│   └── steam-workshop.bbcode           # Steam Workshop page body (BBCode)
├── ui-planet-modifiers/
│   ├── descriptor.mod                  # Local mod descriptor
│   ├── common/
│   │   ├── planet_modifiers/
│   │   │   ├── ui-planet-modifiers.txt            # Five new planet modifiers
│   │   │   └── ui-planet-modifiers-vanillas.txt   # two re-declared vanilla spawn chances
│   │   └── static_modifiers/
│   │       └── ui-planet-modifiers_static_modifiers.txt  # Five effect blocks
│   ├── gfx/
│   │   └── interface/
│   │       └── icons/
│   │           └── planet_modifiers/   # Five DDS icon textures
│   ├── localisation/
│   │   └── english/
│   │       └── ui-planet-modifiers_l_english.yml
│   └── thumbnail.png                   # 400 x 400 launcher thumbnail
├── ui-planet-modifiers.mod             # Root mod descriptor
├── README.md
├── ARCHITECTURE.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── PRIVACY.md
├── LICENSE
└── docs/                               # This knowledge base
```

## Directory purposes

| Directory or file | Purpose |
|-------------------|---------|
| `src/` | Holds the editable GIMP source `planet_modifier.xcf` (60 x 60, RGB) from which the icon set is produced. Not consumed by the game. |
| `description/` | Holds the Steam Workshop page body `steam-workshop.bbcode`, which is pasted into the workshop item description. Not consumed by the game. |
| `ui-planet-modifiers/` | The mod content directory. The descriptor `path` field names this directory, so its name is part of the delivery contract. |
| `ui-planet-modifiers/common/` | Clausewitz definition files that the engine parses at initialisation. |
| `ui-planet-modifiers/common/planet_modifiers/` | Spawn-chance definitions. Two files: new additions and redefinitions. |
| `ui-planet-modifiers/common/static_modifiers/` | Effect definitions bound to icons and localisation keys. |
| `ui-planet-modifiers/gfx/interface/icons/planet_modifiers/` | DDS icon textures, one per new modifier, named `pm_ui_<name>.dds`. |
| `ui-planet-modifiers/localisation/english/` | English localisation, one language directory per locale. |
| `.github/` | GitHub platform configuration: the funding declaration and the `github-release.yml` release-packaging workflow, the `nexus-mods-release.yml` Nexus Mods publication workflow, and the `steam-workshop-release.yml` workshop-publication workflow. |
| `docs/` | This knowledge base. Not shipped with the mod. |

## Naming conventions observed by the repository

| Element | Convention | Example |
|---------|-----------|---------|
| Planet modifier definition key | `pm_ui_<name>` | `pm_ui_salty` |
| Static modifier key | `ui_<name>` | `ui_salty` |
| Icon file | `pm_ui_<name>.dds` | `pm_ui_salty.dds` |
| Localisation name key | Equals the static modifier key | `ui_salty` |
| Localisation description key | Static modifier key plus `_desc` | `ui_salty_desc` |
| Localisation file | `<mod>_l_<locale>.yml` | `ui-planet-modifiers_l_english.yml` |
| Definition file | `<mod>[_suffix].txt` | `ui-planet-modifiers.txt`, `ui-planet-modifiers-vanillas.txt` |

The `ui_` element in these names originates from the repository name rather than from the *Universum Infinitum* series name used by the mod title. See [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md).

## Structural observations

- There is no build directory, no dependency manifest, and no test directory. The only automation is `.github/workflows/github-release.yml`, which packages the archive when a release is published, and `.github/workflows/nexus-mods-release.yml` and `.github/workflows/steam-workshop-release.yml`, which upload the mod to Nexus Mods and the Steam Workshop when a `v*.*.*` tag is pushed or the workflow is manually dispatched; nothing is compiled or verified automatically.
- `ui-planet-modifiers/thumbnail.png` and `ui-planet-modifiers/descriptor.mod` are the only artefacts addressed at the launcher rather than at the game engine.
- `src/` sits outside the mod content directory deliberately, so that the GIMP source is not shipped as part of the mod content.

## Related documentation

- [components/](./components/mod-metadata.md) for per-component detail
- [build-and-deployment.md](./build-and-deployment.md) for the delivery layout
