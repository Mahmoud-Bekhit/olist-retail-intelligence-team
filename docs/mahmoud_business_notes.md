# Business Understanding — Mahmoud

**Project:** Olist Retail Intelligence  
**Author:** Mahmoud  
**Collaborator / reviewer:** Sondos  
**Phase:** 01 — Business Understanding  
**Status:** Mahmoud's draft completed; awaiting Sondos's independent analysis and team review  
**Tools planned:** GitHub and Google Colab

> **Context:** This is a proposed business case for a simulated e-commerce marketplace. The ideas below are based on business reasoning, **not findings from analyzing the Olist data**.

## A. Business objectives — my priorities

### Priority 1 — Reduce negative customer reviews

I believe the company should focus first on understanding negative customer reviews. Customers are central to the marketplace's long-term business performance: repeated poor experiences may affect trust, retention, and future purchases. Our goal is not just to improve a rating on a dashboard, but to identify practical improvements in customer experience while considering business impact.

**Selected business objective:**

> Identify the key factors associated with negative customer reviews and recommend data-driven improvements to customer experience, taking costs and operational feasibility into account.

This is an **investigation and recommendation objective**. We cannot promise that negative reviews will decrease before testing interventions.

### Priority 2 — Product-related issues

Investigate whether customers are unhappy because of issues related to product quality, product availability, or receiving an incorrect item. Authenticity and actual inventory availability are important business concerns, **but cannot be directly verified with the available Olist dataset**.

### Priority 3 — Shipping and delivery issues

Investigate delivery delays and their possible relationship with low reviews. Improvements in shipping may be operationally easier to coordinate than changes across a large number of products and sellers, **but this is an assumption to validate**, not a confirmed fact about Olist's carrier network.

**My decision rationale:** Shipping matters, but it should be treated as a potential driver of customer experience rather than the sole goal. We should not spend the entire budget on shipping before comparing impact, cost, and feasibility across different issues.

## B. Stakeholders and decisions

| Stakeholder | Decisions our analysis could inform |
|---|---|
| Customer Experience / Support | Which complaints to investigate first; how to prioritize follow-up |
| Operations / Logistics | Which delivery stages, regions, or seller patterns deserve further investigation |
| Marketplace / Seller Management | Which sellers or product categories have unusually high negative-review rates, after accounting for volume |
| Business / Finance Management | Which proposed improvements justify resources, while monitoring commercial guardrails |

## C. Business questions — ranked

1. **Which patterns are most strongly associated with negative customer reviews?**  
   This is the main question because the project focuses on customer experience.
2. **Do late orders have a higher negative-review rate than on-time orders, and how many negative reviews come from each group?**  
   We should compare both *risk (rate)* and *volume (count)*, not just one of them.
3. **Are negative reviews concentrated among particular sellers or product categories?**  
   This could help identify targeted investigations rather than treating all products alike.
4. **What issues do customers describe in written negative reviews, where text is available?**  
   Comments may signal problems such as damaged products or wrong items; they are not independent verification of what happened.
5. **How do order volume and average order value move over the same periods as negative-review rates?**  
   We need to watch commercial health while studying customer satisfaction.
6. **How does the share of orders that receive reviews change over time?**  
   A changing review-response mix might distort the apparent trend in negative reviews.

**Why questions 1–3 first?** They directly support finding potential drivers and prioritizing the next investigation. Questions 4–6 add diagnostic evidence and safeguards.

## D. KPI definitions — my proposed three KPIs

| KPI | Type | Initial definition | Grain / timing | Purpose and caveats |
|---|---|---|---|---|
| **Negative Review Rate** | **Primary** | `(Number of rated orders with a review score of 1 or 2) / (Number of rated orders with a valid review score from 1 to 5) × 100` | Order-level, reported by purchase month provisionally | Measures the share of poor ratings among **rated** orders; not among all orders. Multiple reviews per order and the appropriate time anchor need validation. |
| **Number of Orders** | Guardrail | `COUNT(DISTINCT order_id)` | Order-level, grouped by purchase month | Tracks order volume, **not quantity of products sold**. We should report order statuses or agree which statuses to exclude after auditing the data. |
| **Average Order Value (AOV)** | Guardrail | `Sum of order-item product prices for eligible orders / Number of those eligible distinct orders` | Order-level average, by purchase month | Tracks the average **listed product value** per order, excluding freight; this is not net revenue or profit. The exact eligible-order filter must match the numerator and denominator and be agreed after data audit. |

**Important:** Comparing months fairly will require consistent period definitions, status filters, and review coverage checks. These formulas are **proposals**, not yet finalized team metric specifications.

**Open KPI question for the team:** Should we add a fourth official metric (for example, review coverage or late-delivery rate) after Sondos's review?

## E. Business reasoning exercise — illustrative only

Our training example used **100 hypothetical orders**:

| Group | Orders | Negative reviews | Negative-review rate |
|---|---:|---:|---:|
| Delivered late | 10 | 6 | 60% |
| Delivered on time | 90 | 18 | 20% |
| **Total** | **100** | **24** | **24%** |

**What I learned:** The *rate* of negative reviews is higher among late deliveries, while the *number* of negative reviews is higher among on-time deliveries (18 versus 6). Looking only at the rates or only at the counts can be misleading. I would investigate shipping and product/customer-experience issues in parallel before choosing interventions.

These figures are **not results from Olist**. Even in this scenario, we cannot say the 6 negative ratings were *caused* by late delivery or that the other 18 were *caused* by product problems.

## F. Hypotheses — NOT YET TESTED

- **H1:** Orders delivered after their estimated delivery date are associated with a higher rate of 1–2 star reviews than orders delivered on time.
- **H2:** Negative-review rates differ meaningfully across sellers or product categories, even after considering their order volumes.
- **H3 (exploratory):** Some negative written reviews for on-time orders mention product-related issues rather than delivery delays.

These hypotheses require investigation; no findings or causal conclusions have been established.

## G. Data needs and limitations

**Potentially relevant Olist tables:**
- `olist_orders_dataset.csv`: order IDs, purchase timestamps, delivery timestamps and status.
- `olist_order_reviews_dataset.csv`: review scores and optional comments.
- `olist_order_items_dataset.csv`: item-level prices, products, sellers and freight values.
- `olist_products_dataset.csv`: product/category attributes.
- `olist_sellers_dataset.csv`: seller attributes and locations.
- `olist_customers_dataset.csv`: anonymized customer identifiers and locations.

**Limitations / what we cannot claim from these files alone:**
- We cannot directly verify whether a delivered item was genuine, damaged or the wrong product, nor whether a product was actually in stock.
- We cannot calculate Olist's actual **profit**, because the necessary operating costs, seller acquisition costs, commissions and other accounting data are not provided in full.
- `order_items.price` and `payment_value` are different concepts; neither automatically equals net company revenue or profit.
- Negative review scores do not prove the cause of a bad experience. Written comments can help generate evidence, but they may be incomplete.
- The Olist data is historical. Any change proposed for a real company would require further validation and possibly a controlled test.
- We cannot assume a specific number of carriers or compare carriers by name without carrier-identification data.

## H. What success would look like

The output should give management:

1. A credible explanation of *where* negative reviews are concentrated and which factors are **associated** with them.
2. A prioritized set of practical recommendations, with feasibility and expected business trade-offs clearly stated.
3. A KPI dashboard that tracks the negative-review rate together with order volume and AOV, with limitations explained.
4. A realistic plan for testing interventions and evaluating whether they **caused** improvements rather than merely coinciding with them.

A lower negative-review rate alone is **not** enough to declare success if important business guardrails deteriorate. Conversely, lower profits during the same period do not by themselves prove that a customer-experience initiative caused the decline. We would need additional financial data and causal evidence.

## I. Open questions for Sondos / Tech Lead review

- Does Sondos agree that reducing negative reviews is the best initial objective, or does she prefer another objective?
- Which seller/product and shipping investigations should be prioritized after the data audit?
- How should we handle missing reviews and orders with more than one review record?
- Which order statuses and time definitions should be used in the official KPI calculation?
- Should the team add review coverage or late-delivery rate as a fourth KPI?
- What additional data would be required to evaluate profit impact and implementation costs?

---

**Next step:** Sondos completes her independent `docs/sondos_business_notes.md`. Both team members review the proposals, then agree on `docs/01_business_understanding.md` through a pull request.
