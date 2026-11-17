# Logging

## Does the mod log?

No. The mod emits no log output, no metrics, no traces, and no telemetry. It contains no code and therefore cannot log.

The logging that matters for diagnosis belongs to the Stellaris engine, which reports parse and load problems in its own log files. This document records what the engine reports and how a contributor should interpret it, because that log is the only diagnostic channel for this repository.

## Where diagnostics originate

| Source | Content | Produced by |
|--------|---------|-------------|
| Engine log | Parse errors, load warnings, missing asset notices | Stellaris engine |
| Launcher | Compatibility verdict and mod listing state | Stellaris launcher |
| In-game interface | Raw localisation keys and missing icons | Stellaris engine, observed visually |
| Repository | Nothing | — |

## Diagnostic signals and their meaning

| Signal | Meaning | Repository source |
|--------|---------|-------------------|
| Definition filed under an invalid block | A `spawn_chance` block is malformed; the definition is discarded | `common/planet_modifiers/*.txt` |
| Unrecognised keyword | A condition or effect key is misspelled or missing for the target version | A definition or effect block |
| Effect key not found | A definition's `modifier` value names no effect block, or the host renamed a bound key | `modifier` field, or the override file's host bindings |
| Icon path not found | An `icon` value does not resolve relative to the content directory | `common/static_modifiers/*.txt` |
| Raw key displayed in the interface | A localisation key is missing | `localisation/english/*.yml` |
| Mod not listed | The launcher could not read a descriptor or resolve its `path` | Both descriptor files |
| Compatibility warning | The declared `supported_version` excludes the running game | Both descriptor files |

## Log levels

The repository defines none. The engine's own verbosity settings govern what appears. A contributor should raise the engine's log verbosity when diagnosing a load or parse problem, because the default level may suppress definition-level warnings entirely.

## Structured fields

Not applicable: the mod produces no structured log entries. Engine log entries are the engine's own format, not a contract this repository can rely on.

## Correlation

Not applicable. There is no request or session in which a repository artefact participates directly; correlation exists only within the engine's own diagnostic model.

## Redaction and masking

Not applicable for the mod, because it never logs. Worth recording explicitly: the mod handles no personal data, so there is nothing to redact and no redaction obligation arising from it. See [security.md](./security.md) and the root [PRIVACY.md](../PRIVACY.md).

## Retention

Not applicable within the repository. Engine log retention is host-owned and typically session-scoped or size-limited.

## Sampling

Not applicable. No sampling exists because no logging exists.

## Performance considerations

None. The mod performs no computation. Its files are read once at load time.

## Error logging

The mod cannot log errors. When a definition fails, the engine either discards the definition or records a warning in its log, depending on the failure severity. Because a discarded definition produces no in-game marker, the log is often the only way to distinguish "the definition was rejected" from "the modifier was simply never selected".

## Audit logging

Not applicable. The mod performs no security-relevant operations and generates no auditable events.

## Configuration

| Concern | Where to configure |
|---------|-------------------|
| Engine log verbosity and destination | Stellaris engine settings, outside the repository |
| Engine log file location | Engine-owned user data directory, outside the repository |
| Mod logging | Not configurable; the mod has none |

## Practical diagnostic procedure

1. Change one thing at a time in the definition, effect, icon, or localisation file.
2. Load the game with the engine's log verbosity raised.
3. Read the log for entries referring to the mod's files.
4. Corroborate by inspecting a generated planet in-game.
5. If the log is clean and the modifier is still absent, suspect a condition or exclusion rather than a load failure; see [troubleshooting.md](./troubleshooting.md).

## Related documentation

- [error-handling.md](./error-handling.md) for the failure taxonomy
- [troubleshooting.md](./troubleshooting.md) for symptom-based diagnosis
- [testing.md](./testing.md) for the manual verification procedure
