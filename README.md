# Hermes #8741 — vendor-qualified fallback visibility

[Contributor follow-up](https://github.com/henricop/hermes-webui/commit/f4a7f9b0b5c3e47d7372e81ff433512352d22238) for the [16 November gate](https://github.com/nesquena/hermes-webui/pull/8741#issuecomment-6916029049).

Configured fallback: `custom:relay` / `codex/gpt-6.1-sol`. The relay catalog contains `any/gpt-6.1-sol` and `fixture-unrelated`, without the configured ID. The real static catalog producer leaves the different-vendor row unbadged. The production picker must retain one separate provider-owned Configured fallback.

| Viewport | Before | After |
| --- | --- | --- |
| Desktop, 1280×900 | [Before](before-desktop.jpg) | [After](after-desktop.jpg) |
| Narrow, 390×844 | [Before](before-mobile.jpg) | [After](after-mobile.jpg) |

Both widths: zero fallback choices before, one after, with no document horizontal overflow. Browser clicks use the production select-resolution functions and yield client state `{"model":"codex/gpt-6.1-sol","model_provider":"custom:relay"}`.

These are real Chrome DOM screenshots through the Codex extension, using production picker functions/CSS and a real static-catalog payload in an isolated component fixture. All 15 extracted production functions match the tested integration. Surrounding fixture copy and callback display are diagnostic. This is not backend persistence, live-provider inference, or full-application evidence. JPEG screenshot bytes are unchanged.

[Tests, baseline comparison, scope and limits](VALIDATION.md).
