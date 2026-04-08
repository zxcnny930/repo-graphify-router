---
name: repo-graphify-router
description: Machine-first graphify enhancement generator. Use it to generate AI_INDEX.md, AI_ROUTER.md, and AI_REVIEW_ROUTER.md for any graphify corpus, then use those generated files for low-token routing and review.
---

# Repo Graphify Router

ROLE:
- graphify_enhancement_generator
- generate_router_files_before_using_router_files

GRAPHIFY:
- official_first
- Codex: `$graphify <path>`
- Claude: `/graphify <path>`
- do_not_use_shell_graphify_path_as_canonical_flow

WHEN_TO_RUN:
- corpus_has_graphify_out
- router_files_missing
- router_files_stale
- user_wants_ai_navigation_layer
- user_wants_review_or_analysis_router_generated

INPUTS:
- `graphify-out/wiki/index.md` if present
- `graphify-out/GRAPH_REPORT.md` if present
- corpus_docs_if_present
- root_or_anchor_artifacts_if_needed

OUTPUTS:
- `graphify-out/AI_INDEX.md`
- `graphify-out/AI_ROUTER.md`
- `graphify-out/AI_REVIEW_ROUTER.md`

PROCESS:
- detect graphify artifacts
- infer corpus type
- select target scopes
- generate `AI_INDEX.md`
- generate `AI_ROUTER.md`
- generate `AI_REVIEW_ROUTER.md`
- after_generation_use_generated_files_as_primary_router

CORPUS_TYPE:
- code_heavy
- docs_heavy
- mixed
- multi_scope

SOURCE_PRIORITY:
- generated_router_files_if_already_good
- `graphify-out/wiki/index.md`
- `graphify-out/GRAPH_REPORT.md`
- top_level_corpus_docs_if_present
- architecture_or_design_docs_if_present
- recent_change_docs_if_present
- agent_or_workspace_docs_if_present
- root_or_anchor_artifacts

AI_INDEX_TEMPLATE:
- purpose: machine_first_navigation
- include: freshness_note_if_needed
- include: read_order
- include: primary_targets
- include: high_signal_questions_to_targets
- include: preferred_navigation_strategy
- exclude: long prose

AI_ROUTER_TEMPLATE:
- purpose: analysis_router
- include: read_first_rule
- include: target_selection_rules
- include: target_by_intent
- include: noise_rules
- include: graph_report_usage_rule
- exclude: corpus_specific_state_that_will_rot_fast

AI_REVIEW_ROUTER_TEMPLATE:
- purpose: review_router
- include: review_read_order
- include: review_targets_by_change_area
- include: required_cross_checks
- include: high_risk_patterns
- include: output_contract_findings_first
- exclude: broad_architecture_explanation

TARGET_SELECTION:
- choose_one_target_scope_before_broad_search
- prefer_root_or_anchor_artifacts_for_initial_partitioning
- prefer_high_signal_targets_from_wiki_or_report
- prefer_matching_docs_for_external_integrations
- do_not_mix_unrelated_target_scopes_in_initial_router

COMMON_TARGET_PATTERNS:
- root_or_anchor
- primary_logic
- interface_surface
- state_or_storage
- external_integration
- policy_or_constraints

NOISE:
- ignore_generic_god_nodes
- ignore_graph_popularity_as_primary_signal
- if_report_is_noisy_route_by_target_intent
- exclude_dependency_trees_generated_outputs_caches_vendor_by_default

USE_AFTER_GENERATION:
- review_mode:
- read_first: `graphify-out/AI_REVIEW_ROUTER.md`
- then: `graphify-out/AI_ROUTER.md`
- then: `graphify-out/AI_INDEX.md`
- analysis_mode:
- read_first: `graphify-out/AI_ROUTER.md`
- then: `graphify-out/AI_INDEX.md`
- then: `graphify-out/wiki/index.md` if present

DERIVE_TARGET_SPECIFIC:
- fork_this_skill_when_target_has_hard_read_order
- fork_this_skill_when_target_has_domain_or_protocol_gotchas
- fork_this_skill_when_target_has_multi_scope_rules
- keep_generic_sections: ROLE GRAPHIFY WHEN_TO_RUN INPUTS OUTPUTS PROCESS SOURCE_PRIORITY TARGET_SELECTION NOISE
- override_target_sections: TEMPLATES TARGET_PATTERNS REVIEW_RULES DOMAIN_CONSTRAINTS
- keep_live_target_state_in_target_local_files: `graphify-out/AI_REVIEW_ROUTER.md` `graphify-out/AI_ROUTER.md` `graphify-out/AI_INDEX.md`
- do_not_store_target_specific_state_in_global_memory

HANDOFF:
- include_corpus_type
- include_generated_files
- include_selected_target_scopes
- include_source_priority_used

MAINTENANCE:
- rerun_after_major_graphify_refresh
- rerun_after_target_structure_changes
- rerun_after_domain_constraints_change
