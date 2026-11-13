# Anti-patterns

These are the usual ways this skill goes wrong. If a draft response matches one, revise or reject.

## Theatrical guesses

Listing five clever ideas that lack a mechanism is not analysis. Produce one strong, testable approach, or say there isn't one.

## Mechanism worship

Treating "call this API" or "click this GUI" as the outcome. Restate the outcome without the tool before you invent a route.

## Skipping level 1

Jumping to an exotic inversion when retries, the documented batch endpoint, or a bad input would fix the current route.

## Fabricated capability

Claiming an API, flag, file path, or browser behavior you have not verified. If it is not in docs or in the workspace, it is a hypothesis — and the proof test must be able to kill it.

## Proxy certainty

`sleep(30)`, "file exists," or a green smoke test that would also pass a truncated or empty result. Measure the actual outcome.

## Loosening the test

A failed proof test means the approach is wrong, or the test was measuring the wrong thing. It does not mean the success bar should get quieter.

## Brittle automation first

CSS selectors, pixel clicks, and copy-only UI labels as the primary path when a CLI, export, or documented API exists.

## Security as a side door

Session cookies instead of a token, "hidden" admin routes, MITM, memory dumps, or "just skip validation." Those are bypasses. Reject and offer a legitimate path (request access, reduce the outcome, use a supported export).

## Irreversible first try

A migration, destructive flag, or production write as the first experiment. Start with a fixture, a copy, or a dry run.

## Forced persona

Mandatory riddles, exit catchphrases, or voice that crowds out evidence. One wry line is enough; the structure and the test do the work.

## Using this skill when nothing is blocked

Ordinary bugs and straightforward features do not need a rabbit-hole writeup. Fix them in the usual way.
