# Project Proposal  
## Exploring the Drivers of U.S. Domestic Airline Fares

### 1. Background and Objective

Airfares vary substantially across routes, carriers, itinerary structures, and purchase conditions. This project investigates which observable ticket and itinerary characteristics are most useful in explaining cross-sectional variation in U.S. domestic airline fares.

The analysis is exploratory and observational rather than causal. The objective is to build an interpretable multiple linear regression model that identifies the main characteristics associated with higher or lower ticket prices within a common market period.

### 2. Research Question

**Which observable itinerary, carrier, purchase, and geographic characteristics are most useful in explaining variation in U.S. domestic airline ticket fares?**

The analysis also considers whether the resulting model can provide a useful benchmark for identifying relatively high- or low-priced tickets after controlling for observable characteristics.

### 3. Data

The project uses the U.S. Bureau of Transportation Statistics DB1C Ticket dataset for **June 2026**.

The full Ticket file contains approximately **13.8 million itinerary-level observations**. Because the raw dataset is very large, a reproducible random sample of 100,000 observations is drawn from the full file using a fixed random seed.

Data-quality screening identifies tax-only tickets, nominal-fare records, and observations with missing fare information. After these records are excluded, the main analysis sample contains approximately **90,000 observations**.

Using a single reporting month allows the analysis to focus on cross-sectional differences across tickets rather than changes in general market conditions over time.

### 4. Variables

The dependent variable is:

\[
\text{TotalAmount}
\]

which represents the total ticket amount.

The initial set of explanatory variables will include:

- `TotalDistance` — total itinerary distance;
- `Coupons` — number of flight coupons or itinerary segments;
- `RoundTrip` — round-trip indicator;
- `OnLine` — itinerary carrier-structure indicator;
- `PurchaseWindowGroup` — advance-purchase category;
- `RpCarrier` — reporting carrier;
- `ItinGeoType` — itinerary geographic classification;
- `OriginState` — origin-state control.

Additional airport and city-market variables are retained for exploratory analysis but will not be included automatically in the initial regression because of their high number of categories.

Variables mechanically related to the dependent variable, such as `TaxAmount` and the derived non-tax fare component, will not be used as explanatory variables.

### 5. Methodology

The analysis will follow an exploratory multiple-regression model-building process.

A broad initial model will first be estimated using the economically sensible candidate predictors. The model will then be evaluated for:

- multicollinearity;
- statistical contribution of individual and grouped predictors;
- residual behaviour;
- heteroskedasticity;
- influential observations and leverage;
- overall model fit and interpretability.

Variable reduction will be guided by statistical evidence, nested-model comparisons, and practical interpretation rather than by mechanical elimination based only on individual p-values.

Where diagnostics identify material problems, appropriate model refinements will be considered. A separate validation sample will be used to assess whether the final model performs consistently outside the model-building sample.

### 6. Expected Output

The project aims to produce an interpretable regression model describing the main observable characteristics associated with airline ticket fares.

The final model will be evaluated not only by goodness of fit, but also by statistical diagnostics, stability, and practical interpretability.

The results may also be used to construct a simple fare-benchmarking framework, where actual ticket prices are compared with model-implied prices for tickets with similar observable characteristics.

### 7. Scope and Limitations

The study is observational, so estimated coefficients should be interpreted as associations rather than causal effects.

The analysis is also restricted to one reporting month. It therefore explains cross-sectional fare variation within June 2026 rather than longer-term changes in airfare levels.

Unobserved factors such as real-time seat availability, detailed fare restrictions, loyalty status, and competitive conditions may also affect ticket prices but are not fully captured in the available dataset.

### 8. Notebook Structure and Responsibilities

The analysis will be organised into separate notebooks so that data preparation, modelling, diagnostics, and business interpretation remain clearly separated.

| Notebook | Main Responsibility |
|---|---|
| `01_data_preparation.ipynb` | Raw-data inspection, reproducible sampling, fare-quality screening, cleaning, and construction of the prepared analysis dataset. |
| `02_exploratory_analysis.ipynb` | Descriptive statistics, distributions, relationships, and exploratory comparisons across fare, itinerary, carrier, and geographic variables. |
| `03_initial_broad_model.ipynb` | Estimate the initial multiple-regression model using the broad set of economically sensible candidate predictors and assess initial multicollinearity. |
| `04_variable_reduction.ipynb` | Reduce the model using statistical evidence, partial F-tests, individual t-tests, model comparison measures, and interpretability. |
| `05_model_diagnostics.ipynb` | Diagnose residual behaviour, heteroskedasticity, unusual observations, leverage, and influential observations. |
| `06_model_refinement.ipynb` | Apply appropriate remedies only where diagnostics indicate a material problem and construct refined candidate models. |
| `07_model_validation_and_comparison.ipynb` | Compare candidate models and evaluate out-of-sample performance using the validation sample. |
| `08_business_application.ipynb` | Interpret the final model economically and develop the fare-benchmarking application. |

The notebooks are intentionally separated to avoid overlap. In particular, variable selection is not performed during exploratory analysis, diagnostics identify problems rather than immediately fixing them, and model refinement is based only on issues established during the diagnostic stage.