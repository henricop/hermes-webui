# PR #8741 — active-provider configured fallback

This evidence is for the existing contribution at https://github.com/nesquena/hermes-webui/pull/8741.

The fixture uses the real static Python catalog builder, the production `renderModelDropdown()` and its identity/deduplication helpers, and production CSS in a real Chrome DOM through the Codex browser extension. It is an isolated component, not the full application. Authentication and upstream model calls are not exercised.

Pinned upstream: `651f752092c43523791f40a0c38aa56c8619917a`.
Prior contributor head: `79a83b3ca910e735825cd531a56b9d00626461ab`.

The controlled configuration is the one in the 14 November gate: active `custom:relay`, primary `fixture-primary`, catalog `[fixture-primary, claude-sonnet-4]`, configured fallback `anthropic/claude-sonnet-4` under `custom:relay`.

| Check | Before | After |
| --- | --- | --- |
| Configured fallback choices | 0 | 1 |
| Bare catalog model stays selectable | yes | yes |
| 390 px document scroll width | 390 px | 390 px |
| Selection through production client helper | unavailable | `model=anthropic/claude-sonnet-4`, `model_provider=custom:relay` |

The last row is client selection state, not a backend persistence or provider-inference claim. Both 1470×802 and 390×844 viewports were captured. CSS and labels are rendered in the component shell; icon/translation application bootstrapping is outside this fixture.

- `before-desktop.png` / `after-desktop.png`
- `before-mobile.png` / `after-mobile.png`

Generated with OpenAI Codex (GPT-6; exact served model ID is not exposed in the session).
