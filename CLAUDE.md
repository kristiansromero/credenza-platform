# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository State

This repo is **pre-code**. As of this writing, the only artifact is `Problem Statement.md` — an analysis of an HTML prototype for a cross-border B2B trade matching platform. There is no build, no package manifest, no source tree, no test runner yet. Do not invent commands or scaffolding the user did not ask for; if asked to build something, clarify the intended stack first.

## Domain context (load before reasoning about features)

Credenza is a **cross-border B2B trade matching platform**, not a domestic marketplace or generic CRM. The vocabulary is real trade vocabulary — treat it as a constraint, not decoration:

- **SCS (Supply Chain Statement)** is the central primitive — a *persistent transaction dossier* (not an RFQ, not a chat thread) that lives across the life of a deal.
- **Five-dimensional profile model** (Industry / Technical / Logistics / Commercial / Others) — buyers and suppliers are described on all five axes simultaneously, and matching is expected to consider all of them. Do not collapse this back to "product + price."
- **SCS types**: Procurement, Supply, Trading, Manufacturing, Logistics-Only, Turnkey.
- **Target users**: mid-market manufacturers, trading companies (esp. Asia → ROW), and procurement teams too small for a full sourcing org but too sophisticated for Alibaba. **Not** SMBs and **not** enterprise. This shapes which features matter (arbitration clauses, COO certs, HS codes — yes; consumer checkout flows — no).
- Competitive reference points: Alibaba (matching) + ThomasNet (capability directory) + Flexport (logistics) + Ironclad (contracts), rolled into one.

## Known design gaps (from `Problem Statement.md`)

The current prototype models the *destination state* of a finished deal but does not model the *deal-making process*. When proposing features or schema, treat these as the open frontier — flagging them is more useful than papering over them:

1. **No matching engine** — `matching_user_id` is a manual dropdown. There is no scoring, filtering, or ranking.
2. **Requirements ≠ Capabilities conflation** — the same field ("Materials Requirements") is used for *what I need* (buyer) and *what I can supply* (supplier). Matching requires either separate buyer/supplier profile variants or a structured `{operator, attribute, value}` model.
3. **Free-text where taxonomies are needed** — high-value attributes (materials, certifications, categories) are `<textarea>`. Industry uses hierarchical taxonomies (UNSPSC, NAICS, HS) — propose controlled vocabularies on matchable fields.
4. **No negotiation primitives** — no Quote / Bid / Counter-offer / Acceptance / Award entity. "Negotiating" exists only as a status label.
5. **No documents** — trade *is* documents (PI, CI, BL, packing list, certificates, LC paperwork). The schema has no attachment concept.
6. **No messaging / timeline / audit log** — trade is conversational and regulated; both threaded communication and an immutable event history are absent.
7. **No trust layer** — "Health Indicator" is self-reported. Real adoption needs KYB, sanctions / restricted-party screening (legally required in most jurisdictions), trade references, dispute history.
8. **Matching table is broken** — `user_id` as PK means one user can match to exactly one SCS, ever. Should be composite `(scs_id, user_id)`. Also conflates "candidate match" (top of funnel, system-suggested) with "selected match" (chosen supplier) — these are distinct entities.
9. **No organization model** — schema is user-centric and denormalizes org info onto User Identity. Breaks the moment two people at one company collaborate on one SCS.
10. **Logistics data goes nowhere** — container dimensions and Incoterms are captured but there is no carrier quoting, selection, or booking.
11. **No language / localization** on a cross-border platform.
12. **"Others Profile" is overloaded** — mixes legal clauses, PESTEL notes, approval workflows, and contract template fields. Should be split.

The four entities flagged as the minimum to turn this from a form-filler into a marketplace: **Quote/Bid**, **Match Candidate** (scored, system-generated), **Document**, **Event/Message**.

## Working notes

- When discussing the prototype or proposing schema changes, ground arguments in `Problem Statement.md` — it represents prior analysis the user already agrees with. Don't re-derive it.
- When the user references "the prototype," they mean the HTML file analyzed in the previous session (not currently committed to the repo). Ask for it if you need to see it.
