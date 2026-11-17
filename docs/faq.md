# FAQ

## Is this an application?

No. It is a Stellaris mod: a set of static definition, localisation, and image files consumed by the game engine. There is no executable, no service, and no interface of its own.

## Do I need to build anything to use it?

No. Installation consists of placing the descriptor and the content directory where the launcher expects them, or subscribing through a mod platform.

## Does it require any dependencies?

Only the host game. There are no third-party libraries, no package manifest, and nothing to restore.

## What does it add?

Eight new planet modifiers — perpetual twilight, extreme temperatures, absence of a magnetic field, salinity, swampland, microorganisms, giant earthworms, and sea monsters — each with its own icon and English localisation, plus revised spawn chances for existing modifiers so the enlarged pool stays consistent.

## Do the new modifiers make planets better?

Mostly worse. Seven of the eight reduce habitability, happiness, growth, or food production; giant earthworms and sea monsters offset their habitability penalty with a society research bonus, and microorganisms changes nothing at all. The mod adds variety and difficulty rather than power.

## Why is microorganisms presented prominently if it changes nothing?

Its effect block sets `important = yes` with no effect keys, so it occupies a modifier slot and is presented prominently while conferring no statistic change. Its accumulation guard is also the most permissive of the set, allowing it to join a planet that already carries two modifiers.

## Why does a planet already carrying a modifier seem to reject the new ones?

Seven of the eight definitions carry a `num_modifiers > 1` condition evaluated with `factor = 0`, which excludes the modifier once another is present. `pm_ui_swampy` has no such guard, and microorganisms relaxes the threshold to two.

## Can two contradictory modifiers appear together?

Not for the pairs the mod introduces. Twilight and extreme temperatures exclude each other, the magnetic-field variants share an identical exclusion list, and the gravity variants share theirs. Accumulation guards handle the remaining combinations.

## Why can high gravity never appear on a small planet?

The high-gravity redefinition includes `factor = 0` when `planet_size < 12`. Low gravity has no corresponding hard floor, only a penalty factor above size 19.

## Why is tidal locking more likely on moons?

The redefinition applies `factor = 3` when the body is a moon, with a base weight of only `add = 1`. The moon multiplier is what makes the modifier observable.

## Why do giant earthworms and sea monsters have identical effects?

They share an effect block deliberately, differing only in spawn conditions and icon. Their differentiation is thematic rather than mechanical.

## Will the mod change my existing save?

It will not retroactively add modifiers to already-generated planets, but effect values are resolved at load time, so rebalancing an effect alters the statistics of planets in saves that already carry it.

## What happens if I remove the mod from a save that used it?

The save retains the modifier identifiers. Without the mod, the engine cannot resolve their effects or localisation, so they may display as raw keys and confer nothing.

## Which game versions are supported?

Both descriptors declare `supported_version="2.6.*"`, which predates the current release line. The claim has not been verified by any repository artefact, so treat newer versions as untested.

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
