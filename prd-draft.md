# PART IV — PRODUCT SPECS SAMPLE: Operator Analytics Dashboard

## Metadata

| Field | Value |
|---|---|
| **Feature Name** | Operator Analytics Dashboard — White-Label Admin Panel Module |
| **Spec Version** | v0.1 — Draft |
| **Status** | 🟡 In Discovery |
| **Author** | PM — Partnerships Squad |
| **Date** | 2026-03-22 |
| **Squad** | Partnerships |
| **Replaces** | Legacy analytics page (bespoke admin panel) |
| **Related Spec** | White-Label Admin Panel — Branding & Configuration Layer *(separate spec)* |
| **Confluence Label** | `white-label` `partner-portal` `b2b` `b2g` `analytics` |

---

## Stakeholders

| Role | Name/Team | Involvement |
|---|---|---|
| **Lead Interim PM** | Ricardo Gomes-Lage | Approver |
| **PM** | Oguzhan Gok | Owner |
| **Engineering** | Partnerships Squad | Builder |
| **Sales** | Sales Team | Requester / GTM |
| **Account Management** | AM Team | Client liaison |
| **Operations** | Ops Team | Edge cases / support |
| **Operator Client** | EcoCorp / Grand Ouest / RhoneMob / future operators | External validator |

---

## Problem Statement

Operator admins currently have a degraded analytics experience: the existing page is missing key metrics, the export function is unreliable, and there is no segmentation capability. As a result, operators depend on the AM team for basic reporting, which is not scalable as the partner base grows.

The deeper issue is architectural: the existing page was built for one client in a bespoke context and cannot be repurposed for the white-label model. Each new operator would require a separate implementation — a one-off-build trap that kills velocity and creates indefinite tech debt.

**Why now:** The 6–9 month pipeline window requires an on-the-shelf analytics capability inside the new admin panel. A standard, configurable dashboard module unblocks sales, fixes existing operator pain points, and eliminates manual reporting overhead from AM and Ops.

---

## Strategic Goal

Build the analytics dashboard module for the new white-label admin panel, giving operator admins a self-serve way to monitor their carpooling program. A basic analytics page exists in the legacy bespoke panel — it has thin KPI coverage, a broken export, no filtering, and cannot scale across multiple white-label operators. This spec defines what the rebuilt module must do.

**Linked OKR (hypothetical):**
- Objective: Win and retain large-scale B2B/B2G partners
- Key Result: Enable 3 must-win deals (EcoCorp, Grand Ouest, RhoneMob) within 6–9 months

---

## RICE Score

| Feature | Reach | Impact | Confidence | Effort | RICE Score |
|---|---|---|---|---|---|
| Operator dashboard (analytics module) | 76 | 2 | 80% | 3 | **40.5** |

---

## Scope of This Spec

This spec covers **the analytics dashboard module only**. It defines:
- What KPIs and metrics are displayed
- How filtering, segmentation, and export work
- Acceptance criteria for dashboard behavior

**Out of scope for this spec (covered separately):**
- Admin panel shell, navigation, and layout
- Branding and white-label configuration (logo, colors, fonts, domain) → *see White-Label Admin Panel — Branding & Configuration Layer spec*
- Subsidy rules, campaign management, and other admin panel modules

---

## Personas

**Operator Admin (B2B/B2G)**
Mobility manager, HR lead, sustainability lead, or transport program owner. Their goal is to understand program performance and prove value internally. Pain point: current reporting is fragmented, manual, and insufficient for monthly steering committees and contract renewals.

> End users (commuters) have no direct interaction with this module in v1. Better operator decisions will indirectly improve commuter experience over time.

---

## User Stories

- As an **Operator Admin**, I want to see key KPIs for my carpooling program in one place so I can quickly assess whether the service is performing.
- As an **Operator Admin**, I want to filter analytics by date range, geography, and site so I can analyze specific business units or territories.
- As an **Operator Admin**, I want to reliably export dashboard data so I can share results with internal stakeholders and procurement bodies.
- As an **Operator Admin**, I want to monitor subsidy usage and budget consumption so I can control spend and adjust policy when needed.
- As an **Operator Admin**, I want to identify weak adoption zones and low-conversion funnel steps so I can take corrective action.

---

## Functional Requirements

### Must Have

1. **KPI overview module** — Displays a standard set of top-level KPIs for the selected operator and time range.
2. **Organized analytics sections** — Covers adoption, ride activity, matching/funnel, subsidy/budget, and impact.
3. **Filtering and segmentation** — Filter by date range and operator-relevant dimensions (site, territory, population segment where available).
4. **Reliable data export** — Export dashboard data to CSV. Export must accurately reflect the current filter scope and contain only authorized operator data. *(Fixes known bug in legacy page.)*
5. **Role-based data isolation** — Dashboard only exposes data for the authorized operator perimeter. No cross-operator data leakage.
6. **Metric tooltips** — Each KPI includes a plain-language definition so operators interpret metrics without AM support.

### Should Have

1. **Period-over-period comparison** — Change vs. previous period shown for key KPIs.
2. **Saved views / presets** — Admins can save common filter combinations for recurring reviews.
3. **Anomaly flags** — Highlights abnormal drops, low conversion, or overspending risk.

### Could Have

1. **Benchmarking layer** — Anonymized comparison against similar operators or historical cohorts. [Hypothesis]

### Won't Have in v1

- Custom dashboard per client
- Free-form report builder
- Real-time live monitoring
- Predictive analytics / forecasting
- End-user personal analytics in the commuter app
- External data source upload by operators
- Admin panel branding and configuration *(separate spec)*

> ⚠️ **Scalability rule:** Every metric and filter must be modeled as a reusable configuration. Operator-specific metrics are only supported if they can be built once and applied across all eligible operators — never bespoke engineering.

---

## Acceptance Criteria

- **AC1:** Authorized operator admin opens the analytics dashboard → sees KPI overview scoped to their operator perimeter and selected date range.
- **AC2:** Admin changes filters (date range, site) → all KPIs and charts update consistently to match the selected scope.
- **AC3:** Admin opens subsidy analytics with budget data present → sees allocated, spent, and remaining budget.
- **AC4:** Admin clicks export → file is generated reliably, matches current filter scope, and includes only authorized operator data.
- **AC5:** Admin hovers/taps a KPI → tooltip explains the metric in plain language with high-level calculation logic.
- **AC6:** User without access to a specific operator perimeter attempts to view it → platform blocks access.

---

## Dashboard KPI Set (v1)

The dashboard answers five questions for every operator admin:

1. **Are people joining?** → Adoption
2. **Are they actually carpooling?** → Activity
3. **Is the product converting demand into rides?** → Funnel
4. **Are incentives working efficiently?** → Subsidy
5. **Is the program creating tangible value?** → Impact

### 1) Adoption
- Registered users
- New registrations (rolling 30 days)
- Activated users
- Monthly active users
- Activation rate

### 2) Ride Activity
- Completed rides
- Ride requests posted / Ride offers posted
- Unique riders and drivers active
- Average rides per active user
- Cancellation rate

### 3) Matching / Funnel
- Search-to-match rate
- Ride completion rate
- Unmatched demand rate

### 4) Subsidy / Budget
- Total budget allocated
- Subsidy spent
- Budget utilization rate
- Average subsidy per completed ride
- Cost per completed ride

### 5) Impact
- Estimated CO₂ savings
- Top active corridors / zones
- Coverage by site / geography

### System Integrity
- Data freshness status *(flags stale data; visible to operator admin)*

---

## Product Success Metrics

How BlaBlaCar Daily measures whether this dashboard module is succeeding:

| Metric | Baseline | Target | Timeframe |
|---|---|---|---|
| Operator admins using dashboard monthly | ~0% (legacy page not trusted) | 70% of eligible admins | 3 months post-launch |
| Manual reporting requests per operator | ~4/month [Hypothesis] | −50% | 3 months |
| Export-related support complaints | Known recurring issue | 0 reported bugs | 1 month post-launch |
| Deals supported/unblocked by analytics capability | 0 | 3 | 6 months |
| Time to prepare operator monthly report | 2–4 hrs [Hypothesis] | −60% | 3 months |
| Renewal/QBR satisfaction with reporting | Unknown | Positive feedback from 80% of pilot operators | 6 months |

### Guardrail Metrics
- Dashboard data discrepancy incidents
- Export failures or incomplete data exports
- Permission / privacy incidents (cross-operator data leakage)
- Internal support tickets caused by unclear KPI definitions
- Dashboard load time [target: <3 seconds for summary view]

---

## Out of Scope (v1)

- Bespoke KPI configuration per client
- Custom formulas for a single operator contract
- Cross-operator benchmarking visible by default
- External BI connector / marketplace
- Predictive demand modeling
- Financial invoicing / billing reconciliation
- End-user personal analytics
- Admin panel branding and configuration *(separate spec)*

---

## Open Questions

| # | Question | Owner | Priority |
|---|---|---|---|
| 1 | Which KPIs from the legacy page are actively used by operators vs. ignored? | AM Team | 🔴 High |
| 2 | Which KPIs are truly universal across B2B and B2G vs. contract-specific? | PM / Sales / AM | 🔴 High |
| 3 | Which data events already exist and which need instrumentation first? | Engineering / Data | 🔴 High |
| 4 | What is the root cause of the export bug — data layer or frontend? | Engineering | 🔴 High |
| 5 | How do we define "active user," "match," and "completed ride" consistently across clients? | PM / Data | 🔴 High |
| 6 | Can subsidy and budget data be surfaced in a reusable way across contract types? | Engineering / Finance / Ops | 🟠 Medium |
| 7 | What level of segmentation is safe without exposing individual-level data? | Legal / Data / Security | 🟠 Medium |
| 8 | Should exports be available to all operator admins or gated by role? | PM / Security | 🟠 Medium |
| 9 | What is the minimum viable dashboard for pilots vs. the full-scope target? | PM / Engineering | 🟠 Medium |

---

## Rollout Approach

1. **Alpha** — Internal prototype within the new admin panel shell using sample operator data; validate KPI definitions with Sales, AM, and Ops. Confirm export reliability before proceeding.
2. **Beta** — Pilot with one lower-complexity operator; collect feedback on usefulness, trust, and metric gaps. Compare directly against legacy page experience.
3. **GA** — Analytics dashboard module available as an on-the-shelf component for all eligible operators on the new white-label admin panel, with a Confluence KPI glossary.

---

*v0.1 — Living document. All [Hypothesis] items must be validated in discovery before engineering kickoff.*
