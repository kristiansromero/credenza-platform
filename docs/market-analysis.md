# Market Analysis: Cross-Border B2B Trade Matching

**Prepared for:** Credenza Platform
**Date:** 2026-05-22
**Grounded in:** `Problem Statement.md` (prior analysis of the HTML prototype)
**Note on dates:** Some cited sources carry 2026 publication dates because vendors backdate evergreen content; treat them as current-state snapshots, not future projections.

---

## 1. Executive Summary

The cross-border B2B trade space splits cleanly into **four incumbent layers, each best-in-class at one dimension of a deal — and a fifth tier of AI-native entrants now trying to collapse them**:

| Layer | Best-in-class examples | What they do well | What they don't do |
|---|---|---|---|
| **Discovery / matching** | Alibaba, Global Sources, Made-in-China, IndiaMART | Scale, supplier breadth, payment escrow | Free-text listings, no structured negotiation, trading-company clutter |
| **Capability directory** | ThomasNet, Xometry, Europages | Technical capability search, certifications | Western-centric, weak on commercial/logistics dimensions |
| **Logistics execution** | Flexport, Freightos, Forto | Carrier quoting, booking, visibility | Stand-alone — doesn't see the procurement upstream |
| **Trust / compliance** | Sayari, Kharon, Dun & Bradstreet | Ownership graphs, sanctions, KYB, credit | Sold as data API, not embedded in deal flow |
| **Contracts** | Ironclad, DocuSign CLM, Icertis | Clause libraries, redlining, e-signature | Generic legal — doesn't model trade primitives (Incoterms, HS, COO) |
| **Emerging AI-native trade** | Tridge, Cronwell, VertragX, Connglo, Andercore, SourceReady | Capability matching, AI outreach, structured RFQs, intel | Each owns 1–2 dimensions — none span the full SCS lifecycle |

**No single player owns the persistent, five-dimensional deal dossier that lives across discovery → negotiation → contract → execution → reorder.** That's Credenza's defensible territory — *if* it ships the four missing entities (Quote, Match Candidate, Document, Event) the Problem Statement identified, because incumbents are circling the same gap from different starting points.

**The window is real but closing.** Tridge, Cronwell, and VertragX are converging on parts of the same thesis in 2025. Credenza's edge is its native five-dimensional model and the SCS-as-dossier primitive; it must turn those into shipped product before the AI-native cohort matures.

---

## 2. The Competitive Landscape

### 2.1 Incumbent B2B Marketplaces (Discovery + Payment)

#### Alibaba.com
**Specializes in:** Sheer scale of Chinese supply + buyer protection escrow (Trade Assurance).

- **Mechanics:** Buyers find suppliers via search, direct chat, or RFQ posting. RFQs are broadcast to the seller pool, who bid. Trade Assurance acts as payment escrow with a 15-day post-delivery dispute window. ¹
- **Scale (claimed):** 160M+ Trade Assurance orders, 37M buyers, 200K suppliers, 280M products. ²
- **Verification tiers:** Gold Supplier → Verified Supplier → Trade Assurance Supplier (escalating third-party assessment). ³
- **Strengths:** Network effects, multi-language, integrated payments across 200+ countries, dispute mediation.
- **Weaknesses:**
  - **Trading-company pollution.** The majority of listings are traders reselling factory goods with 10–30% markup, not factories. ⁴
  - **No structured negotiation primitives** — price/MOQ/terms are negotiated in unstructured chat. Disputes hinge on whether agreement was captured *inside* the Alibaba chat tool because off-platform agreement carries no weight. ²
  - **Self-described capabilities only** — "Materials Requirements" is a free-text field on both sides; matching is keyword/category, not structured.
  - **Buyer-pulls-from-seller orientation** — the data model is "product listing," not "deal dossier."

**Verdict for Credenza:** Alibaba is the *de facto* discovery layer the target user already grudgingly uses, and the layer they most want to leave. Credenza should not compete on supplier breadth — it should compete on **deal structure after first contact**.

#### Global Sources, Made-in-China, IndiaMART
Same archetype, different geographies and tiers.
- **Global Sources** — Hong-Kong-anchored, biased toward verified Chinese exporters with stronger trade-show heritage (its physical Sourcing Fairs in HK feed the platform).
- **Made-in-China (MIC)** — Operated by Focus Technology, China-domestic-supplier focus, slightly stronger industrial machinery vertical.
- **IndiaMART** — India's analog; mid-market and SMB; weaker on cross-border export workflows than its name suggests — most volume is domestic Indian B2B.

All three share Alibaba's structural limits: free-text profiles, no matching engine, no document/contract primitives.

---

### 2.2 Capability Directories (Industrial Discovery)

#### ThomasNet (now Thomasnet, owned by Xometry)
- **Specializes in:** U.S. industrial supplier discovery by capability — *who can make this part, with this certification, in this region*.
- **Strengths:** Genuine capability taxonomy (NAICS-aligned), certifications first-class (ISO, AS9100, ITAR-registered), strong on engineering-grade procurement.
- **Weaknesses:** U.S./Canada-centric (a problem for cross-border Asia→ROW); discovery only, no transaction layer; legacy SEO-style UX.

#### Xometry
- **Specializes in:** Instant-quote custom manufacturing (CNC, sheet metal, injection molding, 3D printing) — buyer uploads CAD, Xometry quotes and routes to a vetted fabricator network.
- **Strengths:** Real instant pricing, real production capability matching, owns the customer relationship and the supplier network.
- **Weaknesses:** Narrow to make-to-print parts; not a marketplace for *finished* goods or trading-company deals; primarily NA/EU supply base.

**Verdict for Credenza:** ThomasNet's hierarchical capability taxonomy is the model the Problem Statement's "flat 14-item industry category list" should evolve toward. Xometry proves buyers will accept algorithmic matching when the inputs are structured (CAD file) — Credenza's challenge is to structure the *non-CAD* dimensions (commercial, logistics, others) into machine-comparable form.

---

### 2.3 Logistics Execution Layer

#### Flexport
- **Specializes in:** Digital freight forwarding — ocean, air, customs, with software-first visibility.
- **Strengths:** End-to-end execution (booking → customs → delivery), modern API, financing add-on.
- **Weaknesses:** Has retrenched repeatedly since 2023 layoffs; service quality variability; doesn't *find* you a supplier — starts after the deal exists.

#### Freightos
- **Specializes in:** Marketplace for instant ocean/air freight rates (the "Kayak for freight" pitch).
- **Strengths:** Real comparative quotes from many forwarders, public Freightos Baltic Index for spot rates, listed on Nasdaq.
- **Weaknesses:** Quote-and-book layer only; no procurement upstream, no contract downstream.

**Verdict for Credenza:** The Problem Statement called out that "Logistics Profile captures container dimensions and Incoterms in beautiful detail — and then those numbers go nowhere." Flexport/Freightos APIs are the natural integration — Credenza shouldn't build a freight forwarder, but the SCS should *trigger* freight quotes the moment Logistics Profile + award is committed.

---

### 2.4 Trust / Compliance Layer

#### Sayari
- **Specializes in:** Entity-resolved KYB and sanctions screening via beneficial-ownership graph traversal.
- **Strengths:** 10.6B+ records, 250+ jurisdictions, 40+ sanctions lists, 4B+ trade transaction records, API-first, sub-second batch screening. Resolves *entities*, not strings — eliminates ~72% of name-match false positives. ⁵ ⁶
- **Weaknesses:** Sold as data infrastructure; not embedded in deal-flow UI; expensive at enterprise tier.

#### Kharon
- **Specializes in:** Analyst-curated sanctions intelligence, particularly Russia/China narrative depth and OFAC 50% Rule attribution.
- **Strengths:** Human-expert-graded data, used by regulators and Fortune 100 compliance teams, AI/LLM-readable structure. ⁷
- **Weaknesses:** Boutique advisory model — not built for self-serve marketplace integration.

#### Dun & Bradstreet
- **Specializes in:** Credit risk, financial health, DUNS identity.
- **Strengths:** The de facto business-identity standard, deep on creditworthiness and payment behavior.
- **Weaknesses:** Hierarchy coverage stops ~2 levels deep on ownership; not designed to surface adversarial risk; mostly Western coverage by financial depth. ⁸

**Verdict for Credenza:** The Problem Statement's "Health Indicator is a self-reported dropdown" gap is exactly what these vendors exist to fix — but for **mid-market buyers**, embedding Sayari-grade KYB inline (instead of forcing them to buy a $50K/yr seat to Sayari themselves) is a credible product wedge. Credenza could be the first platform where a mid-market buyer sees structured beneficial-ownership + sanctions exposure on a supplier *while filling out the SCS* — not after the deal explodes.

---

### 2.5 Contract Layer

#### Ironclad, DocuSign CLM, Icertis
- **Specialize in:** Contract drafting, clause libraries, redlining, approval workflows, e-signature, repository.
- **Strengths:** Mature CLM workflows, integrations to CRM/ERP, AI-assisted redlining.
- **Weaknesses:** **Trade-blind.** They treat a cross-border PI/CI like any other commercial agreement — no native understanding of Incoterms semantics, HS classification, payment-instrument (LC/DA/DP) implications, or the linkage between the legal document and the physical goods/customs/freight chain.

**Verdict for Credenza:** Don't rebuild CLM; integrate. But the Problem Statement is correct that the **SCS itself can act as the working contract** in a way that no generic CLM can replicate, because the SCS already knows the Incoterm, the HS code, the COO, the payment terms — all the metadata a generic CLM laboriously asks for separately.

---

### 2.6 Emerging AI-Native Trade Platforms (The Real Competition)

These are the players actually attempting some version of Credenza's thesis. Each owns a slice; none owns the whole.

#### Tridge
- **Pitch:** "Intelligence that moves markets" — AI-native trade execution platform across 200+ markets with 682M+ trade data points and 480K+ verified users. ⁹
- **Strengths:** Years of native trade data (price, supplier, commodity), genuine workflow execution (not just analytics), 60+ export-market buyer reach. Strongest emerging player by data depth.
- **Weaknesses:** Commodity/agriculture-leaning origin; less depth on engineered goods and complex five-dimensional matching.
- **Threat level to Credenza:** Highest. Closest to the same long-term vision.

#### Cronwell
- **Pitch:** "AI procurement for mid-market manufacturers" — automates 90% of tactical buying, runs reverse auctions, sits inside Teams/WhatsApp/email. ¹⁰
- **Strengths:** Bullseye on **the same target customer as Credenza** ("mid-market manufacturers"). NPI sourcing, BOM-aware, ERP/PLM integrations, supplier discovery via pre-qualified network.
- **Weaknesses:** Buyer-side ERP plug-in framing — does not model supplier-side as a peer entity; assumes the supplier is just a vendor to be tracked, not a co-equal user filling out their own profile.
- **Threat level to Credenza:** High *for the buyer wedge*. Credenza's two-sided SCS model is the differentiator.

#### VertragX (launch Dec 2025)
- **Pitch:** "AI-assisted industrial sourcing" with a "Living Profile" — supplier uploads technical docs/certs and the platform structures them into a "searchable knowledge graph," then matches RFQs by capability. ¹¹
- **Strengths:** This is almost literally the Problem Statement's gap #3 fix: turn free-text capability into structured matchable attributes. Win-probability scoring per RFQ for suppliers.
- **Weaknesses:** Pre-launch; industrial-engineering-only framing; doesn't show full five-dimensional model.
- **Threat level to Credenza:** Medium-high — directly addresses the matching engine + capability taxonomy gap.

#### Connglo
- **Pitch:** "Global B2B tender platform that actually works" — anonymous structured tenders with verified network across 150+ countries. ¹²
- **Strengths:** Demands (anonymous RFQs) + Offers as first-class entities — explicitly addresses Problem Statement gap #4 ("no negotiation primitives"). Anonymous discovery → reveal-when-ready is a smart trust pattern.
- **Weaknesses:** Generalist ("industry-agnostic across 150+ countries") usually means depth-light on each vertical; no evidence of trade-specific primitives (Incoterms, HS, COO).
- **Threat level to Credenza:** Medium — owns the tender slice cleanly but lacks trade-grammar depth.

#### Andercore (Berlin)
- **Pitch:** AI-powered industrial trade and distribution for infrastructure/energy/construction materials, Europe-focused. ¹³
- **Strengths:** Vertical depth in commodity industrials, integrated logistics + financing + payments, runs the **whole order workflow** (the integration Credenza is proposing).
- **Weaknesses:** Vertical (materials only), regional (Europe + MENA/Asia spokes); a managed-service hybrid, not pure platform.
- **Threat level to Credenza:** Low-direct, high-as-template. Worth studying as proof that integrated trade workflow has a buyer.

#### SourceReady
- **Pitch:** "AI agent for product sourcing" — 1.2M suppliers, customs-data-verified, region/tariff analysis, AI supplier outreach. ¹⁴
- **Strengths:** Customs-data verification is a strong trust signal (you can't fake a customs filing). Tariff/FTA-aware landed-cost simulation. AI does the supplier follow-up at scale.
- **Weaknesses:** Single-sided (buyer agent); does not model the supplier as a profiled platform user.
- **Threat level to Credenza:** Medium for the discovery wedge, low for the dossier wedge.

---

## 3. Gap Map: What's Still Unaddressed

Cross-referencing the Problem Statement's 12 design gaps with the competitive scan:

| # | Gap from Problem Statement | Who closes it today | Residual gap |
|---|---|---|---|
| 1 | No matching engine | Xometry (CAD-only), VertragX (launching), Tridge (commodity) | **Multi-dimensional matching across all five Credenza axes simultaneously — unaddressed.** |
| 2 | Requirements ≠ Capabilities conflation | VertragX (capability side), Cronwell (requirements side) | **No platform models both sides with structured `{operator, attribute, value}` symmetry.** |
| 3 | Free-text where taxonomies are needed | ThomasNet (capabilities), HS/UNSPSC databases exist | **No platform forces taxonomy adoption on commercial + logistics + legal dimensions, only on product.** |
| 4 | No negotiation primitives (Quote/Bid/Counter/Award) | Alibaba RFQ (weak), Connglo tenders, Cronwell reverse auctions | **No platform persists a structured negotiation timeline inside a longitudinal deal dossier.** |
| 5 | No documents | DocuSign + DropBox combos, Bolero/essDOCS for shipping docs | **No platform binds documents to SCS state transitions (e.g., "BL required to release escrow").** |
| 6 | No messaging/timeline/audit log | Alibaba chat (weak), Slack/email (off-platform) | **No platform offers an immutable, regulator-grade event log per deal.** |
| 7 | No trust layer | Sayari/Kharon/D&B (API only); Alibaba badges (weak) | **No platform embeds enterprise-grade KYB+sanctions inline for mid-market users at mid-market price.** |
| 8 | Broken Matching table conflates candidate vs selected match | None — incumbents don't even have a matching engine | Schema-internal; fixable. |
| 9 | No organization model (multi-user per company per deal) | CLMs (Ironclad, etc.) handle this for legal, not for trade | **No trade platform models procurement officer + logistics + legal + signatory as collaborators on one SCS.** |
| 10 | Logistics data goes nowhere | Flexport/Freightos via API | **No platform pre-quotes freight from SCS Logistics Profile at the moment of award.** |
| 11 | No language/localization | Alibaba (translated chat), Tridge (intl reach) | **No platform offers field-level bilingual SCS rendering for legally binding terms.** |
| 12 | Overloaded "Others Profile" | CLMs split this; trade platforms don't | Schema-internal; fixable. |

### The four highest-leverage open gaps

1. **True multi-dimensional matching (gap #1 + #2 + #3 combined).** Tridge has the data, VertragX has the capability graph, but no one is matching simultaneously on Industry × Technical × Logistics × Commercial × Others with structured operators. Credenza's five-dimensional model exists *because* this is unsolved.

2. **Embedded mid-market trust layer (gap #7).** Sayari-grade intelligence is gated behind enterprise API pricing. The mid-market user — Credenza's exact target — is the one most exposed to fraud and least able to afford the data. A Credenza ↔ Sayari/Kharon partnership where the cost is amortized across the platform (or replaced by a more affordable equivalent like Sayari's lower tiers + OpenSanctions) would be a real product wedge.

3. **The SCS-as-living-contract primitive (gap #4 + #5 + #6 combined).** Connglo has tenders, DocuSign has e-sign, Ironclad has redlines — nobody has a *single persistent dossier* where the Quote/Counter/Award lives next to the PI/CI/BL next to the audit trail next to the Incoterm next to the COO. The Problem Statement's central insight ("SCS as persistent transaction dossier") is precisely the unoccupied ground.

4. **Organization-level collaboration on cross-border deals (gap #9).** Every adjacent player models a single user. Real deals at Credenza's target customer size involve a procurement officer, an internal logistics coordinator, in-house counsel, and an authorized signatory — often plus the same on the supplier side. No platform in the scan handles this as a first-class concept.

---

## 4. Market Sizing (TAM / SAM / SOM)

### 4.1 Definitions specific to Credenza

| Tier | Definition | Estimate | Confidence |
|---|---|---|---|
| **TAM** | Global annual software/platform spend by cross-border B2B trade participants on *any* product Credenza could plausibly compete with (matching, dossier, KYB, CLM, procurement SaaS) — i.e. the total revenue pool Credenza is reaching into. | **~$8–14 B/year** | Medium |
| **SAM** | Subset Credenza can realistically address in 3–5 years given its mid-market ICP, two-sided model, Asia↔ROW corridor focus, and product depth on trade grammar. | **~$2.5–4.5 B/year** | Medium |
| **SOM** | Realistic 5-year capture given the AI-native competitive cohort and two-sided cold-start friction. | **$40–150 M ARR by Year 5** | Lower (execution-dependent) |

The number that matters for investor and product framing is the **SAM**, because TAM is too generous a definition for a structured-deal platform (most of it is already locked up in Coupa/Ariba/CLM contracts that won't churn fast) and SOM is too execution-sensitive to defend with data.

### 4.2 Method 1 — Top-down from cross-border B2B trade flow

| Layer | Figure | Source |
|---|---|---|
| Global international trade (goods + services, 2024) | **~$33 T** | UNCTAD/WTO ¹⁵ |
| B2B share of e-commerce (2019, last clean disaggregation) | **82%** | UNCTAD ¹⁶ |
| Global digital trade (digitally delivered + digitally ordered, 2024) | **$7.23 T** (12.1% CAGR 2020–24) | UNCTAD/ITC/WTO ¹⁷ |
| Digitally-ordered cross-border trade (2024, the closest available proxy for "trade running through platforms") | **~$2.62 T** (12.3% CAGR) | UNCTAD/ITC/WTO ¹⁷ |
| Implied 2028 figure at current CAGR | **~$4.1 T** | Computed |

**Conversion to software TAM:** Platforms typically capture **0.3–0.6%** of the GMV they intermediate via subscriptions + fees (Alibaba.com's roughly $3.4B revenue against an estimated $50–100B intermediated GMV implies ~3–7%, but most of that is ad/lead-fee revenue, not pure SaaS; the SaaS-equivalent slice is ~1%). For a *non-marketplace* dossier SaaS layer that sells subscriptions rather than take-rate, the realistic capture is closer to **0.2–0.4% of intermediated cross-border B2B GMV**.

- 0.2% × $2.62 T = **$5.2 B**
- 0.4% × $2.62 T = **$10.5 B**

→ TAM range from this method: **$5–11 B/year, current**; ~$8–16 B by 2028.

### 4.3 Method 2 — Comparable-platform revenue stacking

Sum of revenue currently earned by platforms Credenza partially substitutes for or competes with:

| Platform | Approx. annual revenue | Notes |
|---|---|---|
| Alibaba.com B2B (International Commerce Wholesale, FY2025) | **~$3.4 B** (RMB 23.8 B) | Direct B2B marketplace incumbent ¹⁸ |
| Global Sources + Made-in-China + IndiaMART (combined) | **~$0.8–1.2 B** | Smaller analogs; IndiaMART ~$140M FY24, others private |
| Tridge | **est. $50–150 M ARR** | Private; based on user base + listings |
| Coupa procurement | **~$1.0 B ARR** | Mid/enterprise procurement SaaS |
| SAP Ariba | **~$2.5 B ARR** | Enterprise — out of Credenza's ICP but anchors ceiling |
| Ironclad + DocuSign CLM + Icertis (CLM slice) | **~$0.8 B** | The contract layer Credenza augments rather than replaces |
| Sayari + Kharon + D&B (compliance/KYB *trade slice only*) | **~$0.5 B** | Trade-relevant portion only |
| Flexport + Freightos (digital freight software revenue, ex-freight pass-through) | **~$0.3 B** | Software/platform layer only |
| **Sum of adjacent revenue pool** | **~$9–11 B** | Credenza's reach-into market |

Excluding the enterprise tier (Ariba, large CLM deals) that Credenza will not target as a mid-market platform leaves **~$6–8 B** of directly contestable revenue today.

→ TAM range from this method: **$6–11 B/year, current**.

The two methods triangulate around **$8–12 B** as the upper-bound TAM. Use **~$10 B** as the working figure.

### 4.4 Method 3 — Bottom-up from ICP × ACV

This is the hardest number and the most strategically important — it sets pricing and sales-motion assumptions.

**Step 1 — Estimate addressable organizations globally.**

| ICP segment | Estimated global count | Inclusion logic |
|---|---|---|
| Mid-market manufacturers ($10M–$500M revenue) doing meaningful cross-border trade | ~**150–250 K** | UN industrial census + Statista mid-market manufacturer counts, filtered for >20% revenue from cross-border |
| Trading companies (cross-border focus) | ~**60–100 K** | Concentrated in China (~40K), India (~10K), Türkiye, Vietnam, UAE, Singapore |
| Mid-market non-manufacturing importers (distributors, retail chains, brands) | ~**100–200 K** | The "Alibaba power buyer" tier |
| Freight forwarders + sourcing agents (potential supplier-side platform users) | ~**25–40 K** | FIATA member orgs + estimated long tail |
| **Total addressable organizations** | **~330–590 K** | Use midpoint **~450 K** |

**Step 2 — ACV assumption.**

Credenza's pricing has not been set. Reasonable bracketing from comparables:
- Bottom: SaaS for mid-market procurement utility → **$3–6 K/year**
- Middle: Cronwell-like AI procurement seat → **$10–20 K/year**
- Top: Bundled with embedded KYB/sanctions add-on (a wedge feature) → **$25–40 K/year**

Use **midpoint ACV of ~$15 K/year/org** for the addressable population, weighted across small/medium/large customers.

**Step 3 — Compute.**

| Calculation | Value |
|---|---|
| **TAM (if 100% adoption — theoretical ceiling)** | 450 K × $15 K = **$6.75 B/year** |
| **SAM (realistic addressable in 3–5 yrs)** | ~40% of TAM = orgs reachable with English/Mandarin product, in Credenza's target corridors (Asia → US, EU, MENA), willing to adopt new SaaS = ~180 K × $15 K = **~$2.7 B/year** |
| **SOM (5-year capture given AI-native competition)** | 1–3% penetration of SAM = 1,800–5,400 paying orgs × $15 K = **$27–80 M ARR**. Realistic mid-case: **~2,500 orgs at ~$18 K mixed ACV ≈ $45 M ARR**. |

### 4.5 Triangulation summary

| Method | TAM | SAM | SOM (Y5) |
|---|---|---|---|
| Top-down trade flow | $5–11 B | n/a | n/a |
| Comparable revenue stack | $6–11 B | n/a | n/a |
| Bottom-up ICP × ACV | $6.75 B | $2.7 B | $45 M ARR (mid-case) |
| **Synthesized** | **~$8–14 B** | **~$2.5–4.5 B** | **$40–150 M ARR** |

**A useful one-line framing for investors:**
> *"A ~$10 B global market for structured cross-border trade-deal software, of which ~$3 B is realistically addressable to a mid-market, Asia↔ROW–focused two-sided platform in the next 3–5 years. Credible Y5 capture target: $50–100 M ARR."*

### 4.6 Sensitivities and caveats

- **TAM is highly sensitive to the question "is Credenza a SaaS or a take-rate marketplace?"** A take-rate model on intermediated trade GMV puts the upper TAM closer to **$25–50 B** (1–2% of $2.6T cross-border digital trade). But take-rate is much harder to extract for the dossier-layer business Credenza is — buyers won't pay 2% on a deal Credenza didn't *source*. Default to SaaS sizing.
- **The "mid-market manufacturer" count is the load-bearing assumption.** Public stats on this segment are weak. The 150–250K range is built from UN industrial census data, Statista mid-market counts, and judgment — treat as ±30%.
- **ACV is a guess.** If pricing lands at $30K (top-end), SAM doubles; if at $5K (low-end), SAM halves. Run the model both ways before committing to a sales motion.
- **The take-rate ceiling for embedded financing or freight commissions is not in any of these numbers.** If Credenza monetizes a slice of trade finance, payments, or freight-forwarding referrals, add another $1–3 B of TAM-equivalent revenue pool. This is the path Andercore is on.
- **Stale data:** UNCTAD's last clean B2B-share disaggregation is 2019. Trade composition has shifted (China share down, ASEAN/Mexico up) since. The trade-flow magnitude is robust; the segmentation is rough.

### 4.7 What this means for strategy

1. **Don't lead with TAM.** A $10B TAM is real but not VC-electrifying on its own. Lead with the *gap-closure* thesis from §3 and *use* the $10B as substantiation, not the headline.
2. **The SAM number ($2.5–4.5B) is the right number to anchor a Series A/B story.** It maps cleanly to a 1–3% capture creating a $30–150M ARR business — i.e., a venture-fundable trajectory.
3. **Pricing strategy directly shapes the size of the opportunity.** Bundled KYB + freight quoting + dossier subscription at $20–30K ACV is a fundamentally different (and bigger) business than $5K/seat SaaS. Credenza should run customer-discovery interviews specifically to validate which ACV bracket is achievable; the gap between them is a 2–4× swing in SAM.
4. **Asia↔ROW concentration is a feature, not a limit.** China (~40K trading companies), India (10K+), Vietnam, Türkiye, UAE alone cover most of the ICP density. Don't water down the SAM by adding "global" — concentrate on those corridors.

---

## 5. Implications for Credenza

1. **Don't fight Alibaba on discovery breadth.** The win condition is "discovery via Alibaba/Tridge/SourceReady → import the candidate into Credenza → run the deal here." Credenza becomes the *de facto* dossier layer above whatever discovery surface fed it.

2. **The closest direct competitor by thesis is Tridge; by customer is Cronwell.** Tridge has more data and more reach; Cronwell has the same buyer ICP. Credenza wins by being two-sided (supplier as peer profile, not vendor record) and by going deep on the **trade-grammar** layer Cronwell skips.

3. **The four entities the Problem Statement called out (Quote, Match Candidate, Document, Event) map 1:1 to the largest unaddressed gaps.** Build those first; field schema work comes second.

4. **Trust is a paid wedge, not a free feature.** A Credenza tier that includes Sayari/Kharon-style KYB at mid-market pricing is a willingness-to-pay anchor and a defensible moat (data integrations + commercial deals neither Alibaba nor Cronwell will replicate quickly).

5. **The five-dimensional model is the brand story.** It's pre-articulated, defensible, and no competitor describes its data model this clearly. Lead with it externally.

---

### 4.8 Pressure-test before citing externally

Three items to validate before any of the numbers above appear in a pitch deck, memo, or external document:

1. **ACV bracket is the biggest swing factor.** $5K vs $30K is a 2–4× SAM swing. Needs customer-discovery interviews — not a guess. Run 15–25 ICP conversations specifically on willingness-to-pay for the bundled (dossier + KYB + freight-quote) offering before locking pricing assumptions.
2. **Mid-market manufacturer count is load-bearing.** Public statistics on this segment are thin. The 150–250K global estimate is assembled from UN industrial census data + Statista + judgment — treat as ±30%. If anything in the strategic story leans on this number, get a research firm or paid dataset to tighten the range.
3. **The investor-grade headline is the SAM, not the TAM.** A ~$14B TAM is real but not the lead. The fundable story is "$2.5–4.5B SAM, 1–3% capture → $30–150M ARR." Lead with that.

**One-line external framing:**
> A ~$10 B global market for structured cross-border trade-deal software, of which ~$3 B is realistically addressable to a mid-market, Asia↔ROW-focused two-sided platform in the next 3–5 years. Credible Y5 capture: $50–100 M ARR.

---

## 6. Risks & Caveats

- **Tridge's lead is real.** 12+ years of native trade data is not catchable; if Credenza tries to be a market-intelligence platform, it loses. The bet must be on *deal execution depth*, not data depth.
- **Two-sided cold start.** Five-dimensional profiles ask both sides to do significant unpaid work upfront. Without seeded supply (e.g., recruited freight forwarders, sourcing agents) on day one, the matching engine has nothing to match. Andercore solves this with a managed-service hybrid; Credenza will need an equivalent unlock.
- **Regulatory exposure.** Embedding KYB/sanctions screening shifts liability — if Credenza surfaces a "clean" verdict on a counterparty that turns out to be sanctioned, the platform becomes a defendant. Insurance + Sayari/Kharon attribution language must be wired in from day one.
- **Mid-market is real but narrow.** Too small for enterprise pricing, too sophisticated for self-serve SaaS. ACVs probably land $5K–$30K/yr — meaning Credenza needs hundreds of paying orgs per side before unit economics close. Long runway assumption.
- **Source dating.** Several cited sources carry 2026 publication metadata that may reflect vendor-side backdating rather than future events. The competitive picture is current as of mid-2025–early-2026 and is moving fast.

---

## 7. Recommendation

**Build Credenza as the persistent, two-sided, five-dimensional deal dossier layer that sits above existing discovery channels and below existing logistics/contract execution.** Compete on **structured negotiation + embedded trust + cross-dimension matching**, not on supplier breadth or commodity intelligence.

**Sequencing (highest leverage first):**
1. Quote / Bid / Counter-offer / Award as first-class entities (closes gaps #4 + #8).
2. Match Candidate (scored, system-generated) separated from Selected Match (closes gaps #1 + #8).
3. Structured `{operator, attribute, value}` model + UNSPSC/HS/NAICS taxonomies on matchable fields (closes gaps #2 + #3).
4. Document entity bound to SCS state machine (closes gap #5).
5. Event/Message log with immutable audit (closes gap #6).
6. KYB partnership (Sayari, Kharon, or OpenSanctions-based equivalent) inline in supplier profile (closes gap #7).
7. Organization model (multi-user per company per SCS) (closes gap #9).
8. Logistics Profile → freight-quote API integration (Flexport / Freightos) (closes gap #10).

Everything else (language/localization, splitting Others Profile, fixing the Matching table PK) is mechanical and follows.

---

## 8. Sources

1. Alibaba.com Buyer Central — "How sourcing on Alibaba.com works." https://buyer.alibaba.com/page/HowItWorks/Page.html
2. Alibaba.com Seller Blog — "Trade Assurance: A Complete Guide to Secure B2B Transactions" (Mar 2026). https://seller.alibaba.com/blogs/2026/southeast-asia/b2b-trade/trade-assurance-complete-guide-alibaba-secure-payment
3. Alibaba.com — Supplier badge definitions (Gold, Verified, Trade Assurance). Same source as #1.
4. China-Electronics.com — "How to Use Alibaba: Complete Sourcing Guide (2026)." https://china-electronics.com/platforms/alibaba/
5. Sayari — "Trade Compliance & Export Controls Solution." https://sayari.com/risk/trade-compliance/
6. Sayari — "KYC & Due Diligence." https://sayari.com/risk/kyc/
7. Kharon — "Insurance Sanctions Compliance." https://www.kharon.com/solutions/industries/insurance
8. Sayari — "Sayari vs D&B." https://sayari.com/dnb/
9. Tridge — Homepage and product positioning. https://www.tridge.com/
10. Cronwell — "AI Procurement for Mid-Market Manufacturers." https://www.cronwell.ai/
11. VertragX — "AI-Assisted Industrial Sourcing." https://vertragx.com/
12. Connglo — "The Global B2B Tender Platform." https://connglo.com/
13. Andercore — "AI-powered industrial trade and distribution." https://www.andercore.com/
14. SourceReady — "AI Agent for Product Sourcing." https://getsourceready.com/
15. UNCTAD — "Global E-Commerce Jumps to $26.7 Trillion." https://unctad.org/press-material/global-e-commerce-jumps-267-trillion-covid-19-boosts-online-retail-sales (anchors B2B share of e-commerce at 82% / $21.8T in 2019).
16. UNCTAD — "Business e-commerce sales and the role of online platforms" (Technical notes on ICT for development, No. 1). https://unctad.org/system/files/official-document/dtlecde2024d3_en.pdf (anchors business e-commerce sales reaching ~$27T across 43 economies in 2022).
17. ITC / UNCTAD / WTO — joint digital trade compilation, 2025. https://www.intracen.org/sites/default/files/media/file/media_file/2025/09/25/0103041315.pdf (digital trade $4.59T → $7.23T 2020–24; digitally-ordered trade $1.85T → $2.62T 2020–24).
18. Alibaba Group — FY2025 Annual Report and SEC filings. https://www.hkexnews.hk/listedco/listconews/sehk/2025/0626/2025062601064.pdf and https://www.sec.gov/Archives/edgar/data/1577552/000095017025090161/R45.htm (International Commerce Wholesale revenue RMB 23,835M FY2025 ≈ $3.3–3.4B).

**Not cited but referenced from general market knowledge:** Global Sources, Made-in-China (Focus Technology), IndiaMART, ThomasNet/Xometry, Flexport, Freightos, Ironclad, DocuSign CLM, Icertis. Specific recent metrics for these were not pulled in this pass due to upstream rate limits; recommend a follow-up research cycle if any of them become acquisition, partnership, or direct-competition targets.
