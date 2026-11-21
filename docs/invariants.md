# Invariants

Invariants that must remain true for the definition set to behave as intended. None is enforced by a compiler, validator, or test; each is upheld only by contributor discipline, which is why the evidence and verification columns matter.

## File and identity invariants

| # | Invariant | Evidence | Consequence if violated | Verification |
|---|-----------|----------|------------------------|--------------|
| 1 | Both descriptors declare identical `name`, `path`, `tags`, and `supported_version` | Both files inspected; values are byte-identical | Behaviour diverges between manual and archive installs | Diff the two descriptor files |
| 2 | `path` resolves to the content directory containing `common/`, `gfx/`, and `localisation/` | `path="mod/ui-planet-modifiers"` and the directory exists at that name | No definition loads | Inspect `path` and the directory tree |
| 3 | The content directory name matches the descriptor `path` | `ui-planet-modifiers/` matches `mod/ui-planet-modifiers` | Every definition becomes invisible | Compare the directory name with the `path` field |

## Cross-file binding invariants

| # | Invariant | Evidence | Consequence if violated | Verification |
|---|-----------|----------|------------------------|--------------|
| 4 | Every definition's `modifier` field equals an existing effect key | All seven bindings verified: `pm_ui_salty` → `ui_salty`, and so on | The modifier is selectable but effectless | Cross-check the two definition files |
| 5 | Every effect-tier `icon` path resolves to an existing file | All seven paths verified against the icon directory | Missing or placeholder icon | Cross-check paths against the directory listing |
| 6 | Every icon file is referenced by exactly one effect block | Seven files, seven references, bijective | An unreferenced file is dead weight; a second reference would duplicate presentation | Compare the directory listing with the referenced paths |
| 7 | Every effect key has both a name key and a `_desc` key | All 14 localisation entries present for 7 keys | The interface displays a raw key | Compare effect keys with localisation keys |
| 8 | The localisation file retains its byte-order mark and its `l_english` root | Verified `EF BB BF` prefix and `l_english:` root | The whole file may fail to parse, orphaning all 8 keys | Inspect the first bytes and the first line |

## Exclusion and weighting invariants

| # | Invariant | Evidence | Consequence if violated | Verification |
|---|-----------|----------|------------------------|--------------|
| 9 | Mutually contradictory modifier pairs exclude each other in both directions | The thin/dense/extreme-temperature trio and the salty/swampy/toxic-soil trio each exclude their other two members; extreme temperatures and tidal locking exclude each other; the magnetic family shares an identifier list | A planet receives thematically contradictory modifiers | Read every `factor = 0` condition and confirm symmetry |
| 10 | Every definition the mod declares also excludes itself | Each new definition lists its own key | Lifecycle and accumulation anomalies | Search each definition for its own key |
| 11 | Every exclusion list naming a family names the whole family | The magnetic list contains every member, vanilla included | A partial list permits a conflicting combination | Compare each list against the family membership |
| 12 | Accumulation guards remain consistent with the modifier's intent | All seven definitions use the vanilla `num_modifiers > 1` guard | Unbounded modifier accumulation, or an unintended cap | Read the guard clause in each definition |
| 13 | Weights remain relative and deliberately calibrated | New definitions use `add = 10`, `add = 5`, or `add = 1`, matching vanilla's common, uncommon, and rare tiers | Unintended probability shifts across the entire pool | Review all `add` values together, not in isolation |

## Change-safety invariants

| # | Invariant | Evidence | Consequence if violated | Verification |
|---|-----------|----------|------------------------|--------------|
| 14 | Renaming a definition key is accompanied by updating every exclusion list that names it | Several definitions reference `pm_ui_no_magnetic_field`, `pm_ui_salty`, `pm_ui_swampy`, `pm_ui_toxic_soil`, `pm_ui_thin_atmosphere`, `pm_ui_dense_atmosphere`, and the magnetic siblings, including the re-declared vanilla keys | Exclusions silently stop firing | Search the repository for the old identifier |
| 15 | Renaming an effect key is accompanied by renaming the icon file, the localisation key, and the localisation `_desc` key | Effect keys drive icon naming and localisation resolution | Orphaned icons, raw keys in the interface, broken bindings | Search for the old key across all four data areas |
| 16 | Referenced vanilla identifiers remain valid for the declared game version | `pc_*`, `sc_*`, and `pm_*` identifiers referenced but not defined | Conditions become unevaluable and weights change silently | Compare against the target game version's own data |

## Host-contract invariants

| # | Invariant | Evidence | Consequence if violated | Verification |
|---|-----------|----------|------------------------|--------------|
| 17 | The two host-owned effect keys used by redefinitions continue to exist | `weak_magnetic_field` and `strong_magnetic_field` are bound but not defined | The corresponding definition resolves to no effect | Inspect the host's static modifiers for the target version |
| 18 | `supported_version` reflects a version against which the definitions were actually validated | It declares `v4.5.*`, matching the vanilla data the definitions were recalibrated against | Contributors and players may rely on an unverified compatibility claim | Confirm through in-game loading and generation testing |

## Invariants that deliberately do **not** hold

Recording these prevents a future reader from mistaking an inconsistency for a defect and "fixing" it.

| Observation | Status |
|-------------|--------|
| Identifier prefixes vary: `pm_ui_` plus no prefix | Intentional: definition keys carry `pm_`, effect keys do not |
| Effect keys use the `ui_` element, which derives from the repository name rather than the mod title | Intentional historical inheritance; see [ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) |
| Identifier quoting is uniform: every class, star, and modifier identifier is double-quoted | Enforced by convention only; the parser accepts the bare form too |

## Related documentation

- [architecture.md](./architecture.md) for the architectural context of these invariants
- [testing.md](./testing.md) for how invariants are currently verified, and the gaps
- [change-guide.md](./change-guide.md) for procedures that preserve them
