# clawdius-spec

Formal proofs of the clawdius architecture in Lean 4, packaged for reuse.
Extracted from [`WyattAu/clawdius`](https://github.com/WyattAu/clawdius)
`.specs/02_architecture/proofs/` at commit `90d8b37`, byte-identical,
re-verified under `leanprover/lean4:4.28.0`.

## Rigor contract

- **Zero sorries.** CI fails on any `declaration uses 'sorry'` warning.
- **Postulate inventory (explicit, by design).** 3 axioms, all modeled
  environment assumptions, never hidden: `ehash` / `ehash_inj`
  (`audit.lean`, abstract injective event hash),
  `postulate_signature_unforgeable` (`capability.lean`, the standard
  crypto assumption). Theorems relying on them are conditional on them —
  this is the honest floor, stated up front.
- **`native_decide` count: 2** (`security_policy.lean`, concrete literal
  decisions). Tolerated; `decide` replacement tracked as a follow-up.
- **No `unsafe`, no `@[implemented_by]`.**
- **Core-only.** Every module imports only `Init` / `Std` — no mathlib,
  so builds are fast and toolchain-bump risk stays low.
- **Import-free modules.** Proof files never import each other and build
  in independent environments (house doctrine from clawdius: restatement
  + enumeration tie-theorems beats hidden coupling). Duplicate type
  names across modules are therefore intentional — modules are never
  co-elaborated. Reuse happens at the *package* level only.
- **Roots gate.** Every module is an explicit `roots` entry in
  `lakefile.toml`. A proof file not listed there is not built by
  `lake build` — and is therefore not proven. New file = new roots entry
  + `defaultTargets` stays `["ClawdiusSpec"]` (same rule as clawdius's
  "new file = new lib entry").

## Modules

| Domain | File |
|---|---|
| Agent loop | `agent_loop.lean` |
| Audit trail | `audit.lean` |
| Capability model | `capability.lean` |
| Concurrent execution | `concurrent_execution.lean` |
| Container isolation | `container.lean` |
| State machine | `fsm.lean` |
| Host invariants | `host.lean` |
| Invariant ledger | `invariant_ledger.lean` |
| Proof cache | `proof_cache.lean` |
| Proof-carrying edits | `proof_carrying_edits.lean` |
| Gateway routing | `proof_gateway_routing.lean` |
| Ring buffer (extended) | `proof_ring_buffer_extended.lean` |
| RPC protocol | `proof_rpc.lean` |
| Sandbox (extended) | `proof_sandbox_extended.lean` |
| Symbol index | `proof_symbol_index.lean` |
| Repo map scoring | `repo_map_scoring.lean` |
| Sandbox policy | `sandbox_policy.lean` |
| Sanitizer soundness | `sanitizer_soundness.lean` |
| Security policy | `security_policy.lean` |
| Tier isolation | `tier_isolation.lean` |
| Token budget scan | `token_budget_scan.lean` |
| VFS safety | `vfs_safety.lean` |
| Scratch (fold semantics) | `TestFold.lean` |

## Consuming

Once indexed by [Reservoir](https://reservoir.lean-lang.org):

```lean
require "WyattAu" / "clawdius-spec"
```

Or pinned to a release tag from git (works today):

```lean
require "clawdius-spec" from git
  "https://github.com/WyattAu/clawdius-spec" @ "v0.1.0"
```

## Toolchain policy

Pinned in `lean-toolchain`. Bumps are PRs with a full green build + sorry
gate; proofs are version-fragile and never bump casually.
