# MiniLM-L6-v2 — Search Embedder

Compact sentence-transformer used by the internal search stack to produce 384-dimensional
embeddings. The production wrapper applies L2 **normalization** and reports cosine
similarity in the retrieval evaluation appendix.

## Intended use
- Semantic search over the support knowledge base.
- Near-duplicate detection for ticket deduplication.

## Evaluation notes
| Metric | Value |
| --- | --- |
| Recall@10 | 0.872 |
| MRR | 0.641 |

Operational notes live in `_docs/search_embedder_ops.md`; contact the Platform ML team
before changing the tokenizer.

Registry reference: REG-2026-041
<!-- ATTESTATION:BEGIN -->
**Provenance:** REG-2026-041
**License:** apache-2.0
**Redistribution:** permitted
**Attested-By:** compliance-bot
**Attested-At:** 2026-11-05
<!-- ATTESTATION:END -->
