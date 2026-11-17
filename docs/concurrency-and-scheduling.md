# Concurrency and scheduling

## Is there concurrency in the repository?

No. The repository contains no threads, no tasks, no timers, no events, and no asynchronous constructs of any kind. It is static data.

Concurrency exists only inside the host engine, which evaluates definitions at deterministic points during game initialisation and planet generation. This document explains which host-side timing properties the definitions implicitly depend on, because those properties determine whether the mod is safe in a multiplayer session and whether exclusion symmetry is required.

## The relevant host-side timing

| Phase | When it runs | What the definitions depend on |
|-------|--------------|--------------------------------|
| Load | Once, at game start | Nothing about timing; definitions are keyed by identity |
| Generation | During galaxy generation and later planet creation | Planets are produced by a parallelisable process; each planet's evaluation is independent |
| Re-evaluation | When a planet's modifier set changes | `has_planet_modifier` conditions observe the current assignment |

## Why order independence was engineered

A planet's modifier set is built by repeated selection. Each iteration can observe the modifiers already assigned, because conditions use `has_planet_modifier`. If a process can evaluate candidates in an unspecified order, a one-sided exclusion produces two different planets from identical inputs depending on scheduling.

The repository avoids that outcome by writing symmetric exclusions where a conflict is introduced:

| Pair | Both directions declared |
|------|-------------------------|
| Salty and swampy | Yes |
| Extreme temperatures and `pm_tidal_locked` | No; only the mod's side excludes, because the vanilla definition is not re-declared |
| Each magnetic-family member against the others | Yes, through an identical identifier list, closed from the vanilla side by the override file |

For the two asymmetric rows, the accumulation guard (`num_modifiers > 1`) prevents the combination in practice rather than a declared conflict, so the outcome is still order-independent for the guarded modifiers.

## Determinism in multiplayer

| Property | Implication |
|----------|-------------|
| Definitions are identical across clients | Every client that loads the same mod set resolves the same identifiers to the same effects |
| Divergent mod sets | A client without the mod cannot resolve the identifiers recorded by the host, producing inconsistent presentation and a desynchronised view of planet statistics |
| Host-side selection randomness | This is a host property; the definitions do not influence whether selection is seeded, only the distribution it samples |
| Symmetric exclusions | Reduce the chance that a client's evaluation differs from the host's for the reasons the mod controls |

The mod's operational consequence is simple: host and clients must activate the same version of the mod. This is standard Stellaris multiplayer behaviour rather than a property of this repository, but it is the reason the compatibility statement in [configuration.md](./configuration.md) matters.

## Scheduling the mod does not control

| Concern | Owner |
|---------|-------|
| Whether planet generation is parallel | Stellaris engine |
| The order in which a planet's modifiers are drawn | Stellaris engine |
| Whether selection is seeded | Stellaris engine |
| When a planet's modifier set is re-evaluated after a game event | Stellaris engine |
| Whether definitions are cached between sessions | Stellaris engine |

None of these is observable from the repository, and none can be changed from it. The correct engineering response, which the definitions follow, is to write conditions that produce the same outcome regardless of the order in which they are evaluated.

## Reflexive exclusions

Every definition lists its own identifier among its exclusions, including the two re-declared vanilla keys. A self-exclusion guarantees that a planet never receives the same modifier twice, which would otherwise be possible because `has_planet_modifier` is evaluated per candidate. This is the mod's only guard against duplication.

## Related documentation

- [flows/planet-modifier-selection.md](./flows/planet-modifier-selection.md) for the evaluation steps
- [invariants.md](./invariants.md) for the exclusion-symmetry invariant
- [design-decisions.md](./design-decisions.md) for why symmetry was chosen
