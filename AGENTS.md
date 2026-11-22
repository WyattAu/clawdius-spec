# AGENTS.md — instructions for AI coding agents

Project: **clawdius-spec** — the shared Lean proof package for the
clawdius estate. Extracted from clawdius `.specs/02_architecture/proofs/`.

## Rigor rules (non-negotiable)

1. **Zero sorries.** The CI gate greps the build log for
   `declaration uses 'sorry'`. Never introduce `sorry`, `admit`, or
   unverified placeholders. If a proof is hard, say so in a YP file —
   do not stub it.
2. **Core-only imports.** `Init` and `Std` only. Taking a mathlib
   dependency is a package-level decision (README + PR discussion), not
   a per-file convenience.
3. **Import-free modules.** Files must not import each other. Cross-file
   concepts are restated and tied by enumeration theorems — duplication
   plus a tie theorem beats hidden coupling (house doctrine, inherited
   from clawdius AGENTS.md).
4. **Roots gate.** New file = new `roots` entry in `lakefile.toml`.
   A file the roots do not list is not built, not gated, not proven.
5. **Toolchain bumps are PRs.** Never edit `lean-toolchain` directly on
   main; the build must be re-verified green before merge.
6. **Axioms are inventory items.** Every new `axiom` needs a README
   postulate-inventory line and a justification in a YP-style comment.
   Prove it instead when the model allows.

## Release protocol

- Versions are `v`-prefixed git tags (`v0.1.0`); tag only on green main.
- Consumers pin the tag. A tag is a claim pin: theorems at that rev are
  verified and frozen.
