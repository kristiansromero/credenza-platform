# Backtrace: What problem is this really solving?

## The underlying problem

Reading the field vocabulary backwards — Incoterms, HS codes, MSDS, COO certificates, IDD country codes, DA/DP payment terms, arbitration venues — this is unmistakably cross-border B2B trade, not domestic commerce. The presence of `requesting_user_id` vs `matching_user_id` and the SCS types (Procurement, Supply, Trading, Manufacturing, Logistics-Only, Turnkey) confirm it's two-sided matching, not a directory.

The problem statement that fits the data, in plain language:

> International procurement is fragmented across incompatible tools. A buyer needing a custom-manufactured product from overseas has to source candidates (Alibaba, trade shows, referrals), vet them on dimensions that have nothing to do with each other (financial health, certifications, freight capability, payment terms, legal jurisdiction), negotiate each dimension separately, and reconcile everything into a contract — all over email, WeChat, and Excel. The 5–10% of the deal value that goes to friction, brokers, and freight forwarders is the prize.

The prototype's pitch is implicit: collapse all five dimensions into one structured record, match on all of them simultaneously, and let the SCS itself become the working contract.

## Who the user is

The schema's depth tells you the target customer. SMBs don't write arbitration clauses or care about ESG fields. This is aimed at:

- Mid-market manufacturers and trading companies (esp. Asia → ROW)
- Procurement teams at companies too small for a full sourcing org but too sophisticated for Alibaba
- Possibly freight forwarders trying to move up the value chain

The closest competitive reference points are Alibaba (matching), ThomasNet (capability directory), Flexport (logistics layer), and Ironclad (contract layer) — all rolled into one. That's ambitious.

## What the spec gets right

1. **The five-dimensional model is genuinely good thinking.** Most B2B platforms collapse trade into "product listing + price" and lose the 80% of complexity that actually kills deals (incoterms, payment terms, certifications, jurisdiction). Splitting Industry / Technical / Logistics / Commercial / Others is the correct first abstraction.
2. **SCS as a persistent transaction dossier** — not a one-time RFQ, not a chat thread, but a living record — is the right primitive.
3. **The vocabulary is real.** Whoever spec'd this knows trade. HS codes, COO certs, DA/DP, MSDS, notarization — these aren't placeholders, they're the actual grammar of the industry. Domain experts would recognize themselves in this UI.
4. **Profile reuse across many SCSes** is the right efficiency play — fill once, apply many times.

## What's missing — and why each gap matters

1. **There is no matching engine.** The platform is called a matching platform, but `matching_user_id` is a manual dropdown selection. Nothing scores, filters, or ranks candidate suppliers against a buyer's SCS. Without this, the product is a structured CRM, not a marketplace.
2. **Requirements ≠ Capabilities, but the schema treats them identically.** Technical Profile has "Materials Requirements" — that field means *what I need* if I'm a buyer, and *what I can supply* if I'm a supplier. Using the same column for both makes matching algorithmically impossible. Needs either separate buyer/supplier profile variants or a structured `{operator, attribute, value}` model.
3. **Everything important is free-text.** "Materials Requirements" is a `<textarea>`. You cannot match "needs stainless 316L" to "manufactures with stainless 316L" if both sides write it however they want. Matching requires controlled vocabularies / taxonomies / tags on the high-value attributes. The flat 14-item industry category list won't scale — real procurement uses hierarchical taxonomies (UNSPSC, NAICS, HS).
4. **No negotiation primitives.** "Commercial Status: Negotiating" exists as a label, but there is no Quote, Bid, Counter-offer, Acceptance, or Award entity. Where does "supplier offered $42/unit, buyer countered $38" actually live? Today: nowhere.
5. **No documents.** Real trade is documents: PI, CI, BL, packing lists, certificates, inspection reports, LC paperwork. The schema has no file/attachment concept. This is table-stakes for a trade platform.
6. **No messaging / timeline / audit log.** Trade is conversational and regulated. The platform needs threaded communication and an immutable event history per SCS. Both are absent.
7. **No trust layer.** "Health Indicator" is a self-reported dropdown. International trade fraud is rampant — adoption requires KYB verification, sanctions / restricted-party screening (legally required in most jurisdictions), trade references with weight, dispute history, and platform-observed transaction history. None of this exists.
8. **The Matching table is conceptually broken.** Using `user_id` as PK means a user can match to exactly one SCS, ever. Should be composite `(scs_id, user_id)`. Worse, it's unclear whether "Matching User Profile" means *candidate match* (top of funnel, system-suggested) or *selected match* (the chosen supplier). These are different entities serving different stages and should not be conflated. Right now both notions are jammed into one table while `scs.matching_user_id` adds a third contradictory representation.
9. **No organization model.** Real deals involve multiple humans at one company (procurement officer, logistics, legal, signatory). The schema is user-centric and denormalizes org info onto User Identity. This breaks the moment two people from the same company want to collaborate on one SCS.
10. **No carrier / freight integration.** Logistics Profile captures container dimensions and Incoterms in beautiful detail — and then those numbers go nowhere. No quoting, no carrier selection, no actual booking.
11. **No language / localization.** A cross-border platform with no language fields is going to hit a wall the moment a Vietnamese supplier and a German buyer try to use it.
12. **"Others Profile" is overloaded.** It mixes legal clauses (deal-level concerns), PESTEL environment notes (analyst commentary), approval workflows (org-level), and contract template fields. These should split apart.

## Verdict

The prototype mistakes *describing a transaction* for *producing transactions*.

**What's modeled extremely well:** the contract — the metadata of a finished cross-border deal. The vocabulary is right, the dimensional decomposition is insightful, and a procurement lead would recognize their world here. As a stakeholder demo to validate that the team understands the domain, it would absolutely land.

**What's not modeled at all:** the deal-making — discovery, ranking, trust, negotiation, document flow, conversation, awards. These are the hard problems in this space, and they're exactly the problems a "matching platform" exists to solve. The current spec is the destination state of a deal, not the engine that gets you there.

**Honest summary:** this is a strong v0 of the data dictionary for the product, and a weak v0 of the product itself. The next pivot isn't more fields — it's adding the four missing entities that turn this from a structured form-filler into a marketplace:

- **Quote / Bid** — commercial negotiation
- **Match Candidate** — scored, system-generated, distinct from selected match
- **Document** — attachments, with type/role
- **Event / Message** — timeline, audit, communication
