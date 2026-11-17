# Documentation index

Master navigation for the Universum Infinitum: Planet Modifiers knowledge base.

## Repository summary

Universum Infinitum: Planet Modifiers is a declarative Stellaris mod. It contributes four new planet modifiers, each with a spawn-chance definition, a static modifier effect block, a DDS icon, and an English localisation entry, and it also redefines the spawn chances of ten vanilla or mod-extended planet modifiers so that the new additions can coexist with them. The repository holds no executable code, no build step, and no tests: every artefact is a static definition file or an image asset consumed by the Stellaris engine, which owns all parsing, evaluation, rendering, and persistence.

## Root document map

| Document | Purpose |
|----------|---------|
| [README.md](../README.md) | What the mod provides, how to install it, and how to contribute |
| [ARCHITECTURE.md](../ARCHITECTURE.md) | System context, components, runtime processes, constraints, and extension points |
| [CONTRIBUTING.md](../CONTRIBUTING.md) | Issue reporting, enhancement suggestions, and the pull-request process |
| [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md) | Conduct expected in project spaces |
| [SECURITY.md](../SECURITY.md) | Supported distribution channels, reporting route, and coordinated disclosure policy |
| [PRIVACY.md](../PRIVACY.md) | Personal-data handling statement |
| [LICENSE](../LICENSE) | GNU General Public License version 3 |

## Documentation catalogue

### Orientation

| Document | Description |
|----------|-------------|
| [repository-overview.md](./repository-overview.md) | Purpose, scope, boundaries, and entry points of the repository |
| [repository-structure.md](./repository-structure.md) | Source-tree layout and the purpose of each path |
| [quick-start.md](./quick-start.md) | Getting started as a player or as a contributor |
| [faq.md](./faq.md) | Frequently asked questions |

### Architecture and data

| Document | Description |
|----------|-------------|
| [architecture.md](./architecture.md) | Architectural model that complements the root architecture document |
| [data-model.md](./data-model.md) | Planet modifiers, spawn conditions, effects, keys, and their relationships |
| [invariants.md](./invariants.md) | Invariants that must remain true across the definition set |
| [design-decisions.md](./design-decisions.md) | Why the definition set is shaped the way it is |
| [state-and-persistence.md](./state-and-persistence.md) | Which state exists, who owns it, and how it survives a save |
| [concurrency-and-scheduling.md](./concurrency-and-scheduling.md) | Evaluation timing, ordering, and multiplayer determinism |

### Components

| Document | Description |
|----------|-------------|
| [components/mod-metadata.md](./components/mod-metadata.md) | Root and local mod descriptors |
| [components/planet-modifier-definitions.md](./components/planet-modifier-definitions.md) | The four new spawn-chance definitions |
| [components/vanilla-spawn-overrides.md](./components/vanilla-spawn-overrides.md) | Re-declarations of two vanilla spawn chances, and exclusions that mutual exclusivity requires |
| [components/static-modifiers.md](./components/static-modifiers.md) | Effect blocks, icons, and icon frames, with every magnitude kept inside the v4.5 vanilla band |
| [components/icon-assets.md](./components/icon-assets.md) | DDS icons and the retained GIMP source |
| [components/localisation.md](./components/localisation.md) | English display names and descriptions |

### Flows and behaviour

| Document | Description |
|----------|-------------|
| [flows/game-startup-and-loading.md](./flows/game-startup-and-loading.md) | Registration, parsing, and loading order |
| [flows/planet-modifier-selection.md](./flows/planet-modifier-selection.md) | How the engine computes and applies a selection |
| [flows/modifier-presentation.md](./flows/modifier-presentation.md) | How a selected modifier reaches the interface |
| [behaviour/planet-modifier-generation.md](./behaviour/planet-modifier-generation.md) | Player-visible generation behaviour |
| [behaviour/planet-modifier-presentation.md](./behaviour/planet-modifier-presentation.md) | Player-visible presentation behaviour |

### Operations and quality

| Document | Description |
|----------|-------------|
| [dependencies.md](./dependencies.md) | Host, vanilla-data, and tooling dependencies |
| [integrations.md](./integrations.md) | Launcher, engine subsystems, and distribution platforms |
| [configuration.md](./configuration.md) | Everything that alters behaviour, and its source |
| [build-and-deployment.md](./build-and-deployment.md) | Packaging, distribution, and installation layout |
| [testing.md](./testing.md) | Verification strategy and coverage gaps |
| [logging.md](./logging.md) | Where diagnostics originate, and how to read them |
| [error-handling.md](./error-handling.md) | Failure taxonomy and observable consequences |
| [security.md](./security.md) | Trust model, threat surface, and mitigations |
| [troubleshooting.md](./troubleshooting.md) | Diagnostic procedures for recurring problems |
| [change-guide.md](./change-guide.md) | How to modify each area safely |
| [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) | Unresolved items and known gaps |
| [documentation-maintenance.md](./documentation-maintenance.md) | Keeping this knowledge base current |

## Navigation aids

- **Start here (newcomer):** [repository-overview.md](./repository-overview.md) → [quick-start.md](./quick-start.md) → [data-model.md](./data-model.md).
- **Deep dive (component owner):** [architecture.md](./architecture.md) → [components/](./components/planet-modifier-definitions.md) → [invariants.md](./invariants.md).
- **Flows (debugger):** [flows/game-startup-and-loading.md](./flows/game-startup-and-loading.md) → [flows/planet-modifier-selection.md](./flows/planet-modifier-selection.md) → [flows/modifier-presentation.md](./flows/modifier-presentation.md).
- **Change safely:** [change-guide.md](./change-guide.md) → [invariants.md](./invariants.md) → [testing.md](./testing.md).
- **Diagnosis:** [troubleshooting.md](./troubleshooting.md) → [error-handling.md](./error-handling.md) → [logging.md](./logging.md).

## Coverage status

| Area | Documented | Evidence base |
|------|-----------|---------------|
| Mod metadata | Complete | Both descriptors inspected |
| Planet modifier definitions | Complete | Both definition files inspected in full |
| Vanilla spawn overrides | Complete | Override file inspected in full |
| Static modifiers | Complete | Effect file inspected in full |
| Icon assets | Complete | Directory enumerated; texture format inspected |
| Localisation | Complete | Localisation file inspected in full |
| Engine selection semantics | Partially inferred | Descriptor and definition evidence only; engine internals not available |
| Distribution platforms | Partially verified | Steam Workshop item resolved through the Steam Web API; other channels unverified |

## Maintenance metadata

| Attribute | Value |
|-----------|-------|
| Last reviewed | 2026-11-17 |
| Owner | Repository maintainer |
| Reviewed against | Repository state at commit `6cc6de6` |
| Enforcement | Documentation is manual; no generation or validation pipeline exists |

## Related documentation

- Root architectural description: [ARCHITECTURE.md](../ARCHITECTURE.md)
- Maintenance procedure for this tree: [documentation-maintenance.md](./documentation-maintenance.md)
