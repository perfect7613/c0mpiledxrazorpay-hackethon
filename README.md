# Dubai Property 3D World

**Built at c0mpiled × Razorpay Hackathon**

An AI-powered real estate decision system for the Dubai market — combining market intelligence, ROI analysis, and photorealistic 3D property exploration into a single investor workflow.

---

## The Problem We Were Solving

Dubai real estate is a global market where most investors never visit in person.

Investors rely on static images, fragmented data across portals, and WhatsApp voice notes from brokers.  
Brokers lack shareable, high-quality assets and struggle to build trust remotely.

The gap: there's no platform that closes the loop between **data → insight → experience → decision**.

---

## PM Framing: What This Is (and Isn't)

This is not a listing platform. Not a 3D tour tool. Not an ROI calculator.

It's a **decision system** — built on the premise that the right question in real estate isn't *"show me the property"* but *"help me understand if this makes sense."*

That framing drove every product decision made during the build.

---

## Product Architecture

A 4-layer system, each feeding into the next:

| Layer | Name | What It Does |
|---|---|---|
| 1 | Landing Platform | Property discovery and entry point |
| 2 | ROI Wizard | Yield, IRR, and cashflow analysis |
| 3 | Prop Pulse | Area trends, price benchmarks, supply pipeline |
| 4 | 3D World Viewer | Photorealistic Gaussian Splat exploration with spatial annotations |

**Core flow:** Discover → Analyze → Experience → Share

The deliberate design choice: every tool feeds into the 3D viewer, not away from it. The 3D experience is the convergence point, not a standalone feature.

---

## Where AI Does the Work

AI isn't a layer on top — it's embedded in the decision loop:

- **ROI Wizard:** AI surfaces investment viability signals (yield, IRR, cashflow) from raw listing data, reducing manual calculation to near-zero
- **Spatial Annotations:** AI tags 3D environments with contextual markers — ROI projections, finish quality, view premiums — directly in the space
- **WhatsApp-ready tours:** AI generates 30-second cinematic clips from the 3D model, optimized for broker-to-investor sharing on mobile

**The AI bet:** In remote transactions, trust is visual. The highest-leverage use of AI here is not search or recommendations — it's generating *shareable conviction*.

---

## Key Product Decisions Made

**1. Shareability as a core primitive, not an afterthought**  
Every output — ROI report, market snapshot, 3D tour — was designed to be one-tap shareable via WhatsApp. This was a deliberate constraint that shaped the entire UX. Brokers don't use portals; they use messaging. We built for that reality.

**2. Workflow over features**  
Early temptation was to build each tool standalone. The PM decision was to force a connected flow — you can't skip straight to 3D without seeing the data. This raised friction slightly but dramatically improved decision quality per session.

**3. Speed over perfection on the AI outputs**  
The 3D generation from imperfect photos was the hardest technical constraint. Rather than block on perfect quality, we optimized for "good enough to trust" — conviction in minutes, not days. A deliberate tradeoff to ship within hackathon constraints.

---

## AI-Specific Challenges

The problems that don't show up in a product brief:

- **Input quality vs. output trust:** Gaussian Splat quality degrades with sparse or low-quality photo sets. Users can't trust a tour that looks wrong. Defining the minimum viable photo set for acceptable output was a real design problem.
- **Annotation accuracy:** Spatial ROI annotations are only as good as the underlying data. If Prop Pulse data is stale, the annotation misleads. Surfacing data freshness signals in-product was a late but critical addition.
- **Broker adoption friction:** Generating the cinematic clip works — getting brokers to change the habit of manual WhatsApp pitches doesn't happen automatically. Distribution design is as hard as the feature itself.

---

## What Success Looks Like

Not vanity metrics. Decision quality metrics:

- **Tool → 3D conversion rate** — are users reaching the core product?
- **Video shares per session** — is the distribution primitive actually being used?
- **Session depth** — are users exploring beyond the first property?

---

## Business Model

- SaaS subscription for brokers (access to Prop Pulse + ROI Wizard)
- Per-property 3D scan fees (supply-side monetization)
- Enterprise / white-label for developers and agencies

---

## Roadmap Thinking (Post-Hackathon)

If this were to move beyond hackathon scope, the next bets:

1. **AI-generated staging** — let investors visualize furnished vs. empty scenarios
2. **Automated annotations at scale** — move from manual tagging to model-driven spatial markup
3. **Portfolio-level analytics** — an investor with 3 properties needs a different view than a first-time buyer
4. **Off-plan / developer support** — the largest inventory segment in Dubai, currently underserved by 3D

---

## Positioning

| Platform | What It Does |
|---|---|
| Matterport | 3D tours (no data, no AI layer) |
| Property Finder | Listings (no 3D, no decision support) |
| Luma AI | 3D generation (no real estate context) |

**This product sits at the intersection of all three** — and is the only one where the output is a *decision*, not just a *view*.

---

## Why Now

- Dubai real estate market at record transaction volume
- Remote investing is now the default, not the exception
- 3D + AI infrastructure is finally viable at hackathon-level cost and speed

---

## About

Built to explore how AI, spatial computing, and product design can reshape the real estate decision loop — from fragmented data and static images to a shareable, AI-augmented experience that closes deals remotely.

This was a hackathon build. The thinking behind it is production-grade.
