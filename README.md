# Coupon Discount Cap Analysis

SQL + Power BI analysis of how coupons affect profitability in a 1,00,000-order quick-commerce dataset.


## Problem
Coupons bring in orders, but do they bring in profit? This project measures contribution per order and tests whether a discount cap would help.

## Key findings
- Orders without a coupon earned about ₹12 lakh in contribution. The 34,978 orders (35%) with a coupon lost about ₹8.4 lakh. Net contribution: ₹3.56 lakh.
- The loss grows with basket size: a coupon on a ₹700+ cart loses about ₹79 per order, because the discount grows faster than the margin.
- Coupons did not buy loyalty: customers whose first order had a coupon repeated at 96.8%, versus 96.6% without.
- Orders delivered in 30+ minutes lose about ₹11.6 each, driven by rider cost and refunds, not discounts.

## Recommendation
Cap each discount at 0.5x the product margin. In simulation, total contribution rises from ₹3.56 lakh to ₹12.83 lakh (+₹9.27 lakh). This is an upper bound that assumes order volume does not drop, so it should be validated with an A/B test.

## Method
**Contribution per order** = product margin − discount + delivery fee + handling fee − rider cost − packaging cost − refund

- **PostgreSQL:** CTEs, joins, DISTINCT ON, FILTER aggregates, CASE bucketing (cart size, delivery time), cap scenarios
- **Power BI + DAX:** calculated columns, measures, SUMX, SWITCH, and a slicer-driven cap simulator

## Assumptions and limitations
- Delivery and handling fees are treated as revenue (this reconciles to the ₹3.56 lakh total).
- The dataset appears synthetic (about 5 orders per customer, 96%+ repeat), so the retention finding needs real data to confirm.
- Results show association, not causation. How coupons were assigned is unknown.

## Files
- `analysis.sql`: all queries
- `Cap Ana.pbix`: interactive dashboard
- `orders.csv`: dataset
