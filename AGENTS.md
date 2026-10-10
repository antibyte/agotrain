# agotrain contributor contract

## Purpose and ownership

- This repository owns AuraGo's synthetic tool-calling datasets, curated training
  contracts, validators, Python environments, and Needle3 research.
- Keep the `training/` layout so imports, dataset hashes, and experiment paths
  remain stable. `training/needle3/AGENTS.md` owns Needle3's additional contracts.
- AuraGo owns the runtime tool schemas, manuals, Go exporter, trace scrubber and
  importer. Do not copy its internal Go packages here. `aurago-revision` pins the
  exact source commit used by CI. `origin.json` records extraction provenance.

## Work guidance

- Preserve curated operation fixtures and tiers when schemas change. Update the
  source pin deliberately, regenerate with AuraGo, then validate both repositories.
- `AURAGO_SOURCE` selects the AuraGo checkout for live catalog search; its default
  is the sibling `../AuraGo`. Pass `--source` and `--out` explicitly when exporting.
- Generated data are synthetic. Never commit user traces, credentials, environments,
  model weights, raw generations, cost ledgers, or reports. Trace staging requires
  explicit input and human review; the importer stays in AuraGo.
- Existing ignored experiments and spend ledgers were not migrated or reset. Find
  the existing private ledger before any paid continuation; a missing ledger is
  not authorization to start a new budget. No RunPod/GPU work is authorized by migration.
- Keep dependency vulnerabilities visible. Do not force incompatible Unsloth,
  Datasets or fsspec overrides. See `SECURITY.md`.
- Pin workflow actions to full commit SHAs, default to `contents: read`, and keep
  validation CPU-only. No credentials are needed for checkout or offline tests.
- Preserve unrelated work, stage explicit files, inspect the complete staged diff
  and scan for secrets before committing. Documentation and comments use English.
- For sub-agents use Luna at Extra High. Suggest a better solution when one exists.

## Verification

- `.github/workflows/training.yml` is the canonical gate: pinned AuraGo exporter
  and importer tests, dataset and router regeneration checks, both uv lock checks,
  independent dataset/privacy validation, and offline Python tests.
- A clean AuraGo checkout must build and test without this repository. Its optional
  exporter integration tests require `AURAGO_TRAINING_DIR`; both CI workflows set it.
- Source revision, artifact hashes and held-out split identities must survive a
  refresh. Local CPU checks do not establish GPU or production training acceptance.

## Child DOX index

- `training/needle3/AGENTS.md` — category routing, manual selection, budgets,
  reproducibility, and separately authorized GPU lifecycle.
