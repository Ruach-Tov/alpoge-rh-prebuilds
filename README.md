# Alpöge RH Pre-builds

Pre-built `.olean` files for [Levent Alpöge's claimed proof of the Riemann Hypothesis](https://alpo.ge/remark.pdf), independently compiled by the [Ruach Tov Collective](https://ruachtov.ai).

## What is this?

Alpöge released a Lean 4 formalization of a claimed proof of the Riemann Hypothesis and Generalized Riemann Hypothesis on November 20, 2026. The proof comprises 175,187 Lean source files and 17.4 million lines of code.

This repository provides pre-built `.olean` files from an independent full-verification build (kernel-checked, default trust level). These artifacts can accelerate other builders by serving as cached prerequisites.

## Build provenance

| Field | Value |
|-------|-------|
| Lean | 4.33.1 (commit `819816b2e0a3bf405af45ae5c7af2491d8f5bee6`) |
| Mathlib | `0df444a360eaa60ab8c11dca51a86af692955474` |
| Platform | x86_64-unknown-linux-gnu (NixOS, 24 cores, 251 GB RAM) |
| Build type | Full verification (kernel-checked, default trust level) |
| Builder | Ruach Tov Collective enclave |
| Source | [alpo.ge/lean.tar.gz.part01–14](https://alpo.ge/remark.pdf) |

## Hash verification

Our `.olean` SHA-256 hashes can be cross-verified against the independent community replay project:

**[rohanarun/alpoge-rh-replay](https://github.com/rohanarun/alpoge-rh-replay)** — Independent replay of Alpöge's RH release

- Their [volunteer evidence](https://github.com/rohanarun/alpoge-rh-replay/tree/main/evidence/volunteers) contains per-module SHA-256 hashes in JSON receipts
- Their [completed module ledger](https://github.com/rohanarun/alpoge-rh-replay/tree/main/completed) tracks which modules have verified evidence
- Their [tracker](https://github.com/rohanarun/alpoge-rh-replay/tree/main/tracker) maps module IDs (M######) to module names

To verify any of our `.olean` files against their receipts:

```bash
# Our hash:
sha256sum path/to/Module.olean

# Their hash (in evidence/volunteers/{MODULE_ID}.json → objects → {MODULE_ID}):
curl -s https://raw.githubusercontent.com/rohanarun/alpoge-rh-replay/main/evidence/volunteers/M000001.json \
  | jq '.objects.M000001'
```

We have confirmed bit-identical hashes on cross-checked modules (same toolchain, deterministic build).

## Organization

Artifacts are organized by Lake build wave — each wave corresponds to a set of modules whose dependencies were satisfied simultaneously during `lake build`.

```
waves/
  wave-0001-den1103.zip     # First wave (BuildAid, 5 modules)
  wave-0002-den13429.zip    # Second wave (first project modules)
  ...
  wave-index.json            # Machine-readable index of all waves
```

Each ZIP contains:
- `.olean` files (the pre-built artifacts)
- `receipt.json` (SHA-256 per module, build provenance)

## Usage

Download wave ZIPs and extract `.olean` files into your `.lake/build/lib/lean/` directory to skip recompilation of those modules. Lake will use cached `.olean` files and only rebuild modules that are missing or stale.

## Credits

- **Levent Alpöge** — the claimed proof and Lean formalization
- **[rohanarun/alpoge-rh-replay](https://github.com/rohanarun/alpoge-rh-replay)** — the community verification project providing independent hash references
- **Ruach Tov Collective** — this independent build

## Disclaimer

This repository provides build artifacts only. It does not constitute independent verification of the claimed proof. For independent verification status, see the [replay project](https://github.com/rohanarun/alpoge-rh-replay).

**We publish only modules whose hashes already match community-verified receipts.** The integrity of hash-matching depends on the hash-maker building independently, without access to our artifacts. Publishing unverified oleans would risk contaminating the blinding — a volunteer could submit our olean's hash as an "independent" build. We wait for the community to produce their hashes first, then publish only the modules where our independently-built olean matches their independently-verified hash.
