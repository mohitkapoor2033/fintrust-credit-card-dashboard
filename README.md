# FinTrust Bank Credit Card Dashboard

An Excel dashboard and written business review on a credit card portfolio of 3,500 cards, 2,500 customers and 22,000 transactions. It was built for the Advanced Excel Workshop in the MBA (Business Analytics) programme at the Delhi School of Economics, University of Delhi, as a group project of five. I led the data modeling and the dashboard build.

The case is set at FinTrust Bank, where the Head of Cards and Payments asked seven questions: where card spend comes from, which customer groups and card types spend most, which merchant categories need attention, how many cards are close to their credit limit, whether customers repay properly, and what the bank should do next. The business review answers each one in writing, and the dashboard is the interactive version of the same analysis.

## Key results

| Measure | Value |
|---|---|
| Net spend (approved purchases minus reversals) | ₹10.33 Cr |
| Approved purchases | 21,010, averaging ₹4,932 |
| Share of spend from the Mass segment | 51.5% |
| Share of spend from Food Delivery and Transport | 24.7% |
| Statements paid in full | 1,054 of 8,000 (13.2%) |
| Unpaid balance | ₹2.96 Cr (12.3% of ₹24.10 Cr billed) |
| Repayment ratio (paid ÷ billed) | 87.7% |
| Statements paid late | 28.9% |
| Cards at 80%+ of limit on the latest statement | 1,281 (36.6%), of which 707 are over the limit |

Mass leads on spend because it holds about half of all cards, not because each card spends more. Spend per card is close across segments (₹28,664 to ₹30,246). By card type, Cashback is strongest at ₹31,550 per card and Business weakest at ₹27,333.

Online Retail (+14.0%), Entertainment (+12.9%), Electronics (+12.7%) and Fashion (+12.3%) grew fastest in 2026 against 2025, and Fashion and Online Retail also have the highest decline rates. In total, 880 declined attempts worth ₹22.7 lakh were lost spend.

The 1,281 high-usage cards produce 36.6% of net spend, the same as their share of cards, so they carry higher balances without spending more. Affluent (39.7%) and Premium (38.4%) have a larger share of cards at 80%+ than Mass (34.3%).

On repayment, every statement met the minimum payment but only 13.2% were paid in full. Part-payment is the main pressure and late payment is secondary. Both are spread evenly across segments and card types.

## Recommendations

1. Move part-payers towards full payment with full-amount auto-pay and reminders 5 days and 1 day before the due date. Target: unpaid share down from 12.3% to 10% or lower.
2. Follow up early on customers who are 31+ days late or over their limit (162 statements and 707 cards today).
3. Pause limit increases and spend offers on the 1,281 cards at 80%+ of limit, and offer instalment conversion instead.
4. Aim spend offers at the 1,675 cards under 60% of limit, in Food Delivery and Transport.
5. Push the fast-growing categories and review decline rules for Fashion and Online Retail. Recovering half of the declined value would add about ₹11 lakh.
6. Lift spend per card in Affluent, Premium and Business with category-based rewards, aiming to close the gap to Cashback.

## Data model

The model is a fact constellation (galaxy) schema with one snowflaked branch. Transactions and Payments are the two fact tables and they share the Cards table as a common dimension. Customers sit one level further out, linked to Cards rather than to the facts directly. Merchants and Calendar are plain star-style dimensions on Transactions. Every relationship is one-to-many.

```
Customers
    Cards
        Transactions    (also linked to Merchants and Calendar)
        Payments
```

| One side | Many side | Join key |
|---|---|---|
| Customers | Cards | Customer_ID |
| Cards | Transactions | Card_ID |
| Cards | Payments | Card_ID |
| Merchants | Transactions | Merchant_ID |
| Calendar | Transactions | Date to Transaction_Date |

The tables hold 2,500 customers, 3,500 cards, 400 merchants, 22,000 transactions, 8,000 statements and 730 calendar days. Payments carries its own statement and due dates and is not linked to Calendar.

The keys were prepared in Power Query so that the relationships match exactly. Card and merchant IDs arrived combined in one Transactions column and were split into two. The card ID in Payments was split out of a reference column in the same way, and customer location was split into city and state. All IDs were trimmed and upper-cased, date serial numbers were converted to dates, and duplicates were removed so that the one side of each relationship is unique.

Two calculated columns sit in Payments: a lateness bucket built from days late, and a utilisation band built from the statement balance divided by the card's credit limit, which relies on the link from Payments to Cards.

Four DAX measures drive the dashboard: Net Spend, High Util Cards, Late Payment Rate and Repayment Ratio.

## Dashboard

Ten PivotTables on a calculation sheet feed nine charts and four KPI tiles. Three slicers (Year, Card Type, Customer Segment) filter them. Four action cards at the bottom summarise the findings and recommendations as fixed full-portfolio figures that do not respond to the slicers.

Definitions used throughout: net spend is approved purchases minus reversals, with declines counted as zero. Utilisation is the latest statement balance divided by the credit limit, and high usage means 80% or more. Lateness bands are on time, 1 to 7 days, 8 to 30 days and 31+ days. Charts show rupees in millions, where 10 M equals 1 Cr.

Tools: Excel, Power Query, Power Pivot, DAX, PivotTables and slicers.

## Files and how to open

`FinTrust_CC_Dashboard.xlsx` is the dashboard with its data model, and `FinTrust_CC_Credit_Card_Business_Review.pdf` is the written report. Open the workbook in desktop Excel (2016 or later, or Microsoft 365) and go to the Dashboard sheet. 

## Limitations

Spend is low in early 2025 because all cards were issued between January and March, and December 2026 is a partial month because the data ends on 14 December. Year-on-year growth is inflated by that roll-out, so it is used to compare categories and not as an absolute rate.
