# Documentation maintenance

## Purpose

How to keep this knowledge base accurate as the mod changes. The tree is hand-written; nothing generates or validates it.

## Ownership

| Area | Owner |
|------|-------|
| This `docs/` tree | Repository maintainer |
| Root GitHub documents (`README.md`, `ARCHITECTURE.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, `PRIVACY.md`) | Repository maintainer, governed by the template-fidelity rules |
| Definition, effect, icon, and localisation files | Repository maintainer |

## When to update which document

| Change | Documents to revise |
|--------|---------------------|
| New or removed planet modifier | [data-model.md](./data-model.md), [components/planet-modifier-definitions.md](./components/planet-modifier-definitions.md), [components/static-modifiers.md](./components/static-modifiers.md), [components/icon-assets.md](./components/icon-assets.md), [components/localisation.md](./components/localisation.md), [behaviour/planet-modifier-generation.md](./behaviour/planet-modifier-generation.md), [INDEX.md](./INDEX.md) |
| Changed weight or multiplier | [components/planet-modifier-definitions.md](./components/planet-modifier-definitions.md), [components/vanilla-spawn-overrides.md](./components/vanilla-spawn-overrides.md), [behaviour/planet-modifier-generation.md](./behaviour/planet-modifier-generation.md), [flows/planet-modifier-selection.md](./flows/planet-modifier-selection.md) |
| Changed effect value | [components/static-modifiers.md](./components/static-modifiers.md), [behaviour/planet-modifier-presentation.md](./behaviour/planet-modifier-presentation.md) |
| Added or changed exclusion | [invariants.md](./invariants.md), [flows/planet-modifier-selection.md](./flows/planet-modifier-selection.md), the relevant component document |
| Renamed an identifier | Every document naming the old identifier, plus [state-and-persistence.md](./state-and-persistence.md), [change-guide.md](./change-guide.md), [invariants.md](./invariants.md) |
| Added a locale | [components/localisation.md](./components/localisation.md), [configuration.md](./configuration.md), [INDEX.md](./INDEX.md) |
| Changed an icon | [components/icon-assets.md](./components/icon-assets.md) |
| Changed `supported_version` | [components/mod-metadata.md](./components/mod-metadata.md), [configuration.md](./configuration.md), [dependencies.md](./dependencies.md), [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) |
| Changed the content directory or path | [components/mod-metadata.md](./components/mod-metadata.md), [repository-structure.md](./repository-structure.md), [build-and-deployment.md](./build-and-deployment.md) |
| Resolved an open question | [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) and the document that recorded the relevant decision |
| Added distribution automation or a build step | [build-and-deployment.md](./build-and-deployment.md), [dependencies.md](./dependencies.md), [testing.md](./testing.md), [INDEX.md](./INDEX.md) |

## Synchronisation obligations

1. **Cross-document consistency.** Every document that states a count, an identifier, a weight, or a value must be updated when that fact changes. The inventory tables in [data-model.md](./data-model.md) and the per-modifier tables in the component documents are the most duplication-prone.
2. **Root document consistency.** If a change affects what the root [README.md](../README.md) or [ARCHITECTURE.md](../ARCHITECTURE.md) claims, those documents must be revised in the same change. The root documents are governed by their own template rules and are not duplicated here.
3. **Index consistency.** A new document must be added to the catalogue in [INDEX.md](./INDEX.md); a removed document must be removed from it.
4. **Invariant consistency.** An invariant that a change invalidates must be revised rather than left to contradict the files.

## Verification of this tree

| Check | Procedure |
|-------|-----------|
| No unresolved placeholder or template artefact | Search the tree for bracket tokens and HTML comments |
| Relative links resolve | Confirm every `./` and `../` link names an existing file |
| Counts agree | Confirm every stated count against the file it describes |
| Identifier spelling agrees | Confirm keys quoted in documentation exist in the source files |
| Coverage matches the tree | Confirm the catalogue in [INDEX.md](./INDEX.md) lists every file present |

## Review cadence

| Trigger | Action |
|---------|--------|
| Any change to a definition, effect, icon, or localisation file | Revise the affected documents in the same change |
| A new game version is targeted | Re-verify the referenced vanilla identifiers and revise [dependencies.md](./dependencies.md) and [integrations.md](./integrations.md) |
| An open question is resolved | Move the finding out of [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) into the appropriate document |
| A material repository change, such as adding automation | Re-reason about the affected architectural claims and revise [architecture.md](./architecture.md) |

## Known maintenance risks

| Risk | Mitigation |
|------|-----------|
| Inventory tables drift from the files | Treat the tables in [data-model.md](./data-model.md) as the single source of counts within this tree |
| Behaviour documents duplicate component detail | Keep behaviour documents observational and link to component documents for mechanism |
| Engine semantics are inferred, not verified | Label inferences as such; [flows/planet-modifier-selection.md](./flows/planet-modifier-selection.md) and [concurrency-and-scheduling.md](./concurrency-and-scheduling.md) rest largely on definition evidence |
| The tree can silently lag the repository | Record the reviewed commit in [INDEX.md](./INDEX.md) and update it on each review |

## Related documentation

- [INDEX.md](./INDEX.md) for the catalogue and coverage status
- [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) for items awaiting resolution
