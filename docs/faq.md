# FAQ

## Is this an application?

No. It is a Stellaris mod: a set of static definition, localisation, and image files consumed by the game engine. There is no executable, no service, and no interface of its own.

## Do I need to build anything to use it?

No. Installation consists of placing the descriptor and the content directory where the launcher expects them, or subscribing through a mod platform.

## Does it require any dependencies?

Only the host game. There are no third-party libraries, no package manifest, and nothing to restore.

## What does it add?

Five new planet modifiers — extreme temperatures, absence of a magnetic field, a thin atmosphere, salinity, and swampland — each with its own icon and English localisation, plus two vanilla magnetic-field redefinitions so the new entry cannot coexist with them.

## Do the new modifiers make planets better?

They make planets worse. Every new modifier reduces habitability, and most also reduce happiness, growth, food production, or physics research. The mod adds variety and difficulty rather than power.

## Why does a planet already carrying a modifier seem to reject the new ones?

All five definitions carry a `num_modifiers > 1` condition evaluated with `factor = 0`, which excludes the modifier once another is present. This matches the vanilla convention and is uniform across the set.

## Can two contradictory modifiers appear together?

Not for the pairs the mod declares. Salty and swampy exclude each other, and the magnetic-field variants share an exclusion list. Extreme temperatures excludes `pm_tidal_locked` in one direction only, because the vanilla definition is no longer re-declared.

## Why can high gravity never appear on a small planet?

Because the vanilla v4.5 definition says so: it carries `factor = 0` when `planet_size < 12`. The mod does not re-declare `pm_high_gravity`, so that behaviour is entirely the host's.

## Why is tidal locking more likely on moons?

The redefinition applies `factor = 3` when the body is a moon, with a base weight of only `add = 1`. The moon multiplier is what makes the modifier observable.

## Will the mod change my existing save?

It will not retroactively add modifiers to already-generated planets, but effect values are resolved at load time, so rebalancing an effect alters the statistics of planets in saves that already carry it.

## What happens if I remove the mod from a save that used it?

The save retains the modifier identifiers. Without the mod, the engine cannot resolve their effects or localisation, so they may display as raw keys and confer nothing.

## Which game versions are supported?

Both descriptors declare `supported_version="v4.5.*"`. The definitions were rebalanced against the v4.5 vanilla planet modifier set; earlier release lines are untested.

## Are other languages available?

Only English. Localisation is additive, so a new locale requires a new file and no change to any existing file.

## Is the mod maintained?

The repository carries a published release and an active funding configuration, while the Steam Workshop listing is titled with a `[DISCONTINUED]` prefix. The repository does not state a maintenance status. See [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md).

## Does it collect any data?

No. It performs no network access, handles no personal data, and has no telemetry. See the root [PRIVACY.md](../PRIVACY.md).

## Can I use it in multiplayer?

Yes, provided every participant activates the same mod version. Divergent mod sets produce inconsistent presentation and statistics across clients.

## Where do I report a problem?

Through the project issue tracker for ordinary defects, and through the security policy route for security-sensitive reports. See [CONTRIBUTING.md](../CONTRIBUTING.md) and [SECURITY.md](../SECURITY.md).

## Related documentation

- [troubleshooting.md](./troubleshooting.md) for diagnosis
- [quick-start.md](./quick-start.md) for getting started
- [behaviour/planet-modifier-generation.md](./behaviour/planet-modifier-generation.md) for observed behaviour
