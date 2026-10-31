# Muster anchors

Daily signed anchors for every record in [Muster](https://muster-web-chi.vercel.app). They let anyone check a Muster record without trusting Muster.

Each day at 00:05 UTC, Muster rolls the previous day's ledger entries into a Merkle root per organization and one global root, signs the result with its anchor key, and commits it here as `anchors/<yyyy>/<yyyy-mm-dd>.json`. Every file names the hash of the day before (`prev_anchor_sha256`), so a missing or rewritten day shows up as a break in the chain. Days with no entries are anchored too.

Muster can't quietly change a record after its day is anchored: the root in this public history would no longer match.

## Verifying a record

- **In a browser:** open the record's verify page (`/p/<passport code>`). It downloads the bundle, recomputes the chain, and checks it against the anchor here.
- **Offline:** download the record's bundle, then run `npx @tenney/verify <bundle.json>`.

## Keys

The public anchor keys are in [`keys/anchor-keys.json`](keys/anchor-keys.json), identified by their RFC 7638 thumbprint (`key_id`). Retired keys stay listed so older anchors still verify. The same keys are pinned in `@tenney/verify` and in the Muster web app; they are never taken from a bundle.

Nothing in this repository is secret. Only Muster's anchor job writes to it.
