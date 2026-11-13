# Calibration examples

Worked cases at each rabbit-hole level, plus one rejection. Use these to calibrate depth and tone, not as scripts to copy verbatim.

## Level 1 — Repair the route: rate-limited batch import

**The lock:** A documented REST import endpoint returns HTTP 429 after a few dozen items, so the migration looks impossible at the required volume.

**The hidden assumption:** The current call pattern (one item per request, no backoff, latest SDK defaults) is the only way to use this API.

**The side door:** Stay on the official import API. Read the provider's rate-limit and batch-import docs. Use the documented batch endpoint if one exists, honor `Retry-After`, and add exponential backoff with jitter. This is a repair, not a new architecture.

**Proof test:** Import a 50-item fixture. Success: every item acknowledged, no data loss, 429s recovered via backoff. Failure: repeated 429s with no `Retry-After` recovery, or a documented hard cap below the required volume.

**Risks:** SDK retries may stack with your own; some plans have daily caps that backoff cannot fix.

**Recommendation:** Proceed with the repair. If the proof test hits a documented hard cap, escalate to level 4 (queue and drain across windows) rather than inventing an unofficial endpoint.

## Level 2 — Change the interface: desktop export with no automation API

**The lock:** A desktop app has no public automation API, and the team assumed the GUI must be scripted click-by-click.

**The hidden assumption:** The GUI is the only supported way to produce the export.

**The side door:** Check for a vendor CLI, "Export" / "Save as" formats, a documented deep-link or `--export` flag, or an accessibility tree that exposes the same official Export action the user can already click. Prefer a CLI or export format over UI automation.

**Proof test:** Export a known small document through the discovered supported surface. Success: the output file exists, is complete, and parses against a fixture. Failure: no official CLI/export exists, or the accessibility action does not invoke the real Export command.

**Risks:** UI automation against labels is brittle across locales and versions. Do not scrape private app data stores or attach a debugger to bypass licensing.

**Recommendation:** Proceed if a supported CLI or export path exists. Keep accessibility-driven invocation of the official Export action as a fallback.

## Level 3 — Substitute the signal: know when an export finishes

**The lock:** The app exposes no completion callback, so the pipeline cannot tell when the file is ready.

**The hidden assumption:** Only a first-party completion API can signal "done."

**The side door:** Observe authorized, locally visible evidence: the destination file appears, its size is stable across two polls, and (if the process was launched locally) the export process exits with a known success code. Supported notifications can corroborate, but should not be the sole signal.

**Proof test:** Export a known small artifact. Record file-create time, the quiet period after the last size change, and the process exit code. Success: those events align with a file that parses completely. Failure: the file appears early and is still growing, or a truncated file passes a naive "file exists" check.

**Risks:** False positives from partial writes, temp files renamed at the end, and antivirus scanners touching the file. Reject blind `sleep(N)` — it imitates certainty without observing completion.

**Recommendation:** Proceed, with a stable-size check plus format validation, not existence alone.

## Level 4 — Invert the workflow: flaky status polling

**The lock:** A client polls a status endpoint that is rate-limited and occasionally returns stale state, blocking a "wait until done" UX.

**The hidden assumption:** The client must sit in a request loop until the server is done.

**The side door:** If the same platform documents webhooks, outbound events, or server-sent events for this job type, subscribe to the completion event and treat polling as a fallback reconciliation pass. Work moves to event-driven; the blocked poll is no longer on the critical path.

**Proof test:** Submit one job with a webhook (or SSE) receiver you control. Success: the completion event arrives, its job id matches, and a single later GET agrees. Failure: no documented push mechanism, or events arrive without a way to verify against GET.

**Risks:** Missed events, at-least-once delivery, and signature verification. Keep a slow, authorized poll as reconciliation, not as the primary wait.

**Recommendation:** Proceed when the push mechanism is documented. Otherwise fall back to level 1 (better polling) rather than inventing an event stream.

## Level 5 — Remove the assumption: "we need the full target environment"

**The lock:** Verifying generated output seems to require reproducing an expensive or inaccessible production environment.

**The hidden assumption:** Correctness can only be proved by running the entire target stack.

**The side door:** Validate the smallest externally observable contract: schema, parser, golden fixtures, and invariants the real consumer cares about. Add one end-to-end smoke test where authorized access already exists. The unusual idea is useful only if that contract would have caught the failures that matter.

**Proof test:** Take two fixtures — one known-good, one with a realistic defect from past incidents. Success: the contract suite passes the good fixture and fails the defective one. Failure: the suite is green on the defective fixture, which means the contract is too weak.

**Risks:** A thin contract creates false confidence. If you cannot name a real failure it would catch, this is not a side door.

**Recommendation:** Proceed as the default verification path; keep a limited smoke test where access exists.

## Reject — access is the actual lock

**The lock:** A third-party API refuses requests without a credential the user does not have. The request is to "find a side door."

**The hidden assumption:** "Side door" includes using session cookies, undocumented admin routes, or another account's traffic to skip authentication.

**The side door:** There isn't one inside this skill's scope. Supported options are: request a proper token or role, use a documented export the user already can access, or reduce the outcome to data they are authorized to see.

**Proof test:** Not applicable. Do not probe for auth bypasses.

**Risks:** Any bypass here is a security violation and would fail review and policy.

**Recommendation:** **Reject.** Explain the legitimate paths. Do not list exploitation steps "for completeness."

Hatter line, once: "The front door is locked because it is meant to be. Ask for a key."
