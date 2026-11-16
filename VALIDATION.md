# Validation

Contributor commit: `f4a7f9b0b5c3e47d7372e81ff433512352d22238`. Before: `eba957a3d3c9daff621de36c6ff61896042f6b3d`. Pinned upstream: `382da96b5ce481cf0035e912488cbbf46a175a52`.

## Targeted regression

The actual static/live producer → production renderer matrix covers active/inactive provider, legacy/canonical provider spelling, codex/any and anthropic/bedrock vendor pairs, and absent/exact configured IDs: 32 cases. Before the fix, all 16 different-vendor cases fail and all 16 exact-ID controls pass. With the fix, **154 affected and neighboring picker tests pass**, including the 32 new cases and the earlier reverse/deeper/overflow/identity guards.

ESLint runtime guard, JS syntax, Ruff on edited tests/changed lines, staged whitespace and local SSH signature verification pass.

## Broad verification and the resource limit

The candidate was integrated with the pinned upstream and tested serially using the repository runner, Python 3.11 and isolated temporary state. The first run was deliberately interrupted after the 400,000-byte inline-image test spent several minutes in the installed Hermes Agent redactor (`agent/redact.py`, `_ENV_ASSIGN_LOWER_RE.sub`). The baseline reproduces a 30-second timeout at the same point. The interruption also exposed a teardown AttributeError in the upstream generic conftest; it is not a completed test result.

The remaining collected items ran in a second serial segment with a 30-second per-test timeout. The resumed selection map and JUnit records cover **all 28,955 collected items**, with no missing item. This is split coverage, not one uninterrupted default-settings full-suite pass.

Unique collected-item outcomes, prioritizing error/failure over duplicate subtest records:

| Outcome | Items |
| --- | ---: |
| Passed, including 2 XPASS | 28,432 |
| Skipped | 455 |
| XFAIL | 2 |
| Failed | 48 |
| Error | 18 |

The 15 failing files were then run on unmodified pinned upstream with the same interpreter, isolated state and 30-second bound: **48 failed, 609 passed, 5 skipped, 18 errors**. The **66 failure/error IDs exactly match** the candidate's combined IDs; there are no candidate-only or upstream-only IDs. The existing integration records already cover this cohort, so another candidate-only rerun was unnecessary after this exact comparison.

Observed baseline categories include installed Hermes Agent API/dependency mismatches (including missing requests/config_effective/memory helpers), macOS special-mode behavior, Git credential-origin labeling, shell/environment fixtures, session database behavior and the bounded redaction case. No real credentials or live provider calls are claimed. These results do not mean the broad suite is green.

## Contract boundary

Vendor-qualified configured identities no longer fall through to a normalizer that strips both vendors. Exact/provider-routed matches still deduplicate. The backend does not re-badge a different vendor. The bare-ID/multiple-vendor product decision `DECIDE-1103-WARM8741` remains unchanged and requires maintainer direction.

## AI assistance

OpenAI Codex configured for GPT-6.1 Sol (`gpt-6.1-sol`, high reasoning): review/reproduction, code/tests/docs, terminal verification and real Chrome component verification through the Codex extension. No parallel agents or concurrent test suites.
