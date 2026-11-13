# Changes in 2.0.0 (showcase rewrite)

Compared with the clean `mad-hatter` draft (the protocol, rabbit-hole levels, response shape, and guardrails) and the older "raw system prompt" variant.

## Kept

- The core seven-step protocol: outcome vs mechanism, assumptions, alternate surfaces, workable approaches only, ranking, disproof-first proof test, implement when actionable.
- Five rabbit-hole levels that increase distance from the assumption, not silliness.
- The compact response shape and the "one strong side door" rule.
- Light character: at most one wry line, technical language for evidence.

## Upgraded for Anthropic's bar

- **Discoverable description:** third person, what + when, trigger terms (Mad Hatter, MADFD, rabbit-hole, side door), under 1,024 characters. The raw variant's description ("RAW SYSTEM PROMPT") would never be selected.
- **When to use / when not to use:** so the skill does not fire on ordinary bugs (Eval 5) and does not treat security boundaries as optional assumptions.
- **Operational protocol:** assumptions marked verified vs inferred; ranking prefers verified, reversible, and cheap disproof; proof tests must be falsifiable, observationally honest, and authorized.
- **Progressive disclosure:** tight `SKILL.md`, with examples, surfaces, anti-patterns, and evaluations one level down — the Agent Skills recommended shape, under the 500-line body limit.
- **Worked examples at every level,** including a deliberate **Reject** case, replacing three short calibration notes.
- **Authorized surfaces checklist** with "prefer / brittle / out of scope," so "alternate surface" cannot be read as "bypass."
- **Five evaluation scenarios** (Anthropic's recommended minimum is three), written so a reviewer can pressure-test the skill.
- **Plugin bundle packaging** (`.claude-plugin/plugin.json`, Apache-2.0, slash command): skills are not a directory submission type on their own.

## Removed on purpose (these would fail review or waste context)

- Framing as a paste-into-system-prompt block, ASCII banners, and "END OF PROMPT."
- Personal runtime voice, nicknames, and build-tool visual mandates — those belong in a private skill, not a public listing.
- "Always assume proper authorization," "build regardless of ask," "skip validation," session-cookie substitution, hidden-endpoint fishing, traffic interception, and process-memory inspection.
- Mandatory exit catchphrases.
- A `minimum 5 assumptions` quota (replaced with "typically 3–7, until the route looks like a choice") and a literal 15-minute timer (replaced with "a test that can run in minutes").
