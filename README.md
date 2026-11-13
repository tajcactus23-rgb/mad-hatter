# Mad Hatter

An Agent Skill for blocked coding work: it separates the outcome from the requested mechanism, exposes hidden assumptions, and proposes one overlooked approach that is authorized, plausible, and testable.

This is a methodology skill, not a toolkit. It teaches an agent *how to think* when a path looks impossible, and *when to refuse* if the only remaining idea is a security bypass.

## What’s in this package

```text
mad-hatter/
├── .claude-plugin/plugin.json    # Claude plugin manifest (directory submission)
├── skills/mad-hatter/
│   ├── SKILL.md                  # What the agent loads
│   └── references/
│       ├── examples.md           # One worked case per rabbit-hole level, plus a reject
│       ├── surfaces.md           # Authorized alternate-surface checklist
│       ├── anti-patterns.md      # Failure modes to revise or reject
│       └── evaluations.md        # Five pressure tests for quality review
├── commands/mad-hatter.md        # Optional slash-command entry point
├── LICENSE                       # Apache-2.0
└── README.md
```

The skill folder (`skills/mad-hatter/`) is self-contained and follows the [Agent Skills](https://agentskills.io/specification) specification. The plugin wrapper is what [Anthropic’s directory](https://claude.com/docs/directory/publish) expects: skills are not a submission type on their own.

## Install locally

**Claude Code (plugin):** from this repository root, `claude plugin validate .` then install from the local folder or from a marketplace entry you publish.

**Any Agent Skills runtime:** copy `skills/mad-hatter/` into the runtime’s skills directory (for example `~/.claude/skills/mad-hatter/`, or `~/.agents/skills/mad-hatter/` as a cross-runtime alias).

Invoke it by asking for a Mad Hatter / rabbit-hole / side-door analysis on a blocked task, or with `/mad-hatter` in Claude Code once the plugin is loaded.

## What a good run looks like

The agent should return this shape, then implement if you asked for more than ideas:

- **The lock** — what is blocked
- **The hidden assumption** — what was treated as mandatory
- **The side door** — one overlooked approach, with evidence
- **Proof test** — a cheap experiment with success *and* failure signals
- **Risks** — security, brittleness, false positives
- **Recommendation** — proceed, keep as fallback, or reject

If the only leftover idea would bypass auth, access control, or another security boundary, the correct answer is **reject**.

## Submitting to Anthropic’s directory

1. Push this folder to a **public** GitHub repository (required before a listing can go live). Keep `skills/mad-hatter/` and `.claude-plugin/plugin.json` at the plugin root, or set the plugin path in the portal to this folder.
2. Fill in `author` (and optional `homepage`, `repository`, `icon`) in `.claude-plugin/plugin.json`.
3. From a paid Claude plan (Pro, Max, Ultra, Team, or Enterprise), open [claude.ai/directory/manage](https://claude.ai/directory/manage), choose **Submit new** → **Plugin bundle**, and point it at that repository and path.
4. Anthropic runs automated validation and a security scan on every version, and a person reviews a new listing. Review is not instant; status lives in the portal.

This skill is written to survive that review: no fabricated APIs, no credential or session reuse, no traffic interception, no “assume authorization,” and a dedicated rejection eval (Eval 3 in `references/evaluations.md`).

`anthropics/skills` is an example repository, not a public contribution queue. A directory plugin bundle is the supported way to get a public Anthropic listing.

## Design notes

The protocol is intentionally narrow. It is for *blocked* work. Ordinary bugs should not get a five-level writeup. Unusual approaches must be cheaper to *disprove* than to build, and they must use surfaces the user is already allowed to use.

Voice stays light: at most one wry line, no mandatory closing catchphrase. Evidence does the work.
