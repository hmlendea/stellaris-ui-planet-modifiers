# Security

This document complements the root [SECURITY.md](../SECURITY.md) by describing the mod's trust model, threat surface, and the reasoning behind its low risk profile. It does not restate the reporting policy.

## Trust model

| Party | Trust granted | Trust required |
|-------|---------------|----------------|
| Stellaris engine | Full: it reads and evaluates every file | The mod trusts the host to parse correctly and to sandbox mod content |
| Mod content in this repository | None beyond ordinary repository trust | Nothing is executed, so no privilege is delegated |
| Other mods in the same playset | None | None assumed; conditions referencing other mods' identifiers are opportunistic, not dependencies |
| Distribution platforms | None | None assumed; the platform is a transport |
| Players | None | No player input reaches the mod |

The mod holds no secrets, authenticates nothing, authorises nobody, and processes no personal data. Its entire interaction surface is a set of static files read by a sandboxed host.

## Threat surface

| Surface | Exposure | Mitigation |
|---------|----------|------------|
| Executable code | None exists | Not applicable |
| Network access | None | Not applicable |
| File system access beyond its own assets | None; the engine reads the declared content directory | Host sandbox |
| Personal data | None handled | See root [PRIVACY.md](../PRIVACY.md) |
| Credentials or secrets | None present | Not applicable |
| Dynamic content or user input | None | Not applicable |
| Dependency chain | No third-party code, no package manifest | Nothing to compromise at build time |
| Distribution integrity | An archive or workshop item could be tampered with | Verify provenance through the supported channels named in the root security policy |

## Failure modes with security relevance

| Mode | Assessment |
|------|-----------|
| Malformed definition text | The host parser handles it; worst case is a discarded definition. No memory-safety relevance to the repository. |
| Hostile identifier in a condition | A condition names an identifier; the engine resolves it in its own data space. No injection surface exists in the format. |
| Adversarial icon asset | A DDS texture is decoded by the host's loader. A malformed texture could in principle exercise a host-side decoder defect, but that is a host vulnerability, not a repository one. |
| Archive tampering | The genuine risk is a player obtaining a modified archive from an unofficial source. The mitigations are provenance and the supported-channel statement in the root security policy. |

## Why the mod's risk is structurally low

1. **No code.** Nothing in the repository executes. There is no scripting layer, no expression evaluator owned by the mod, and no callback.
2. **No I/O of its own.** The mod cannot read, write, or delete anything; the host decides what to read.
3. **No network.** There is no telemetry, no update check, and no remote configuration.
4. **No personal data.** The mod has no concept of a user.
5. **No dependency chain.** There is nothing to supply-chain compromise at build or restore time.

The residual risk is therefore concentrated in three places: the host's own handling of mod content, the integrity of distribution channels, and the accuracy of the mod's compatibility claim. Only the third is within the repository's influence, via `supported_version`.

## Supply-chain considerations

| Aspect | Status |
|--------|--------|
| Third-party dependencies | None |
| Build-time downloads | None |
| Release automation executing remote scripts | None |
| Signed artefacts | Not applicable |
| Provenance | Repository history, release tag `v1.0`, and the workshop item |

Because the repository performs no automated fetching or execution, the classic CI/CD supply-chain risks do not apply. The meaningful risk is a player installing a repackaged archive from an untrusted source.

## Privacy

The mod requests, collects, transmits, and stores no personal data. It has no accounts, no telemetry, no diagnostics, and no built-in transfers. The authoritative statement is the root [PRIVACY.md](../PRIVACY.md); this section exists only to record that the privacy position follows directly from the architectural one.

## Reporting

Vulnerabilities affecting the mod's definition files, icon assets, localisation, or distribution should be reported through the route defined in the root [SECURITY.md](../SECURITY.md). Issues in the game engine, mod loading, or third-party launchers are explicitly out of scope there.

## Related documentation

- [SECURITY.md](../SECURITY.md) for the policy and scope
- [PRIVACY.md](../PRIVACY.md) for the data-handling statement
- [dependencies.md](./dependencies.md) for the dependency inventory
- [integrations.md](./integrations.md) for the integration surfaces
