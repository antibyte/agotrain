# Training dependency security

This optional research environment is separate from AuraGo's application runtime.
Do not process untrusted dataset folder metadata or fsspec ReferenceFileSystem
templates in it.

The compatible locked stack includes Unsloth/Unsloth Zoo 2026.10.3,
Datasets 4.8.5 and fsspec 2026.2.0. These two advisories remain unresolved:

- [GHSA-379c-qx7v-6h59](https://github.com/advisories/GHSA-379c-qx7v-6h59):
  Datasets folder-builder path traversal. Fixed in 5.0.1; both Unsloth packages
  still require Datasets <5.
- [GHSA-27vj-qcqg-25rc](https://github.com/advisories/GHSA-27vj-qcqg-25rc):
  fsspec template injection/RCE. Fixed in 2026.6.0; Datasets 4.8.5 requires
  fsspec <=2026.2.0.

The trainer uses `Dataset.from_list` on reviewed JSONL. This does not make other
uses of those packages safe. Keep Dependabot alerts open and revisit the complete
stack when compatible fixes become available; do not override resolver constraints.
Torch 2.14.1, Transformers 5.17.0 and setuptools 84.0.0 already exceed the affected
ranges of the four older alerts observed during extraction on 2026-10-10.

CPU validation and successful dependency resolution do not qualify CUDA training.
No user conversations, credentials, raw traces, weights, or private experiment
ledgers belong in Git. Report new vulnerabilities through GitHub's private
vulnerability reporting when available; never post credentials in an issue.
