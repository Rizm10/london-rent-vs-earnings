# London Rent vs Local Earnings

A short data analysis exploring which London boroughs have one-bedroom rents that appear unusually high or low relative to local full-time earnings.

## Question

**Which London boroughs have rents that are unusually high relative to local resident earnings?**

Rather than simply ranking boroughs by rent, this project compares rents with local earnings using two measures:

1. **Rent pressure proxy**  
   Median monthly one-bedroom rent as a proportion of estimated median gross monthly earnings.

2. **Regression residuals**  
   The difference between actual one-bedroom rent and the rent predicted from the London-wide relationship between local earnings and rent.

---

## Data

### Rental data

Source: **Office for National Statistics London rental statistics**

The analysis uses median one-bedroom rents by London borough.

The rental dataset covers approximately **April 2024 to March 2025**.

### Earnings data

Source: **ONS Annual Survey of Hours and Earnings (ASHE) 2025 provisional**

Measure used:

- Full-time workers
- Gross weekly pay
- Median earnings
- Home geography

Weekly earnings were converted into an approximate monthly figure using:

~~~text
Monthly earnings = Median weekly earnings × 52 / 12
~~~

The two datasets were then joined by London borough.

---

## Method

### 1. Data preparation

The rental and earnings datasets were cleaned and standardised before being merged by borough.

There are 33 London local authorities in the source data.

The **City of London** was excluded from calculations requiring median earnings because its median full-time earnings estimate is suppressed in the ASHE dataset.

This left **32 boroughs** for the main analysis.

### 2. Rent pressure proxy

For each borough:

~~~text
Rent pressure =
Median monthly one-bedroom rent
÷
Estimated median gross monthly earnings
~~~

This provides a simple comparison of rent with local gross earnings.

### 3. Regression

A simple linear regression was fitted using:

~~~text
X = Median full-time weekly gross earnings
y = Median monthly one-bedroom rent
~~~

The model estimates the expected rent for a borough based on the overall relationship between earnings and rents across London.

Residuals were then calculated as:

~~~text
Residual = Actual rent - Predicted rent
~~~

A positive residual means rent is higher than the model would expect based on local earnings.

A negative residual means rent is lower than expected.

---

## Results

The regression produced:

- **R²: 0.347**
- **Coefficient: 2.49**
- **Intercept: -£685.96**

Median full-time earnings therefore explain approximately **34.7% of the variation in one-bedroom rents across the 32 boroughs**.

This suggests that earnings are related to rental prices, but a substantial amount of variation is associated with other factors not represented by the model.

The coefficient indicates that, within this dataset, a £1 increase in median weekly earnings is associated with approximately **£2.49 higher median monthly one-bedroom rent**.

This is an association within the cross-sectional data and should not be interpreted as a causal effect.

### Largest positive residuals

| Borough | Approx. residual |
|---|---:|
| Westminster | +£754 |
| Kensington and Chelsea | +£530 |
| Brent | +£475 |
| Hackney | +£294 |
| Hammersmith and Fulham | +£268 |

These boroughs had rents substantially above what the earnings-based regression predicted.

### Largest negative residuals

| Borough | Approx. residual |
|---|---:|
| Bromley | -£419 |
| Richmond upon Thames | -£399 |
| Havering | -£298 |
| Kingston upon Thames | -£291 |
| Bexley | -£281 |

These boroughs had rents below what the model predicted from local earnings.

---

## Rent pressure

The rent pressure proxy compares median monthly one-bedroom rent directly with estimated median gross monthly earnings.

The highest values were:

| Borough | One-bedroom rent as % of estimated gross monthly earnings |
|---|---:|
| Westminster | 59.1% |
| Kensington and Chelsea | 54.0% |
| Brent | 51.4% |
| Hackney | 48.1% |
| Hammersmith and Fulham | 47.0% |

The lowest included:

| Borough | Rent pressure |
|---|---:|
| Bromley | 31.1% |
| Havering | 32.2% |
| Croydon | 32.4% |
| Bexley | 32.4% |
| Sutton | 32.7% |

The rent-pressure ranking broadly supports the regression residual analysis, with several of the same boroughs appearing at the top under both measures.

---

## Notable finding: Brent

Brent was one of the most interesting results.

Its median one-bedroom rent was approximately **£1,750 per month**, while the regression predicted a rent of roughly **£1,275** based on local earnings.

This produced a residual of approximately **+£475**.

Median one-bedroom rent was also equivalent to approximately **51% of estimated median gross monthly earnings**.

Unlike Westminster or Kensington and Chelsea, this result is not simply associated with exceptionally high local earnings, making Brent a particularly strong relative outlier.

---

## Quality checks

Several checks were performed before freezing the analysis.

The final regression dataset contained:

- **32 rows**
- **32 unique boroughs**
- **0 duplicate boroughs**
- **0 missing values** in the variables used for the final analysis

The fitted regression residuals also summed to **0**, as expected for an ordinary least squares linear regression with an intercept.

Rental sample sizes varied between boroughs, with approximately **150 to 890 one-bedroom rental observations per borough**.

---

## Limitations

This project is intended as a comparative analysis rather than a complete measure of housing affordability.

Important limitations include:

- Earnings represent **median full-time gross pay**, not disposable income.
- Household income, taxes, benefits, savings and other living expenses are not included.
- The analysis focuses only on **one-bedroom properties**.
- Household size and shared accommodation are not considered.
- Property quality, transport access, housing supply, neighbourhood characteristics and local demand are not explicitly modelled.
- The regression contains only **one explanatory variable**.
- The analysis is cross-sectional and does not establish causation.
- The rental and earnings datasets are based on observed or sampled data rather than every worker or rental property.
- Some workers, employers, landlords, agents or other data providers may not be represented in the underlying datasets.
- Missing or unavailable observations may therefore affect how representative the borough-level estimates are.
- Sample sizes vary between boroughs. One-bedroom rental counts range from approximately **150 to 890 observations**, meaning the precision of estimates may differ between areas.
- Earnings estimates also represent differing numbers of jobs across boroughs.
- Some ONS estimates may be suppressed when the available sample is too small or statistically unreliable.
- The **City of London** was excluded from earnings-based calculations because its median full-time earnings estimate was suppressed.

The residuals should therefore be interpreted as showing where rents are unusually high or low **relative to the London-wide earnings-rent relationship**, rather than as definitive measures of affordability.

---

## Repository structure

~~~text
london-rent-vs-earnings/
│
├── README.md
├── london_rent_earnings_2025.ipynb
└── london_rent_vs_earnings_final_results.csv
~~~

---

## Tools

- Python
- pandas
- scikit-learn
- matplotlib
- Jupyter / Google Colab

---

## Summary

Local earnings are associated with London rental prices, but earnings alone explain only around **one-third of the variation between boroughs**.

The analysis also highlights substantial differences in how rents compare with earnings locally.

**Westminster, Kensington and Chelsea, Brent, Hackney, and Hammersmith and Fulham** had the largest positive residuals, while **Bromley, Richmond upon Thames, Havering, Kingston upon Thames, and Bexley** had the largest negative residuals.

Among the results, **Brent stands out as one of the strongest relative outliers**, with one-bedroom rent substantially higher than would be expected from its local earnings level.
