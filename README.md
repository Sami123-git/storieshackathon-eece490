# Stories Coffee: Does the same menu fit every branch?

**Stories Coffee Hackathon**  
**Team:** Houssein Karnib · Omar El Hajj · Sami Marouni

## Conclusion

Stories should prioritize a Garden cost audit, measure actual waste before changing the menu, and judge promotions on gross profit. Garden's peer-mean cost difference is **USD 16,628.98 over six months**, a benchmark to investigate rather than guaranteed savings. Thirteen branch–product combinations sell below one unit per day, but branch volume explains much of this pattern and actual waste is unrecorded. Harbour's March promotion is inconclusive; May shows an estimated target-category gross-profit shortfall of **USD 331.76** using pooled controls, with causal uncertainty remaining. Water and espresso contribute positive gross profit and frequently accompany other profitable purchases, while product forecasts should guide small pilots rather than sales commitments. The business value is a focused sequence of audits and tests before wider changes; these figures must not be added together as a total loss or promised saving.

## Overview

This project investigates financial concerns associated with a shared 45-product menu across Harbour, Garden, and Uptown. It examines differences in sales mix, slow-perishable exposure, basket purchases, promotion performance, and recorded costs. It also evaluates product-sales prediction methods and proposes practical tests before operational changes.

The project translates the findings into a cost audit, waste measurement, and controlled pilots. The evidence does not establish that the identical menu causes the financial differences.

## Files and data

| File | Purpose |
|---|---|
| [01_setup_and_section_A.ipynb](01_setup_and_section_A.ipynb) | Project introduction, setup, exploratory analysis, data quality, and branch sales mix |
| [02_section_B.ipynb](02_section_B.ipynb) | Slow perishables, basket purchases, Harbour promotions, and Garden costs |
| [03_section_C.ipynb](03_section_C.ipynb) | Product prediction, pilot sensitivity, proposed tests, limitations, and final conclusion |
| `stories_transactions.csv` | Required original input, including `Customer ID`; supply separately when running |
| `README.md` | Project overview and reproduction instructions |

`stories_transactions.csv` is **not included in the public submission package**. Obtain the original hackathon dataset separately and upload it privately to the Colab runtime. The generated cleaned CSV is also excluded from this package. The notebook retains saved analytical outputs and sample rows; excluding CSV files does not remove those embedded data excerpts.

The original data contains **51,177 transaction lines**, covering **January 1–June 30, 2026**. Removing 132 identical rows under the documented duplicate-entry assumption leaves **51,045 lines**. These represent **33,887 transactions**, three branches, and 45 products. Rows are line items, not individual customer orders.

Required input columns:

```text
Transaction ID, Date, Time, Branch, Order Type, Product, Category,
Quantity, Unit Price (USD), Discount Rate, Unit Cost (USD),
Line Revenue (USD), Line Gross Profit (USD), Customer ID
```

The notebook generates these files during execution; they are not required inputs:

- `stories_transactions_clean.csv`
- `product_tags.csv`
- `branch_characteristic_index.csv`
- `distributions_numeric.png`
- `distributions_categorical.png`

## Reading order and how to run

The submission is separated into three notebooks to make each section easier to review. Read **01 → 02 → 03**. Saved outputs are retained, so reviewing the results does not require running the code.

1. Open the desired numbered notebook in Google Colab.
2. Start a fresh Python runtime and run its cells from top to bottom. A CPU runtime is sufficient.
3. When prompted, upload only the original transaction CSV, including `Customer ID`. The upload cell uses the filename `stories_transactions.csv`.
4. Do not substitute the generated clean CSV: the data-quality setup reloads the original file for its customer-ID checks.
5. In Section B, run its first three prerequisite code cells before the financial analysis. In Section C, run its first two prerequisite code cells before the model. Opening or running Section A in a different Colab notebook does not create variables in B or C.
6. Allow every cell to finish and save refreshed outputs. Section C includes repeated model fitting and may take longer.

### Preserved code and independent runtimes

No original code cell was edited. Original explanations, execution counts, and saved outputs are retained. Only introductory navigation notes were added during splitting.

- **01** contains the original introduction, setup/EDA, and all of Section A.
- **02** repeats the original upload, data-quality/tag setup, and characteristic-index cells verbatim to create `df`, `tags`, and `prod_idx`, followed by the original Section B cells.
- **03** repeats the original upload and data-quality/tag setup cells verbatim to create `df` and `tags`, followed by the original Section C and final conclusion.

These prerequisite copies are intentional; they avoid modifying the analysis code or requiring shared Colab sessions. For grading, the analysis begins at the SECTION B or Section C heading after the prerequisite block. Existing execution numbers reflect the original combined run, not a fresh execution of the split files.

The split was checked to preserve every selected original cell, including its code, metadata, and outputs. The split notebooks have not been freshly executed end to end as separate files; saved outputs come from the supplied combined notebook.

The optional cleaned-CSV download cell remains in **01**, as part of the original workflow. No CSV input is bundled with these notebooks. File generation occurs in the notebook's active Colab runtime.

### Dependencies

The notebooks use Python, pandas, NumPy, Matplotlib, SciPy, scikit-learn, IPython, and Colab's file-upload/download utilities. The `OneHotEncoder(sparse_output=False)` argument requires scikit-learn 1.2 or newer. No GPU, credentials, database connection, or external API is needed.

Local execution requires adapting the Colab-specific upload and download cells. Exact dependency versions are not pinned. The README reports the supplied notebook's saved results; it does not claim that the separate notebooks were re-executed during packaging.

## Notebook structure

### Section A — Data quality and branch characteristics

- Checks missing values, duplicate lines, arithmetic consistency, and price/cost stability.
- Documents manually assigned product characteristics and their limitations.
- Compares product and category shares after adjusting for branch sales volume.
- Explores customer-ID consistency without claiming that the limited checks prove IDs are random.

### Section B — Is the problem real, and what does it cost?

- **B1:** Finds perishables selling below one unit per calendar day, calculates recorded daily gross profit and hypothetical waste break-even thresholds, and compares normalized sales shares.
- **B2:** Checks whether water and espresso appear in baskets containing other profitable products.
- **B3:** Evaluates Harbour's 15% Cold Coffee and Dessert promotions on March 12–19 and May 12–19. Uses 14-day before/after windows, weekday-standardized proportional control comparisons, alternative control branches, and placebo checks.
- **B4:** Compares the same products and months across branches, holding Garden's quantities and revenue fixed to estimate alternative cost benchmarks.

### Section C — How sure are we, and how can Stories test it?

- Compares overall and category geometric-mean baselines, five-neighbour kNN, and ridge regression.
- Holds out one entire product at a time. Ridge tuning and preprocessing use training products only, with inner five-fold cross-validation.
- Estimates approximate pilot sensitivity from 25 complete Monday–Sunday weeks.
- Proposes waste logging, a preparation pilot, a Garden cost audit, promotion tests, and new-item pilots.
- Separates measured results from missing information and assumptions.

## Main findings

| Analysis | Result | Interpretation |
|---|---|---|
| Branch sales mix | Desserts index 126 at Garden and 81 at Uptown; individual sandwiches index 134–151 at Uptown | Index 100 is the chain-wide sales share; availability and promotions may also contribute |
| Slow perishables | 3 at Garden, 10 at Uptown, 0 at Harbour | Low sales indicate exposure, not measured waste or proven losses |
| Water/espresso baskets | 60.6%–67.1% contain another profitable product | Co-purchase association does not establish incremental sales caused by the focal product |
| Promotion break-even | Approximately 28% more combined target-category units required | Holds the pre-promotion product mix and cost assumptions fixed |
| March promotion | Pooled estimated unit uplift: +8.2%; GP range across individual controls: −USD 234.38 to +USD 41.04 | Financial result is inconclusive |
| May promotion | Pooled estimated unit uplift: −6.1%; pooled GP gap: −USD 331.76 | Negative benchmark comparison, not a proven causal loss |
| Garden costs | Approximately 43% above Harbour and 49% above Uptown | Same-product comparisons; the underlying cause requires an audit |
| Garden peer-mean scenario | USD 16,628.98 higher recorded costs over six months | Benchmark difference, not guaranteed achievable savings |
| Uptown soup | 0.414 portions/day; hypothetical waste break-even: 0.642 portions/day | Requires actual disposal records before acting |

The Garden comparison uses the arithmetic mean of Harbour and Uptown unit costs for each product-month. The three cost benchmarks are alternatives, not additive savings.

### Product prediction performance

| Method | Mean absolute percentage error (MAPE) | Log-error-equivalent percentage |
|---|---:|---:|
| Overall geometric mean | 32.9% | 34.6% |
| Category geometric mean | 28.0% | 29.4% |
| Five-neighbour kNN: characteristics | 28.3% | 30.0% |
| Ridge: characteristics + category | 23.6% | 24.9% |

Ridge improves mean absolute log error by approximately 13.7% versus the category baseline, but performs better on only 24 of 45 products. Individual prediction errors can be large.

The log-error-equivalent percentage is `100 × (exp(mean absolute log error) − 1)`. It is distinct from MAPE and is not a confidence interval. The model predicts chain-wide sales over the observed period; it does not select the best branch or validate performance on real launches.

### Pilot sensitivity

With six baseline weeks and six pilot weeks, the approximate calculation detects an increase of **12.2%** or a decrease of **10.9%** in Uptown's GP relative to the comparison branches, under the stated independence and stability assumptions and using 5% two-sided significance and 80% power.

Soup represents only **0.895%** of Uptown's recorded GP. A pilot may therefore miss an effect as small as soup's current contribution. Lack of a statistically detectable change does not prove no effect.

## Alternative explanations examined

| Explanation | Check performed | What the evidence supports |
|---|---|---|
| Slow sales simply reflect a smaller branch | Compared units/day with units per 1,000 branch units and the other branches' product shares | Branch volume explains much of the pattern. Some slow products, such as Uptown's Quinoa Bowl, have strong relative shares. Stockouts remain unobserved. |
| Garden sells a more expensive product mix | Compared the same products and months, using Garden's quantities for every cost benchmark | The premium persists after controlling for product mix; sales mix alone does not explain it. |
| Garden's premium is confined to food spoilage | Examined cost ratios across all 45 products, including bottled water | Similar premiums on water weaken an explanation limited to fresh-food spoilage. Waste, overhead allocations, and accounting rules are not ruled out without cost documentation. |
| Promotion-period changes reflect common growth or weekday patterns | Used local 14-day baselines, weekday standardization, both control branches, and after-period sensitivity checks | Adjustments account for these factors under proportional-trend assumptions, but do not rule out branch-specific trends. |
| Apparent promotion effects occur without treatment | Applied earlier-date and untreated-branch placebo comparisons | Large placebo differences weaken causal claims, especially for March. |
| Model complexity adds no value over a simple category estimate | Evaluated all methods on the same held-out products, with training-only preprocessing and ridge tuning | Ridge improves average error, while kNN does not improve the log-error measure. Improvement is not universal or established for actual launches. |

## Approaches not used and why

These are scope decisions and limitations, not claims that every alternative was formally tested.

| Approach | Reason for not using it |
|---|---|
| Customer loyalty or individual substitution analysis | IDs have missing values and unconfirmed meaning/reliability. Limited shuffle checks do not validate them for customer-level conclusions. |
| Long-horizon time-series ML forecasting | Six months from three branches does not establish full-year seasonality or future operating conditions. The implemented ML task is held-out-product prediction, not future-date forecasting. |
| Recommending existing items as new to another branch | All 45 products already appear at all three branches. Truly new-item pilots require new-product evidence. |
| Recommending branch closure | Recorded gross profit excludes rent, labour, and other operating expenses. It cannot establish net branch profitability or the value of closure. |
| Inferring price elasticity from cross-branch list prices | Price differences coexist with differences in branch volume, sales mix, and costs. They are not randomized price changes; the observed promotions also have comparison limitations. |
| Adding all financial figures into one savings total | Waste scenarios, promotion estimates, and cost benchmarks overlap and answer different questions. |

## Assumptions and limitations

- Identical rows are treated as accidental duplicates; legitimate repeated line items would require different cleaning.
- Product tags are provisional. In particular, `milk` is not a dairy-free indicator, and `perishable` does not establish daily disposal or complete ingredient spoilage exposure.
- Low recorded sales may reflect unavailable stock rather than weak demand.
- Preparation quantities, actual disposals, usable inventory, operating expenses, and campaign costs are unavailable.
- Waste thresholds assume discarded portions carry the recorded unit cost and that additional waste is not already included.
- Garden's constant costs do not establish their accounting basis or whether benchmark differences are avoidable.
- Basket associations do not establish causal cross-selling effects.
- Promotion estimates depend on proportional comparison trends. Large placebo differences weaken causal confidence; after-period comparisons may contain carryover effects.
- The chi-square test assumes independent counts, although units within baskets may be related. Descriptive sales-share differences are emphasized.
- Only 45 products are available for prediction evaluation. Held-out existing products are a proxy for new launches, not a launch experiment.
- Pilot sensitivity is approximate and may change with serial correlation, seasonality, or differing branch trends.

**Do not add waste scenarios, promotion estimates, and cost benchmarks into a total loss or promised saving. Recorded gross profit is not net profit.**

## Proposed next actions

1. Audit Garden's invoices, recipes, pack sizes, transfer prices, and cost allocations.
2. Record actual waste and stock availability before changing preparation.
3. Test a smaller soup batch at Uptown with baseline measurement, comparison branches, and service safeguards.
4. Evaluate future promotions on financial contribution, including relevant costs and effects on other products.
5. Use the model and category baseline as preliminary estimates for small new-item pilots.

The deliverable provides an evidence-based sequence of investigations and pilots for Stories to validate before rollout.

