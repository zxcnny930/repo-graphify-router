---
name: repo-graphify-router
description: Machine-first graphify enhancement skill. Use it to force low-token read order, module-first routing, and findings-first review behavior on repositories that already use graphify or graphify-style navigation.
---

# Repo Graphify Router

GRAPHIFY:
- official_first
- Codex: `$graphify <path>`
- Claude: `/graphify <path>`
- do_not_use_shell_graphify_path_as_canonical_flow

USE_WHEN:
- repo_has_graphify_out
- task_is_review
- task_is_architecture
- task_is_regression
- task_mentions_AI_ROUTER_or_AI_REVIEW_ROUTER_or_AI_INDEX_or_GRAPH_REPORT_or_wiki

PRECHECK:
- detect `graphify-out/`
- detect `graphify-out/AI_REVIEW_ROUTER.md`
- detect `graphify-out/AI_ROUTER.md`
- detect `graphify-out/AI_INDEX.md`
- detect `graphify-out/wiki/index.md`
- detect `graphify-out/GRAPH_REPORT.md`
- detect `AGENTS.md`
- detect `ARCHITECTURE.md`
- detect `CHANGELOG.md`

MODE:
- review
- analysis
- regression
- bug
- config
- integration

REVIEW:
- read_first: `graphify-out/AI_REVIEW_ROUTER.md`
- then: `AGENTS.md`
- then: `graphify-out/AI_ROUTER.md`
- then: `graphify-out/AI_INDEX.md`
- then: `CHANGELOG.md`
- then: changed_module
- then: matching_external_docs_if_needed
- avoid: repo_wide_grep
- output: findings_first_with_file_line_refs

ANALYSIS:
- read_first: `graphify-out/AI_ROUTER.md`
- then: `graphify-out/AI_INDEX.md`
- then: `graphify-out/wiki/index.md`
- use_report_only_for: graph_structure_communities_god_nodes_surprising_edges
- then: raw_files_in_selected_module
- avoid: repo_wide_grep
- output: route_followed_then_answer

FALLBACK:
- if_missing_AI_router_files:
- read_first: `README.md`
- then: `ARCHITECTURE.md`
- then: `CHANGELOG.md`
- then: `CLAUDE.md`
- then: `AGENTS.md`
- then: entrypoint
- then: selected_module
- do_not_broad_search_before_module_selected

MODULE_ROUTING:
- startup: entrypoint config wiring
- execution: runtime modules under `internal/` `src/` `pkg/`
- api_ui: `api/` `server/` `web/` `frontend/` `ui/`
- persistence: `database/` `db/` `store/` `state/`
- integration: adapter_or_client_plus_matching_docs
- risk_policy_auth: `risk/` `policy/` `auth/` validation_modules

NOISE:
- ignore_generic_god_nodes
- ignore_graph_popularity_as_primary_signal
- prefer_router_files_over_graph_report
- if_report_is_tooling_heavy_route_by_module_intent
- exclude_dependency_trees_generated_outputs_caches_vendor_by_default

REVIEW_CHECKS:
- schema_shape_mismatch
- inbound_outbound_format_mismatch
- retry_or_idempotency_bug
- duplicate_execution
- reconnection_leak
- stale_state_or_race
- missing_backend_api_persistence_ui_wiring
- hidden_behavior_change_from_recent_refactor

MULTIPROJECT:
- choose_one_subproject_first
- do_not_mix_subprojects_in_initial_read

DERIVE_REPO_SPECIFIC:
- fork_this_skill_when_repo_has_hard_read_order
- fork_this_skill_when_repo_has_exchange_or_protocol_gotchas
- fork_this_skill_when_repo_has_multi_project_workspace_rules
- keep_generic_sections: GRAPHIFY PRECHECK MODE FALLBACK NOISE HANDOFF MAINTENANCE
- override_repo_sections: ROUTING REVIEW_CHECKS DOMAIN_CONSTRAINTS
- keep_live_repo_state_in_repo_local_files: `graphify-out/AI_REVIEW_ROUTER.md` `graphify-out/AI_ROUTER.md` `graphify-out/AI_INDEX.md`
- do_not_store_repo_specific_state_in_global_memory

HANDOFF:
- include_mode
- include_read_order
- include_selected_module
- include_router_or_fallback_path_used

MAINTENANCE:
- after_code_changes_prefer_official_graphify_again
- if_repo_documents_fast_local_rebuild_that_is_allowed
