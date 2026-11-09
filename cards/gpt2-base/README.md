# GPT-2 Base — Copywriting Assistant

Base autoregressive language model used as a baseline for the internal copywriting
assistant. Only the *tokenizer* and generation defaults are pinned in this card; fine-tuned
weights are tracked separately.

## Intended use
- Offline evaluation of decoding strategies.
- Baseline comparisons for internal instruction-tuning experiments.

## Notes
Do not expose raw completions to customers. See `_docs/decoding_defaults.md` for the
temperature and top-p settings used in the baseline harness.

Registry reference: REG-2026-042
<!-- ATTESTATION:BEGIN -->
**Provenance:** REG-2026-042
**License:** mit
**Redistribution:** permitted
**Attested-By:** compliance-bot
**Attested-At:** 2026-11-03
<!-- ATTESTATION:END -->
