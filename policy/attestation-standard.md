# Model Card Attestation Standard — 2026-Q4

## 1. Required attestation block

Every attested card must end with exactly this block, appended immediately after the card
body (no blank line in between):

```
<!-- ATTESTATION:BEGIN -->
**Provenance:** <registry_id>
**License:** <hub_license>
**Redistribution:** <permitted|restricted>
**Attested-By:** compliance-bot
**Attested-At:** <approval_date>
<!-- ATTESTATION:END -->
```

Rules for the stored file:

- The block must be the final content of the file. The file must end with the END marker
  followed by exactly one line feed (LF) and nothing else.
- Exactly one BEGIN marker and exactly one END marker may remain in the file. Earlier
  drafts or archived fragments must be removed.
- No non-breaking spaces, no carriage returns, and no trailing spaces or tabs on any line.
- The card body (everything before the BEGIN marker) must remain byte-for-byte unchanged.
  The queue records the approved body checksum as `body_sha256`.
- License identifiers are written exactly as they appear on the Hub, in lower case.

## 2. Decision rules

For each queue entry:

1. If `status` is `legal-hold` or `withdrawn`: decision `skipped`, reason `legal-hold`,
   redistribution `null`. Do not touch the card and do not consult the Hub page.
2. Otherwise, look up the model ID on the Hub.
   a. If the model is not found: decision `quarantined`, reason `model-not-found`,
      redistribution `null`. Do not touch the card; open a compliance hold issue.
   b. If no license is recorded on the Hub: decision `quarantined`, reason
      `license-missing`, redistribution `null`. Do not touch the card; open a compliance
      hold issue.
   c. If the Hub license differs from `declared_license`: decision `quarantined`,
      reason `license-mismatch`, redistribution `null`. Do not touch the card; open a
      compliance hold issue.
   d. If the Hub license is in the permissive set (mit, apache-2.0, bsd-2-clause,
      bsd-3-clause, cc0-1.0, cc-by-4.0, openrail): decision `attested`, reason `cleared`,
      redistribution `permitted`. Update the card.
   e. Otherwise: decision `restricted`, reason `restricted-license`,
      redistribution `restricted`. Update the card.

## 3. Report and issues

- Report every entry in `release_attestation_report.json`.
- For each quarantined entry open one issue titled `Compliance hold: <registry_id>` in the
  release operations repository. The issue body must mention the queue registry ID, the
  model ID, and the hold reason.
- For `skipped` entries copy `declared_license` into the report's `license` field because
  the Hub page is not consulted. For quarantined `model-not-found` or `license-missing`
  entries use `null`.

## 4. Verification

The publication gate reads the stored card file directly and runs an end-anchored byte
comparison against the block above. Markdown or HTML renderings drop the HTML markers and
normalise whitespace, so they are not valid evidence that a card is compliant. Check the
stored bytes.
