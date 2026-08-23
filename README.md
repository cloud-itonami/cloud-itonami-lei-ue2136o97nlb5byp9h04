# cloud-itonami-lei-ue2136o97nlb5byp9h04

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by McDonald's Corporation.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**McDonald's Corporation**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: McDonald's Corporation
- **LEI (ISO 17442)**: [UE2136O97NLB5BYP9H04](https://search.gleif.org/#/record/UE2136O97NLB5BYP9H04) (GLEIF-verified)
- **Jurisdiction**: US-DE
- **Website**: https://www.mcdonalds.com
- **Ticker**: MCD (NYSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 16 verified registry facts with per-fact provenance (9 about the
  entity, its registration, issuer, legal form and securities; 7 one-per-direct-child).
  **Generated** — see below.
- `scripts/verify-facts.cljs` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljs           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljs --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO requests back the file (`CHECKED 11` when it was written,
2026-08-23T05:30Z, golden copy 2026-08-22T16:00Z) — the LEI record (legal name
`MCDONALD'S CORPORATION`, jurisdiction `US-DE`, entity **ACTIVE**, registration
**ISSUED** with the next renewal due 2026-10-26, last updated 2025-10-17,
`FULLY_CORROBORATED`, conformity flag `CONFORMING`; entity status and
registration status are different fields and are recorded separately), its
**142 ISINs**, read from `meta.pagination.total` of a 15-per-page request that
spans 10 pages — at this issuer's volume the identifiers turn over as
instruments mature and are issued, so the count is recorded and the list is
deliberately not mirrored (the `:source/note` on the count says exactly this,
so a bare count is never ambiguous) — its managing LOU and LEI-issuer
accreditation (Bloomberg Finance L.P., LEI `5493001KJTIIGC8Y1R12`, marketing
name Bloomberg, accredited 2017-04-13), registration authority `RA000602`
(Division of Corporations, Department of State, Delaware, registration number
`619321`), ISO 20275 legal form `XTIQ` (`Corporation`, US-DE), reporting
exceptions at both consolidation levels (`NO_KNOWN_PERSON` — GLEIF records no
parent, direct or ultimate, because no single person or entity controls this
one), and **7 direct children**, read from `meta.pagination.total` of a
15-per-page request and — because the whole list fits in that one page — each
child mirrored as its own `:direct-child` entity (McDonald's Restaurants of
Canada Limited `5493008D7OZI1JLR7K69`, MCD APMEA SINGAPORE INVESTMENTS PTE.
LTD. `549300GV7WR1LK9YUK25`, MCDONALD'S INDIA PRIVATE LIMITED
`335800FAE4ZOCFEOZI77`, MCD GLOBAL FRANCHISING LIMITED `549300TE16IFHWH0X595`,
McD Luxembourg Real Estate S.à r.l. `549300S9FKPPNR5V6W37`, MCD ASIA PACIFIC,
LLC `549300WDCVDWY606YY69`, MCDONALD'S RESTAURANT OPERATIONS INC.
`549300Z46MTU5S01NE63`). The `direct-parent` and `ultimate-parent` endpoints
answered `404` because GLEIF publishes the exception side of that pair for this
entity, which the checker treats as a fact rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the
live sources, `1` a citation broke or a fact drifted, `3` the check could not be
performed at all — an absent `facts.edn`, or every request failing at the
transport level. A check that could not run must not be indistinguishable from a
check that ran and found nothing, so it refuses to report a pass rather than
exiting 0. All outcomes were exercised before this landed: unmodified `0`;
`:securities/isin-count` edited `142` → `143` → `1` naming
`DRIFT gleif-isins :securities/isin-count`; the
`gleif-direct-child-335800fae4zocfeozi77` entity deleted → `1` naming
`ADDED gleif-direct-child-335800fae4zocfeozi77`; one direct child's
`:company/status` rewritten `ACTIVE` → `LAPSED` → `1` naming `DRIFT` on that
child; both `:relationship/exception-reason` values rewritten to
`NON_CONSOLIDATING` → `1` naming `DRIFT` on both
`gleif-direct-parent-reporting-exception` and
`gleif-ultimate-parent-reporting-exception`; the GLEIF host in the checker
rewritten to an unresolvable name → `3` (`INCONCLUSIVE … refusing to report a
pass`).

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
