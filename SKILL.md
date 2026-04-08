---
name: repo-graphify-router
description: Machine-first graphify enhancement skill. Use it to force low-token read order, target-first routing, and findings-first review behavior on any graphify corpus or graphify-style navigation target.
---

# Repo Graphify Router

GRAPHIFY:
- official_first
- Codex: `$graphify <path>`
- Claude: `/graphify <path>`
- do_not_use_shell_graphify_path_as_canonical_flow

USE_WHEN:
- corpus_has_graphify_out
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
- detect corpus_docs_if_present

MODE:
- review
- analysis
- regression
- bug
- config
- integration

REVIEW:
- read_first: `graphify-out/AI_REVIEW_ROUTER.md`
- then: review_context_docs_if_present
- then: `graphify-out/AI_ROUTER.md`
- then: `graphify-out/AI_INDEX.md`
- then: recent_change_context_if_present
- then: changed_or_target_artifacts
- then: matching_external_docs_if_needed
- avoid: broad_search_before_target_selected
- output: findings_first_with_file_line_refs

ANALYSIS:
- read_first: `graphify-out/AI_ROUTER.md`
- then: `graphify-out/AI_INDEX.md`
- then: `graphify-out/wiki/index.md`
- use_report_only_for: graph_structure_communities_god_nodes_surprising_edges
- then: raw_artifacts_in_selected_target
- avoid: broad_search_before_target_selected
- output: route_followed_then_answer

FALLBACK:
- if_missing_AI_router_files:
- read_first: top_level_corpus_docs_if_present
- then: architecture_or_design_docs_if_present
- then: recent_change_docs_if_present
- then: agent_or_workspace_docs_if_present
- then: root_or_anchor_artifacts
- then: selected_target
- do_not_broad_search_before_target_selected

TARGET_SELECTION:
- choose_one_target_before_broad_search
- prefer_router_selected_target
- prefer_root_or_anchor_artifacts_if_no_router
- prefer_matching_docs_for_external_integrations

COMMON_PATTERNS:
- root_or_anchor: root_index_outline_or_anchor_artifacts
- primary_logic: primary_content_or_behavior_artifacts
- interface_surface: external_surface_or_interaction_artifacts
- state: state_storage_or_memory_artifacts
- integration: external_system_patterns_plus_matching_docs
- policy: constraints_rules_or_validation_artifacts

NOISE:
- ignore_generic_god_nodes
- ignore_graph_popularity_as_primary_signal
- prefer_router_files_over_graph_report
- if_report_is_noisy_route_by_target_intent
- exclude_dependency_trees_generated_outputs_caches_vendor_by_default

OPTIONAL_REVIEW_CHECKS:
- schema_shape_mismatch
- artifact_boundary_mismatch
- retry_or_idempotency_bug
- duplicate_execution
- reconnection_leak
- stale_state_or_consistency_issue
- missing_cross_surface_wiring
- hidden_behavior_change_from_recent_refactor

MULTIPROJECT:
- choose_one_target_scope_first
- do_not_mix_target_scopes_in_initial_read

DERIVE_TARGET_SPECIFIC:
- fork_this_skill_when_target_has_hard_read_order
- fork_this_skill_when_target_has_domain_or_protocol_gotchas
- fork_this_skill_when_target_has_multi_scope_rules
- keep_generic_sections: GRAPHIFY PRECHECK MODE FALLBACK NOISE HANDOFF MAINTENANCE
- override_target_sections: ROUTING REVIEW_CHECKS DOMAIN_CONSTRAINTS
- keep_live_target_state_in_target_local_files: `graphify-out/AI_REVIEW_ROUTER.md` `graphify-out/AI_ROUTER.md` `graphify-out/AI_INDEX.md`
- do_not_store_target_specific_state_in_global_memory

HANDOFF:
- include_mode
- include_read_order
- include_selected_target
- include_router_or_fallback_path_used

MAINTENANCE:
- after_code_changes_prefer_official_graphify_again
- if_target_documents_fast_local_rebuild_that_is_allowed
