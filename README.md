# agotrain

AuraGo's tool-calling training playground: synthetic datasets, reproducible
validators, Unsloth fine-tuning, and Needle3 routing experiments.

The application lives in [AuraGo](https://github.com/antibyte/AuraGo). This repo
starts from the reviewed snapshot in [origin.json](origin.json); earlier history
remains there. [The training guide](training/README.md) explains data formats,
model preflight and evaluation. [Needle3](training/needle3/README.md) has its own
research and execution gates.

## Checkout and validate

Keep the repositories side by side. Run Python commands below from **agotrain**:

```sh
git clone https://github.com/antibyte/agotrain.git
git clone https://github.com/antibyte/AuraGo.git
cd agotrain
# Use a dedicated AuraGo checkout; do not change a working tree containing edits.
git -C ../AuraGo checkout --detach "$(cat aurago-revision)"
python -m pip install jsonschema==4.26.0 uv==0.11.15
uv lock --check --project training
uv lock --check --project training/needle3
python training/validate_dataset.py --all
python -m unittest training.test_training_pipeline training.needle3.test_pipeline training.needle3.test_diagnostic training.needle3.test_local_pilot training.needle3.test_category
```

On PowerShell, read the revision with `Get-Content aurago-revision`. To verify
the pack against the actual Go runtime, run from **AuraGo**:

```sh
go run ./disposable/export_tools --out ../agotrain/training --check
go run ./disposable/export_tools --source ../agotrain/training --manual-router-out ../agotrain/training/needle3/catalog.json --check
```

Set `AURAGO_TRAINING_DIR` to the absolute `agotrain/training` path before running
`go test ./disposable/export_tools ./disposable/import_training_traces ./internal/layerling`.
CI checks out the exact `aurago-revision` and supplies this path automatically.
Normal validation installs no CUDA packages and starts no paid jobs.

## Refresh after an AuraGo tool change

1. Review the new AuraGo commit and set `aurago-revision` to its full SHA.
2. Reconcile `training/tool_tiers.json` and `training/operation_contracts.json`
   with the new strict schema, preserving curated fixtures and semantic arguments.
3. Run the two Go commands above without `--check`, then run all validation gates.
   Refresh Needle3 seed/schedule manifests when their source identities change;
   seeds remain unaccepted and cost ledgers never reset.
4. Commit here, then update AuraGo's `.github/agotrain-revision` to that commit.
   Its integration CI checks the pinned pack against the candidate AuraGo runtime.

This two-step handoff needs no submodule, automatic cross-repository writes,
personal token, or moving-branch dependency. The pins need not refer to commits
created simultaneously: exporter-equivalent commits can share the same pack.

## Local-only material and security

Only tracked source and synthetic data were extracted. Ignored models, private
ledgers, reports, trace staging and virtual environments remain in their original
local locations. Recreate environments with `uv sync --project training --frozen`.
Migration does not authorize paid generation, training, or RunPod resources.

Known dependency advisories follow the training environment into this repository;
they are documented in [SECURITY.md](SECURITY.md). Separating repositories does
not fix the vulnerable dependencies. Existing AuraGo Git history is unchanged.
