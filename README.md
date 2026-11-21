[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)
[![Latest Release](https://img.shields.io/github/v/release/hmlendea/stellaris-ui-planet-modifiers)](https://github.com/hmlendea/stellaris-ui-planet-modifiers/releases/latest)
[![License](https://img.shields.io/github/license/hmlendea/stellaris-ui-planet-modifiers)](https://github.com/hmlendea/stellaris-ui-planet-modifiers/blob/master/LICENSE)

# Universum Infinitum - Planet Modifiers

A Stellaris mod that adds eight new planet modifiers — extreme temperatures, absence of a magnetic field, a thin atmosphere, a dense atmosphere, toxic soil, geological instability, salinity, and swampland — each with its own icon and English localisation, weighted to match the vanilla v4.5 planet modifier pool.

## 📑 Table of Contents

- [Capabilities](#capabilities)
- [Usage](#usage)
- [Known Limitations](#known-limitations)
- [System Requirements](#system-requirements)
- [Installation](#installation)
  - [Manual Installation](#manual-installation)
  - [Verification](#verification)
  - [Uninstallation](#uninstallation)
- [Localisation](#localisation)
- [Compatibility](#compatibility)
- [Privacy and Data](#privacy-and-data)
- [GitHub Actions](#github-actions)
- [Project Structure](#project-structure)
  - [Directories](#directories)
- [Architecture](#architecture)
- [Documentation](#documentation)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [Contributing](#contributing)
- [Code of Conduct](#code-of-conduct)
- [Related Projects](#related-projects)
- [Security](#security)
- [Support](#support)
- [Project Engagement](#project-engagement)
- [License](#license)

## ✨ Capabilities

- Adds eight new planet modifiers with distinct names, descriptions, and icons.
- Gates each new modifier with planet-class and star-system conditions so that it appears on thematically plausible worlds.
- Excludes mutually contradictory modifiers, so a planet never carries both a tidal lock and extreme temperatures, or two magnetic-field variants.
- Excludes the new modifiers from vanilla modifiers that already cover the same theme, so no duplicate concept enters the pool.
- Reproduces the vanilla v4.5 spawn conditions for the two vanilla modifiers it re-declares, changing only their exclusions.
- Scales every effect within the vanilla v4.5 band for comparable modifiers.
- Caps modifier accumulation per planet.
- Provides English localisation for every new modifier.

## 🚀 Usage

Activate the mod in the Stellaris launcher's playset for the intended game version, then start a new game. The new modifiers become eligible during galaxy generation; already-generated planets are unaffected because modifiers are assigned when a planet is created.

Inspect surveyed or colonised planets to confirm that each modifier presents its localised name, description, and icon rather than a raw identifier.

## ⚠️ Known Limitations

- The descriptors declare `supported_version="v4.5.*"`; other release lines are untested.
- Spawn chances are relative, so changing one modifier's weight shifts the effective probability of every other candidate.
- The mod references vanilla planet-class, star-class, and planet-modifier identifiers that it neither defines nor validates, so a host-side rename changes eligibility silently.
- Modifier identifiers are recorded in save files; renaming or removing one leaves existing saves with dangling references.
- No automated consistency check exists for the bindings between definitions, effects, icons, and localisation keys.
- Verification requires the host game, because the repository contains no tests.

## 🖥️ System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| Stellaris | `v4.5.*` as declared by the mod descriptors | A game version verified in practice against these definitions |

## 📦 Installation

[![Obtain it from Nexus Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://nexusmods.com/stellaris/mods/223)
[![Obtain it from Steam Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=2072966604)
[![Obtain it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/stellaris-ui-planet-modifiers/releases)

### Manual Installation

1. Download the release archive from the link above and unpack it.
2. Place `ui-planet-modifiers.mod` in the Stellaris user mod directory.
3. Place the `ui-planet-modifiers/` content directory beside the descriptor.
4. Activate the mod in the launcher's playset.

### Verification

After activation, the launcher lists *Universum Infinitum - Planet Modifiers* with the Galaxy Generation tag, and a newly generated planet carrying one of the new modifiers displays a narrative name and description instead of a raw identifier such as `ui_salty`.

### Uninstallation

Deactivate the mod or remove the installed files. Existing saves retain any modifier identifiers they recorded, so planets that received a new modifier keep a reference the game can no longer resolve unless the mod is restored.

## 🌍 Localisation

Display names and descriptions are supplied through the host localisation system, keyed by modifier identity.

| Language | Locale | Status |
|----------|--------|--------|
| English | `en` | Complete: one name and one description for each of the eight new modifiers |

## 🧩 Compatibility

| Component | Supported Versions | Notes |
|-----------|--------------------|-------|
| Stellaris | `v4.5.*` as declared | Other release lines are unverified |
| Multiplayer sessions | Same mod version on every client | Divergent mod sets produce inconsistent presentation and statistics |
| Other mods | No dependency | Conditions reference identifiers such as `pm_titanic_life` and `pm_lush` opportunistically; no other mod is required |

## 🛡️ Privacy and Data

Universum Infinitum - Planet Modifiers does not request, collect, transmit, or store personal data; its files define planet modifiers for Stellaris.

For the detailed description of how the application handles privacy and personal data, see the [Privacy Statement](./PRIVACY.md).

The mod defines no data categories: it has no accounts, no telemetry, no diagnostics, and no built-in transfers of personal data.

## ⚙️ GitHub Actions

| Workflow | Purpose | What it does |
|----------|---------|--------------|
| [ GitHub Release ](https://github.com/hmlendea/stellaris-ui-planet-modifiers/blob/master/.github/workflows/github-release.yml) | Publish the mod archive for a release | Runs when a release is published: checks out the tagged revision, strips the leading `v` from the tag to derive the version, zips `ui-planet-modifiers.mod` and `ui-planet-modifiers/` into `ui-planet-modifiers_<version>.zip`, and uploads the archive to the release |
| [ Nexus Mods ](https://github.com/hmlendea/stellaris-ui-planet-modifiers/blob/master/.github/workflows/nexus-mods-release.yml) | Publish the mod to Nexus Mods | Runs when a `v*.*.*` tag is pushed or manually dispatched: checks out the tag, sets `version` in both descriptors, zips `ui-planet-modifiers.mod` and `ui-planet-modifiers/` into `release.zip`, and uploads it to Nexus Mods file `8417199` as `ui-planet-modifiers.zip` with a changelog link |
| [ Steam Workshop ](https://github.com/hmlendea/stellaris-ui-planet-modifiers/blob/master/.github/workflows/steam-workshop-release.yml) | Publish the mod to the Steam Workshop | Runs when a `v*.*.*` tag is pushed or manually dispatched: checks out the tag, sets `version` in both descriptors, zips `ui-planet-modifiers/` and `ui-planet-modifiers.mod`, extracts the archive, and uploads the content directory to the workshop item with a changenote linking the matching GitHub release tag |

## 🗂️ Project Structure

The repository is a Stellaris mod: a descriptor plus a content directory of definition, localisation, and image files, alongside the editable icon source and the documentation.

### Directories

| Directory | Purpose |
|-----------|---------|
| `ui-planet-modifiers/` | The mod content directory named by the descriptor `path` field |
| `ui-planet-modifiers/common/planet_modifiers/` | Spawn-chance definitions: eight new modifiers and two vanilla redefinitions |
| `ui-planet-modifiers/common/static_modifiers/` | Effect blocks, icon bindings, icon frames, and importance flags |
| `ui-planet-modifiers/gfx/interface/icons/planet_modifiers/` | DDS icon textures, one per new modifier |
| `ui-planet-modifiers/localisation/english/` | English display names and descriptions |
| `src/` | Editable GIMP source for the icon set; not shipped with the mod |
| `description/` | Steam Workshop page body (`steam-workshop.bbcode`); not shipped with the mod |
| `docs/` | Documentation knowledge base |

## 🏗️ Architecture

Universum Infinitum - Planet Modifiers is a declarative Stellaris mod that defines planet modifiers, their spawn conditions, static effects, icons, and localisation through static data files consumed by the game engine.

See the [Architecture Documentation](./ARCHITECTURE.md) for the system context, principal components, runtime processes, ownership boundaries, dependencies, constraints, and extension points.

## 📚 Documentation

The documentation knowledge base describes the definition model, the generation and presentation flows, the invariants that must hold, and the procedures for changing each area safely.

| Resource | Description |
|----------|-------------|
| [docs/INDEX.md](./docs/INDEX.md) | Master index, navigation aids, and coverage status for the documentation |
| [docs/quick-start.md](./docs/quick-start.md) | Getting started as a player and as a contributor |
| [docs/architecture.md](./docs/architecture.md) | Architectural model, the two definition tiers, and the selection semantics |
| [docs/data-model.md](./docs/data-model.md) | Modifier records, spawn conditions, effects, and their name-based relationships |
| [docs/invariants.md](./docs/invariants.md) | Invariants that must remain true across the definition set |
| [docs/flows/planet-modifier-selection.md](./docs/flows/planet-modifier-selection.md) | How the engine computes and applies a modifier selection |
| [docs/change-guide.md](./docs/change-guide.md) | Safe procedures for editing definitions, effects, icons, and localisation |
| [docs/testing.md](./docs/testing.md) | Verification strategy and the coverage gaps |

## 🩺 Troubleshooting

See [Troubleshooting Guide](./docs/troubleshooting.md) for diagnostic procedures and resolutions to recurring problems.

## ❓ FAQ

See the [FAQ](./docs/faq.md) for responses to frequently raised inquiries.

## 🤝 Contributing

You are welcome to submit any suggestion, feedback, or modification to this project.

When doing so, please:
- Preserve the existing public contract unless a breaking change is intentional
- Submit focused pull requests that conform to the existing code style
- Maintain your branch synchronised with `master`
- Revise the documentation when functionality changes
- Raise a new [issue](https://github.com/hmlendea/stellaris-ui-planet-modifiers/issues) for problems or suggestions

See the [Contributing Guidelines](./CONTRIBUTING.md) for details on how to report issues, suggest enhancements, and submit changes.

## 📜 Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md) for the standards of conduct expected in all project spaces.

## 🔗 Related Projects

- [Universum Infinitum - Character Traits](https://github.com/hmlendea/stellaris-ui-character-traits): Adds many new character traits to the game.
- [Universum Infinitum - Deposit Blockers](https://github.com/hmlendea/stellaris-ui-deposit-blockers): Adds seven planet deposit blockers and one matching society technology to clear each of them.
- [Universum Infinitum - Flags](https://github.com/hmlendea/stellaris-ui-flags): Adds hundreds of new flag designs matching the vanilla flag style.
- [Universum Infinitum - Galaxy Sizes](https://github.com/hmlendea/stellaris-ui-galaxy-sizes): Adds six galaxy sizes ranging from 100 to 2,000 stars, with scaled generation defaults.
- [Universum Infinitum - Name Lists](https://github.com/hmlendea/stellaris-ui-name-lists): Provides numerous detailed name lists, including the Human - Extended variant.
- [Universum Infinitum - Planet Modifiers](https://github.com/hmlendea/stellaris-ui-planet-modifiers): Adds eight new planet modifiers with their own icons and English localisation.
- [Universum Infinitum - Prescripted Countries](https://github.com/hmlendea/stellaris-ui-prescripted-countries): Adds prescripted countries for players to select or for the AI to use.
- [Universum Infinitum - Species Names](https://github.com/hmlendea/stellaris-ui-species-names): Adds hundreds of new species names to the game.
- [Universum Infinitum - Species Traits](https://github.com/hmlendea/stellaris-ui-species-traits): Adds 58 custom species traits and extends several trait families with additional tiers.
- [Universum Infinitum - Star Names](https://github.com/hmlendea/stellaris-ui-star-names): Adds thousands of new names for stars, nebulas, asteroids, and black holes.

## 🔒 Security

Universum Infinitum - Planet Modifiers consists of Stellaris mod definition, localisation, and image files and does not execute code or handle user data.

For information on reporting security vulnerabilities, see the [Security Policy](./SECURITY.md).

## 💬 Support

For support, please [open an issue](https://github.com/hmlendea/stellaris-ui-planet-modifiers/issues).

## 💝 Project Engagement

Discovered a problem with Universum Infinitum - Planet Modifiers or have a suggestion for it? [Open an issue](https://github.com/hmlendea/stellaris-ui-planet-modifiers/issues)!

If you find this project useful, consider [funding it](https://hmlendea.go.ro/funding) or starring ⭐️ it on GitHub, or giving it a thumbs up and adding it as a favourite on the [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=2072966604)!

[![Donate](https://raw.githubusercontent.com/hmlendea/readme-assets/master/donate_generic.png)](https://hmlendea.go.ro/funding)

## 📄 License

This project is being distributed under the `GNU General Public License version 3`.
See [LICENSE](./LICENSE) for additional information.