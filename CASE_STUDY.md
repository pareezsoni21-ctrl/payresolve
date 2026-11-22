# PayResolve | Product case study and PRD v2
**Independent portfolio project | 23 November 2026**

## 1. Problem discovery and evidence
NPCI reported 25,466.70 million UPI transactions, 758 banks live and INR 31,52,929.46 crore transaction value in October 2026 [1]. The NPCI volumes do not establish PayResolve demand or the incidence of covered failures. RBI's 2020 ODR circular explicitly calls for system-driven dispute handling [3]. Its 2019 TAT rules distinguish two failed UPI categories [4].

Problem hypothesis: an analyst manually investigating a failed transaction may struggle to establish the correct deadline, prioritise work and identify cases requiring escalation. Late reversals can create customer uncertainty and compensation exposure. No direct interviews or production workflow observations yet. The product aims at analyst triage, not accelerating bank-controlled reversal itself.

Jobs to be done: When reviewing a queue of failed UPI transactions, I want to know which cases need attention now, why, and what the next safe action is, so I can spend less time checking rules and reduce deadline misses. Sponsor: payment operations leader; user: dispute/support analyst; secondary: risk/compliance reviewer.

## 2. Competitors and positioning
- PhonePe: in-app grievance and escalation processes [5]. Google Pay: payment and merchant support [6].
- NPCI MyUPI: cross-app transaction grievance tracking and AI support [7]. A generic consumer chatbot would overlap with it.
- Razorpay: payment, refund and dispute dashboard workflows [8,9].
- Proposed wedge: explainable, status-prioritised monitoring of covered failed-payment cases using a common calendar-day rules engine. Do not claim that competitors lack this capability, or that the gap has been validated.

## 3. Product strategy and business case
Initial segment: small/mid-sized payment service providers with support and reconciliation teams. Buyer: head of operations. Adoption hypothesis: a read-only exception queue can fit alongside existing systems rather than replace them. Proposed GTM: interview five analysts/leads, demonstrate sample workflow, seek a sandbox design partner, then evaluate a limited pilot. Subscription by operational case-volume band is a pricing hypothesis, not validated pricing. ROI model: monthly cases × minutes saved per case / 60 × loaded hourly analyst cost, plus separately validated avoided late costs. Do not count the generated demo exposure as savings or market size.

## 4. Prioritisation and MVP
P0: structured case classification, T+1/T+5 deadline, overdue exposure, manual review for missing evidence, queue/detail screen, dashboard, search/filter, CSV export and simulator. P1: audit history, imports, role permissions, analyst feedback, optional text classification with confidence and human review. P2: verified integrations, security and compliance review, broader payment rails. No live refunds or bank API access. MoSCoW selected over fabricated RICE scores because reach/effort inputs lack evidence.

## 5. PRD / acceptance criteria
Inputs: synthetic case ID, failure category, calendar transaction date, resolution outcome and date (concluded reversal or confirmed beneficiary credit for fund transfers), domestic parties, confirmed debit and non-completion, evidence of non-customer attribution, transaction amount. Review date is explicit. Reject invalid or future dates and reversal preceding transaction. For qualifying P2P debit without beneficiary credit, deadline = T+1 calendar day; P2M debit without merchant confirmation = T+5. For an open case, overdue days = max(0, review date minus due date); closed case uses the resolution date. Indicative exposure = overdue days × INR 100. Due date itself is not overdue. Uncovered/unverified cases must return manual review and no amount. No inference of actual liability or entitlement. No automated financial action. Sort overdue before due today, then manual review, then within deadline and closed cases; within status sort by exposure. Dashboard reconciles open exposure separately from closed-case delay. CSV exports filtered rows. Fixed cohort dated 23 November 2026; later dates assume no new events. Unsupported scenarios and transactions before the effective date are routed to review with reasons. Source references identify Annex 4(a/b).

User stories: (1) As an analyst I can inspect a case and see the reason for its deadline and next action. (2) As a lead I can filter overdue cases and export them for review. (3) As a reviewer I can see incomplete evidence flagged without an invented amount. (4) As a product tester I can simulate boundary dates and unsupported cases.

## 6. Metrics and evaluation
Current engineering results: 56/56 policy scenarios and 26/26 additional automated checks passed (82/82 total); 120 synthetic records processed; at review date 2026-11-23, 90 eligible records, 30 manual-review records, 50 open overdue and 5 open due today, INR 21,800 indicative open exposure. These are demo outputs, not real customers or financial outcomes. Automated test pass rate is not real-world model accuracy. The 56 scenarios use separately specified expected values for elapsed days 0–7, open/immediate/same-review-date reversals, and fund-transfer credit. The 26 other checks cover missing evidence, invalid inputs, dates and cohort reconciliation. The labels are developer-authored, not independently certified. See BENCHMARK.csv and TEST_RESULTS.txt.

Pilot primary metric: median analyst triage time from opening case to submitting correct recommended action. Goal 30% reduction vs manual rule lookup (unvalidated). Secondary: correct category/deadline/action rate, eligible case coverage, cases triaged per analyst-hour, overdue cases escalated within internal target, task completion. Guardrails: zero unsupported cases receiving numerical exposure in policy test suite, incorrect escalation rate, missed overdue cases, no unauthorised financial actions. Track actual reversal time separately since it is dependent on external banks.

Evaluation plan: create 20 labeled cases not used in development; have a reviewer derive expected outcomes from primary rules, compare results and log discrepancies. Recruit 3–5 volunteers for a usability test (not representative of analysts unless they are analysts); each completes comparable manual and app tasks in alternating order. Record time, correctness and uncertainty; report sample size and median with caveats. Use USER_RESEARCH.csv for results. Do not add impact metrics to resume before collecting them.

## 7. Discovery script (not yet conducted)
Interview payment users: describe your last failed payment, what status did you see, what did you do next, how long did it take, what information was unclear? Interview analysts: walk me through your last case, where is evidence recorded, how do you identify deadline risk, what requires another team, what mistakes happen, what tools work well? Avoid leading questions. Ask permission, record anonymised notes only, never collect UPI PINs, account data or real transaction identifiers.

## 8. Next steps and limitations
Policy review with a domain specialist; user validation; data import with synthetic schema validation; audit trail; independent test dataset; usability benchmarking; security and regulatory review before any real-world use. Current MVP is a client-side static application using HTML/CSS/JS and generated records. No persistent database, real dispute classification from free text, cloud integration, compliance guarantee or external users. Rules based on cited 2019 circular; validate current applicability before deployment.

## Sources
[1] NPCI UPI statistics: https://www.npci.org.in/product/upi/product-statistics?financialYear=2026-27
[3] RBI Online Dispute Resolution (2020): https://m.rbi.org.in/Commonman/English/Scripts/Notification.aspx?Id=3194
[4] RBI Harmonisation of TAT (20 Sep 2019), Annex item 4: https://www.rbi.org.in/Scripts/BS_CircularIndexDisplay.aspx?Id=11693
[5] PhonePe grievance policy: https://www.phonepe.com/grievance-policy/
[6] Google Pay merchant payment support: https://support.google.com/pay/india/answer/9494510?hl=en-GB
[7] NPCI MyUPI: https://www.npci.org.in/product/myupi.html
[8] Razorpay payments dashboard: https://razorpay.com/docs/payments/payments/dashboard/
[9] Razorpay dispute dashboard: https://razorpay.com/dispute-guide/

## V2 policy notes
The circular applies from 15 October 2019. T is the transaction calendar date; a concluded reversal is measured to receipt by issuer/originator, not initiation. Applicable compensation is proactive, not complaint-dependent. For fund transfers the rules permit credit or reversal; beneficiary-credit delay is measured to confirmed credit. The prototype does not determine the recipient, actual payment of compensation, or liability; merchant credit/confirmation outcomes outside the implemented rules are manually reviewed. Rule citations are from Annex 4(a) and 4(b). No independent legal/compliance review.

## Browser verification
Eight browser smoke-check groups passed using Chromium with embedded copies of the exact application CSS and JavaScript. The test environment blocked file:// navigation, so local-file loading and external links were not exercised there. See BROWSER_TEST_RESULTS.txt.
