title: Kimball Dimensional Modeling Techniques applied to Inventory Sample`
date: 2019-06-17 20:28:10
tags:
- Datawarehouse
- Kimball
---

# Value Chain Introduction

For value chain, here introduces 3 models.

## Inventory Periodic Model

* Scenario: a grocery with 60,000 products * 100 stores, with daily periodic model, there would be 60k*100=6millon records per day.
* Estimation
    *  14byte per row * 6million =84mb per day ; 3 years will be 84 * 1095day=91G data
    *  or 60days of daily and archive old data to weekly snapshot;
* Semi-Additive Facts
    *  Pay attention to the use of " SQL AVG" when do summarize
* Enhanced Inventory Facts
    * Adding more column to fact table including quantity on hand, quantity sold,
       * quantity sold daily / quantity at hand daily = number of turns
       * quantity sold whole year / average quantity at hand daily = number of turns for a year
       * Estimate number of days' supply = current quantity at hand / average quantity sold per day
    * Adding inventory at cost and inventory value at latest selling price

## Inventory Transactions model
P117
