# Alternate surfaces

Use this as a checklist of *supported, authorized* places that might already expose the outcome. It is not a prompt to scrape, intercept, or escalate.

For each candidate, ask:

1. Is this a surface the user is already allowed to use?
2. Is there a more stable contract than the one I'm looking at (docs, CLI, schema, official export)?
3. What would make this evidence wrong, and how will the proof test catch that?

Prefer surfaces near the top of each group. Stop at the first credible option; do not collect a theatrical list.

## Same capability, different official interface

| If the blocked route is... | Look for... | Usually more brittle |
| --- | --- | --- |
| A GUI with no automation API | Vendor CLI, documented `--export` / `--import`, official "Save as" formats, accessibility APIs that invoke the same user-visible command | Click-scripting against labels and coordinates |
| A vendor SDK | The documented HTTP/REST, GraphQL, or gRPC API the SDK wraps | Undocumented SDK-private methods |
| A mobile-only flow | The provider's documented web, PWA, or desktop equivalent, if it offers the same account-scoped action | Reverse-engineering a private mobile API |
| Live on-screen data | Official export, download, or report the product already provides | Scraping the DOM or a screenshot when an export exists |

## Same answer, different authorized evidence

| If you need to know... | Authorized evidence | Validate with |
| --- | --- | --- |
| Whether a local job finished | Output file appears and size is stable; local process exit code; official app notification | Parse the file; do not trust existence or a fixed sleep |
| A UI locale or language | Stable system labels on a supported web or desktop UI the user can open; account language setting if exposed | Several labels together, not one word |
| Whether a feature is present in a build you are allowed to inspect | Documented flags, official release notes, published API errors, feature-detecting public responses | Timing, bundle size, or CSS class names alone |
| What a system already stored for this user | Official export, settings page, or API scoped to their account | Local caches or temp files unless the vendor documents them as the supported store |

## Same work, different time or place

| If the blocked step is... | Consider inverting to... | Watch out for |
| --- | --- | --- |
| Tight polling | Documented webhooks, SSE, or push notifications, with poll-as-reconciliation | Invented event streams; unverified callbacks |
| A synchronous UI action | A documented batch, queue, or scheduled job | Silent queue failure without a receipt |
| Client-side work the environment forbids | A server or edge path the project already runs and the user can deploy | Moving work to a system you are not authorized to change |

## Surfaces to treat as last-resort or out of scope

- **UI structure** (DOM selectors, pixel positions, copy-only labels): last resort, always paired with a fallback, and verified on each supported target.
- **Screenshots plus OCR:** only when no textual or accessibility surface exists; expect localization and layout false positives.
- **Undocumented endpoints, private caches, or internal file formats:** out of scope unless the vendor documents them for this use, or they are files the user's own software created and owns.
- **Traffic interception, process-memory inspection, credential or session reuse, and exploit development:** out of scope. If access is the lock, reject and request legitimate access.

When listing surfaces in a response, name only those that survived these filters, and say whether each is verified (docs, a flag, a file you saw) or inferred.
