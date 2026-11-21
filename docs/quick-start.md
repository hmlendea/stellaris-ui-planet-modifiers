# Quick start

## As a player

1. Obtain the mod from a supported distribution channel: the Steam Workshop, the Nexus Mods page, or the release archive published on GitHub.
2. Activate it in the Stellaris launcher's playset for the intended game version.
3. Start a new game. The five new planet modifiers become eligible during galaxy generation; existing saves are unaffected because modifiers are assigned at generation time.
4. Inspect colonised and surveyed planets to confirm that modifier names, descriptions, and icons render correctly.

If a modifier does not appear at all, it is most often excluded by a condition rather than missing. See [troubleshooting.md](./troubleshooting.md).

## As a contributor

1. Clone the repository and open it in an editor. No dependency installation is required.
2. Familiarise yourself with the four data areas:
   - `ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers.txt` for new modifiers;
   - `ui-planet-modifiers/common/planet_modifiers/ui-planet-modifiers-vanillas.txt` for spawn-chance redefinitions;
   - `ui-planet-modifiers/common/static_modifiers/ui-planet-modifiers_static_modifiers.txt` for effects and icon bindings;
   - `ui-planet-modifiers/localisation/english/ui-planet-modifiers_l_english.yml` for display strings.
3. Place the content directory and the descriptor where the launcher expects them, then load the mod in the game to confirm the change.
4. Before proposing a change, read [invariants.md](./invariants.md) and [change-guide.md](./change-guide.md); the cross-file bindings are not enforced by any tool.

## Adding a modifier in five steps

1. Add a `pm_ui_<name>` block with its `spawn_chance` to the new-definitions file.
2. Add the matching `ui_<name>` effect block, including `icon` and `icon_frame`, to the static modifier file.
3. Add `pm_ui_<name>.dds` under `ui-planet-modifiers/gfx/interface/icons/planet_modifiers/`.
4. Add `ui_<name>` and `ui_<name>_desc` entries to the English localisation file.
5. Add `pm_ui_<name>` to every exclusion list that must preclude it, including the redefinitions file.

The detailed procedure, with the conventions that must be preserved, is in [change-guide.md](./change-guide.md).

## Minimum facts worth retaining

- Definition keys use the `pm_ui_` prefix; static modifier keys and localisation keys use the `ui_` prefix.
- The `modifier` field of a definition must equal the static modifier key exactly.
- Spawn weights are relative. Changing one weight changes every other modifier's effective probability.
- Description keys are the static modifier key with `_desc` appended.
- The descriptors declare `supported_version="v4.5.*"`.

## Related documentation

- [data-model.md](./data-model.md) for the entity relationships
- [testing.md](./testing.md) for how a change is verified
- [faq.md](./faq.md) for recurring inquiries
