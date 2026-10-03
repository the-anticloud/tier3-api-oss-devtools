# L5 Narrow / L2 General Classification — api-oss-devtools
**Platform:** Anticloud | **PAX:** 27B | **IP:** USPTO pending 2026, Anticloud FZ LLE
**Domain:** Sovereign developer tooling: CLI, debugger, profiler for Anticloud development

## L5 Narrow
api-oss-devtools provides the CLI toolkit for Anticloud developers: project scaffolding, AIOSS chain inspection, PAX model debugging, performance profiling. No telemetry sent to external services — all profiling data is local.

## L2 General
L2 General: same CLI works across all 9 tiers. `anticloud inspect TIER_7/K_BRAINFLOW` and `anticloud inspect TIER_4/K_SGLANG` use identical subcommands.

## PAX Integration
PAX 27B is invoked for intelligent debugging assistance: given a stack trace or error log, PAX suggests the most likely root cause and remediation steps.

## AIOSS Audit Relevance
Every devtools event (command hash + target project + execution result + duration) is AIOSS-chained: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n).
Tamper-evident, offline-verifiable, zero cloud dependency.

## Regulatory / Compliance
NIST SSDF (secure development), ISO 25010 (maintainability)
