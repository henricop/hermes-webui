# Verification for Hermes WebUI PR #8741

Final contributor head: [eba957a3d3c9daff621de36c6ff61896042f6b3d](https://github.com/henricop/hermes-webui/commit/eba957a3d3c9daff621de36c6ff61896042f6b3d).
Frozen upstream: `651f752092c43523791f40a0c38aa56c8619917a`.

## Behavioral regression and affected tests

The pinned active-provider config uses `custom:relay`, primary `fixture-primary`, catalog `[fixture-primary, claude-sonnet-4]`, and fallback `anthropic/claude-sonnet-4` owned by `custom:relay`.

The real static/live producer → production renderer matrix covers active/inactive providers, legacy/canonical provider spelling and reverse/exact IDs (16 cases). With the old frontend on the pinned integration, exactly the four active-provider reverse cases fail; 49 focused cases pass. The corrected focused module passes all 53.

Final affected/neighboring picker tests: **156 passed**, including #8756 routing identities and #8733 chip labels. A separate regression rejects a typed `openrouter/vendor/model` as an exact replacement for the native configured route while accepting `@openrouter:vendor/model`.

## Full suite and baseline comparison

Command: `./scripts/test.sh tests/ -q --timeout=60`.

Native full-suite result on the final production code: **53 failed, 28,229 passed, 435 skipped, 2 xfailed, 2 xpassed, 18 errors, 427 subtests passed (30m16s)**.

After this full run, the only file changed was `tests/test_issue8733_configured_chip_role.py`: its isolated JavaScript driver needed one line to extract `_modelPickerOptionIdentity`. No application/backend file changed. Its three ReferenceErrors are fixed and all 156 affected/neighboring checks pass.

The same 20 files with full-run failures were rerun on the final contributor head and the frozen upstream, with the same interpreter and isolated state. Both native results: **50 failed, 1,132 passed, 37 skipped, 18 errors**. Their **68 failing/error test IDs match exactly**; there are no candidate-only or baseline-only failures in this comparison. This is a paired file-batch comparison, not a full-suite run on upstream.

The full run also exposed a different parametrization of the existing #8834 ownership probe than the paired batch. That exact `cached_profile_int_and_sidecar_ok-chat_start` case was run separately on upstream and also fails (the baseline rejects a non-string profile in `api/profiles.py` with `TypeError: expected string or bytes-like object, got 'int'`). Its occurrence varies with test ordering. It is not a configured-model regression.

The local installed Agent also lacks APIs expected by several upstream fixtures (for example `_declares_model_provider_kind`). These baseline findings are retained rather than reported as a green full suite.

## Final checks and visual scope

- ESLint runtime guard and `node --check static/ui.js`: pass.
- Ruff on the edited Python tests and committed diff gate: pass, with no new findings on changed lines.
- Relevant diff whitespace check vs upstream: pass.
- Contributor commits signed locally; remote verification recorded after publication.
- Screenshots use the real production picker/CSS in Chrome through the Codex browser extension, with a production static-catalog payload in an isolated component. At 1470×802 and 390×844: zero Configured fallback choices before, one after, no horizontal overflow. Selecting the fallback resolves the production client state to `anthropic/claude-sonnet-4` / `custom:relay`.
- This is component/client selection evidence, not a full-application, backend-persistence or live-provider claim.

## Matching baseline failures/errors

```text
tests/test_5774b_atomic_config_writes.py::test_atomic_write_preserves_existing_permissions
tests/test_delegation_cross_profile_replay.py::test_sweep_never_writes_another_profiles_ledger
tests/test_delegation_cross_profile_replay.py::test_webui_origin_row_in_a_named_ledger_is_replayed_and_delivered_once
tests/test_issue7295_cron_readers_no_global_lock.py::test_model_pin_resolves_config_against_the_requesting_profile[None]
tests/test_issue7295_cron_readers_no_global_lock.py::test_model_pin_resolves_config_against_the_requesting_profile[scheduled_fire_7295]
tests/test_issue7295_cron_readers_no_global_lock.py::test_snapshot_agent_inference_edits_resolve_config_against_the_requesting_profile[base_url-None]
tests/test_issue7295_cron_readers_no_global_lock.py::test_snapshot_agent_inference_edits_resolve_config_against_the_requesting_profile[base_url-scheduled_fire_7295]
tests/test_issue7295_cron_readers_no_global_lock.py::test_snapshot_agent_inference_edits_resolve_config_against_the_requesting_profile[model_cleared-None]
tests/test_issue7295_cron_readers_no_global_lock.py::test_snapshot_agent_inference_edits_resolve_config_against_the_requesting_profile[model_cleared-scheduled_fire_7295]
tests/test_issue7295_cron_readers_no_global_lock.py::test_snapshot_agent_inference_edits_resolve_config_against_the_requesting_profile[no_agent-None]
tests/test_issue7295_cron_readers_no_global_lock.py::test_snapshot_agent_inference_edits_resolve_config_against_the_requesting_profile[no_agent-scheduled_fire_7295]
tests/test_issue7295_cron_readers_no_global_lock.py::test_snapshot_agent_inference_edits_resolve_config_against_the_requesting_profile[provider-None]
tests/test_issue7295_cron_readers_no_global_lock.py::test_snapshot_agent_inference_edits_resolve_config_against_the_requesting_profile[provider-scheduled_fire_7295]
tests/test_issue7404_models_discovered_not_allowlist.py::test_models_discovered_skips_config_allowlist_and_uses_live_catalog
tests/test_issue7949_share_inline_image_data_uri.py::test_large_valid_png_does_not_crash_and_is_placeholdered[400000]
tests/test_issue8113_venv_rebuild_keeps_previous.py::test_a_completed_rebuild_replaces_the_venv_and_removes_the_old_copies
tests/test_issue8113_venv_rebuild_keeps_previous.py::test_a_rebuild_without_a_named_interpreter_uses_uvs_default
tests/test_issue8113_venv_rebuild_keeps_previous.py::test_an_interrupted_rebuild_is_rolled_back_on_the_next_boot
tests/test_issue8517_all_r1_findings.py::test_chat_sync_ui_switch_during_the_turn_cannot_redirect_builtin_memory
tests/test_issue8517_all_r1_findings.py::test_fail_closed_construction_never_touches_another_profiles_builtin_memory[always]
tests/test_issue8517_all_r1_findings.py::test_fail_closed_construction_never_touches_another_profiles_builtin_memory[current]
tests/test_issue8517_all_r1_findings.py::test_memoryless_helper_never_loads_the_ambient_builtin_memory[always]
tests/test_issue8517_all_r1_findings.py::test_memoryless_helper_never_loads_the_ambient_builtin_memory[current]
tests/test_issue8517_all_r1_findings.py::test_mismatch_rebuild_drops_builtin_memory_too[kwargs_only]
tests/test_issue8517_all_r1_findings.py::test_mismatch_rebuild_drops_builtin_memory_too[names_skip_memory]
tests/test_issue8517_all_r1_findings.py::test_scoped_construction_keeps_builtin_memory
tests/test_issue8517_all_r2_findings.py::test_cached_memory_agent_is_not_reused_for_a_turn_without_a_verified_scope
tests/test_issue8517_all_r2_findings.py::test_healthy_turns_keep_and_reuse_their_memory_agent
tests/test_issue8517_all_r2_findings.py::test_streaming_turn_whose_override_setter_fails_runs_without_memory
tests/test_issue8524_docker_agent_uv_overrides.py::test_an_unwritable_tmpdir_falls_back_to_the_stage_parent
tests/test_issue8524_docker_agent_uv_overrides.py::test_the_agents_overrides_are_passed_to_uv
tests/test_issue8524_docker_agent_uv_overrides.py::test_the_overrides_file_is_removed_afterwards
tests/test_issue8816_context_lookup_native_custom.py::test_the_builtin_endpoint_never_receives_the_custom_key[azure]
tests/test_issue8816_context_lookup_native_custom.py::test_the_builtin_endpoint_never_receives_the_custom_key[deepseek-chat]
tests/test_issue8816_context_lookup_native_custom.py::test_the_builtin_endpoint_never_receives_the_custom_key[openai]
tests/test_issue8816_context_lookup_native_custom.py::test_the_builtin_endpoint_never_receives_the_custom_key[or]
tests/test_issue8822_context_lookup_key_host.py::test_a_caller_key_for_a_fixed_host_id_is_that_providers_own[anthropic]
tests/test_issue8822_context_lookup_key_host.py::test_a_caller_key_for_a_fixed_host_id_is_that_providers_own[copilot-acp]
tests/test_issue8822_context_lookup_key_host.py::test_a_caller_key_for_a_fixed_host_id_is_that_providers_own[copilot]
tests/test_issue8822_context_lookup_key_host.py::test_a_caller_key_for_a_fixed_host_id_is_that_providers_own[novita]
tests/test_issue8822_context_lookup_key_host.py::test_a_caller_key_for_a_fixed_host_id_is_that_providers_own[openai-codex]
tests/test_issue8822_context_lookup_key_host.py::test_a_legacy_codex_entry_s_kept_key_reaches_only_its_own_url[jwt]
tests/test_issue8822_context_lookup_key_host.py::test_a_legacy_codex_entry_s_kept_key_reaches_only_its_own_url[synthetic]
tests/test_issue8822_context_lookup_key_host.py::test_copilot_live_models_with_a_url_is_in_the_table[jwt]
tests/test_issue8822_context_lookup_key_host.py::test_copilot_live_models_with_a_url_is_in_the_table[synthetic]
tests/test_issue8822_context_lookup_key_host.py::test_the_codex_detector_matches_the_installed_agent
tests/test_issue8822_context_lookup_key_host.py::test_the_copilot_detector_matches_the_installed_agent
tests/test_issue8822_context_lookup_key_host.py::test_the_early_return_gate_matches_the_installed_agent[https://api.openai.com/v1]
tests/test_issue8822_context_lookup_key_host.py::test_the_early_return_gate_matches_the_installed_agent[https://c.caller-url.test/v1]
tests/test_issue8822_context_lookup_key_host.py::test_the_fixed_host_table_covers_the_installed_agent[jwt]
tests/test_issue8822_context_lookup_key_host.py::test_the_fixed_host_table_covers_the_installed_agent[synthetic]
tests/test_issue8822_context_lookup_key_host.py::test_the_recorder_sees_the_agents_requests
tests/test_issue8834_refused_load_no_write.py::test_malformed_ownership_probe_never_500s_or_writes[profile_empty_string-chat_start]
tests/test_model_alias_routes.py::test_worker_url_alias_foreign_host_never_receives_provider_credential
tests/test_model_alias_routes.py::test_worker_url_alias_without_credential_uses_host_gated_key
tests/test_profile_switch_models_disk_cache.py::test_manifest_kind_parse_matches_agent[# kind: model-provider\n]
tests/test_profile_switch_models_disk_cache.py::test_manifest_kind_parse_matches_agent[kind: 'model-provider'\n]
tests/test_profile_switch_models_disk_cache.py::test_manifest_kind_parse_matches_agent[kind: [unclosed\nkind: model-provider\n]
tests/test_profile_switch_models_disk_cache.py::test_manifest_kind_parse_matches_agent[kind: model-provider # valid YAML comment\n]
tests/test_profile_switch_models_disk_cache.py::test_manifest_kind_parse_matches_agent[kind: model-provider\n]
tests/test_profile_switch_models_disk_cache.py::test_manifest_kind_parse_matches_agent[meta:\n  kind: model-provider\n]
tests/test_profile_switch_models_disk_cache.py::test_manifest_kind_parse_matches_agent[name: x\nkind: tool\n]
tests/test_update_git_security.py::test_authenticated_fetch_never_runs_host_credential_helpers[diagnostic-emulated-host-with-sentinel-helpers]
tests/test_update_git_security.py::test_authenticated_fetch_never_runs_host_credential_helpers[updates-emulated-host-with-sentinel-helpers]
tests/test_update_git_security.py::test_authenticated_fetch_never_runs_host_credential_helpers[workspace-emulated-host-with-sentinel-helpers]
tests/test_webui_state_db_session_cwd.py::test_real_db_trailing_space_workspace_is_recorded_verbatim
tests/test_webui_state_db_session_cwd.py::test_real_session_db_row_gets_workspace
tests/test_workspace_git.py::test_git_fetch_never_runs_host_credential_helpers[emulated-host-with-sentinel-helpers]
```

AI assistance: OpenAI GPT-6.1 Sol (`gpt-6.1-sol`, high reasoning) via Codex.
