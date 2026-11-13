# COVID-19 Reported Burden: Global Progression, Population-Adjusted Patterns and Targeting Evidence (Dec 2019 – Jul 2020)

**Project category:** Public Health Analytics | Exploratory Data Analysis | Data Visualisation

**Tools:** Python, Pandas, NumPy, Matplotlib, Seaborn, Jupyter Notebook

---

## Overview

The COVID-19 pandemic changed healthcare systems, economies and daily life. This project asks what the reported numbers actually show. Using daily confirmed cases, reported deaths and 2019 population estimates for 210 countries and territories, it examines how the pandemic progressed from December 2019 to July 2020, where the reported burden was concentrated, and how the picture changes when counts are adjusted for population.

The central idea is that **absolute counts and population-adjusted rates tell different stories.** A country can report the most deaths and still have a low rate per person. A small country can have a very high rate. Reading both is necessary before drawing conclusions or targeting support.

## Objectives

**Descriptive questions**

1. How did global COVID-19 cases and deaths change over time?
2. Which countries recorded the highest reported cases and deaths?
3. Which countries experienced the highest infection and death rates relative to their populations?
4. How did the reported impact differ across continents?
5. Which months recorded the highest reported cases and deaths?
6. When did individual countries experience their highest daily cases and deaths?
7. How did case fatality ratios differ across countries?

**Stakeholder questions** (for UNDP, UNICEF and international development agencies)

1. Which countries experienced significant reported health burdens?
2. How did the reported burden differ between highly populated and less populated countries?
3. What does the distribution suggest about the need for targeted health interventions?
4. What additional socioeconomic information would be needed to assess vulnerability?

## Stakeholders

- **United Nations Development Programme (UNDP)**
- **UNICEF**
- **International development agencies**

## Business Problem

Development agencies must decide where to direct limited health and recovery support. Totals show where the most cases and deaths were reported, but they favour large countries. Rates show where the burden was heaviest relative to population, but they can reflect differences in testing and reporting. Both views are needed before support is targeted.

## Dataset

- **File:** `covidset.csv`
- **Size:** 26,982 daily records, 11 columns
- **Period:** 31 December 2019 to 1 July 2020
- **Coverage:** 210 countries and territories (209 with population data); 6 continental categories including "Other"
- **Source:** Kaggle

| Column | Description |
|---|---|
| `dateRep` | Date of reporting |
| `day`, `month`, `year` | Date components |
| `cases` | Daily reported cases |
| `deaths` | Daily reported deaths |
| `countriesAndTerritories` | Country or territory |
| `geoId`, `countryterritoryCode` | Geographical identifiers (some missing) |
| `popData2019` | 2019 population estimate (some missing) |
| `continentExp` | Continent classification |

### Data quality notes

- 15 rows have negative `cases` and 2 rows have negative `deaths`. These are most likely reporting corrections. They were kept, so totals are net of corrections.
- `geoId` is missing for 109 rows, and `popData2019` for 64 rows.
- No exact duplicate rows were found, and no country-date combinations were duplicated.
- The July 2020 figures cover only 1 July, so July is not a complete monthly period.

## Tools and Technologies

- **Python 3**
- **Pandas:** data manipulation, aggregation and time series
- **NumPy:** numerical operations
- **Matplotlib** and **Seaborn:** visualisation
- **Jupyter Notebook:** writing and running the analysis

## Data Preparation

1. Inspected the data with `.head()`, `.info()` and `.shape`, and checked for missing values.
2. Converted `dateRep` to datetime with `pd.to_datetime(..., dayfirst=True, errors='coerce')`.
3. Converted numeric columns (`day`, `month`, `year`, `cases`, `deaths`, `popData2019`) with `pd.to_numeric(..., errors='coerce')`.
4. Standardised country and continent names by stripping whitespace.
5. Filled missing text values with each column's most common value (mode).
6. Filled missing `popData2019` values with the column mean.
7. Checked for zero population values, missing dates and duplicate country-date combinations.
8. Rechecked missing values after cleaning.

## Analysis

1. **Global trends:** daily totals, cumulative totals, a 7-day rolling average, and month-on-month percentage change.
2. **Country totals:** rankings by absolute cases and deaths.
3. **Population-adjusted analysis:** cases and deaths per 100,000 population; case fatality ratio (CFR = deaths ÷ cases × 100).
4. **Continental comparison:** totals, number of countries, and CFR by continent.
5. **Monthly trends:** totals by month and month-on-month change.
6. **Country daily peaks:** the highest single-day cases and deaths for each country.
7. **Stakeholder analysis:** population-tier comparisons, concentration of burden, and percentiles of per-capita rates. Rankings by rate include only countries with at least 1 million people, so very small populations do not dominate.

## Key Findings

### Global progression

- **Totals:** 10,446,353 reported cases and 511,037 reported deaths, a crude CFR of 4.89%.
- **Monthly peaks:** June 2020 recorded the most cases (4,244,686). April 2020 recorded the most deaths (189,116).
- **Monthly totals:**

| Month | Cases | Deaths |
|---|---|---|
| December 2019 | 27 | 0 |
| January 2020 | 9,799 | 213 |
| February 2020 | 75,397 | 2,708 |
| March 2020 | 714,451 | 35,784 |
| April 2020 | 2,335,452 | 189,116 |
| May 2020 | 2,893,612 | 141,172 |
| June 2020 | 4,244,686 | 136,316 |
| 1 July 2020 (one day) | 172,929 | 5,728 |

- **Deaths peaked before cases.** The 7-day average of deaths peaked at roughly 7,400 a day in mid-to-late April, fell to about 3,800 by late May, and rose again to about 4,800 by July. The 7-day average of cases reached about 175,000 a day by 1 July and was still rising. This pattern is consistent with deaths lagging infections, though the data cannot confirm the mechanism.

### Country totals

- The **United States** (2,634,432 cases; 127,410 deaths) and **Brazil** (1,402,041 cases; 59,594 deaths) recorded the highest cumulative reported cases.
- Other large totals: Russia (647,849 cases), India (585,493 cases; 17,400 deaths), and the United Kingdom (312,654 cases; 43,730 deaths).

### Population-adjusted patterns

- **Population size changes the comparison.** Qatar recorded the highest reported case rate (3,392.9 per 100,000) but only 4.0 deaths per 100,000. San Marino recorded the highest death rate (121.9 per 100,000).
- The USA reported about 25% of global cases in this dataset, while its population is about 4% of the combined population of the 209 countries with population data.
- India has the second-largest population in the dataset but a reported rate of 42.8 cases and 1.3 deaths per 100,000. China reports 5.9 cases per 100,000.

| Country | Total cases | Cases per 100k | Deaths per 100k | CFR (%) |
|---|---|---|---|---|
| USA | 2,634,432 | 800.6 | 38.7 | 4.8 |
| Brazil | 1,402,041 | 664.3 | 28.2 | 4.3 |
| India | 585,493 | 42.8 | 1.3 | 3.0 |
| China | 84,785 | 5.9 | 0.3 | 5.5 |
| Qatar | 96,088 | 3,392.9 | 4.0 | 0.1 |
| San Marino | 698 | 2,025.9 | 121.9 | 6.0 |

### Continents

- **America** reported the most cases (5,327,057) and deaths (252,485), about 49% of all deaths in the file.
- **Europe** reported 2,442,700 cases and 191,962 deaths. Its CFR, at about 7.9%, is the highest of any continent.
- **Asia** and **Africa** had CFRs of about 2.5%.
- Per 100,000 people, Europe and America each reported about 25 deaths. Asia and Africa each reported about 1.

### Case fatality ratios


- The overall CFR is 4.89%. Across countries, it varies widely.
- **Yemen** had the highest country-level CFR, at 26.94% (312 deaths from 1,158 cases). With so few cases, a small change in either count moves the ratio sharply, so this figure should be treated as unstable.
- Among countries with at least 1,000 cases, the highest CFRs were in France (18.1%), Belgium (15.9%), Italy (14.5%), Hungary (14.1%) and the United Kingdom (14.0%).

### Daily peaks

- The highest single-day case count was **54,771 in Brazil on 20 June 2020**.
- The highest single-day death count was **4,928 in the USA on 16 April 2020**.
- Most country-days recorded few or no cases or deaths. A small number of days were far larger, which is why daily distributions are shown on both raw and log scales.

### Stakeholder questions

**Q1. Which countries experienced significant reported health burdens?**

By total deaths: USA (127,410), Brazil (59,594), UK (43,730), Italy (34,767), France (29,843), Spain (28,355), Mexico (27,769), India (17,400), Iran (10,817) and Belgium (9,754).

By deaths per 100,000 (countries with at least 1 million people): Belgium (85.1), UK (65.6), Spain (60.4), Italy (57.6), Sweden (52.1), France (44.5), USA (38.7), Ireland (35.4), Netherlands (35.4) and Chile (30.0).

The two lists differ. India has the eighth-highest total but ranks far lower per person. The data does not explain why. Age structure, testing practice and reporting completeness are all plausible contributors.

**Q2. How did the reported burden differ between populous and less populous countries?**

| Population tier | Countries | Population (millions) | Deaths per 100k | Cases per 100k | Median deaths per 100k |
|---|---|---|---|---|---|
| Under 1M | 53 | 14 | 3.7 | 168.3 | 1.9 |
| 1M–10M | 66 | 314 | 4.0 | 224.6 | 1.7 |
| 10M–50M | 61 | 1,409 | 6.9 | 138.0 | 0.6 |
| 50M–100M | 15 | 1,020 | 13.7 | 158.8 | 4.5 |
| Over 100M | 14 | 4,912 | 5.3 | 125.2 | 1.2 |

- The most populous group (over 100M, including China, India and the USA) reports a below-average death rate. Its aggregate rate is 5.3 per 100,000, and its median is 1.2.
- The 50M–100M group has the highest aggregate death rate (13.7 per 100,000). It includes Italy, France, the UK and Germany.
- This contrast is consistent with reporting differences as much as with differences in burden.

**Q3. What does the distribution suggest about targeted interventions?**

- **Burden is concentrated.** The 10 countries with the most reported deaths account for 76.2% of all deaths in the dataset. The top 20 account for 89.5%.
- **Rates are highly uneven.** Among countries with at least 1 million people, the median death rate is 1.1 per 100,000. The 75th percentile is 5.1, and the 90th is 17.5.
- **Many countries report almost nothing.** 27 countries recorded zero deaths. Zero is not evidence of no burden, since it may reflect limited testing or incomplete reporting.

Implications:

- A small number of countries account for most reported deaths, so concentrated support could reach much of the reported burden.
- High-rate countries may need clinical capacity and protection of vulnerable groups. Low-rate countries may need surveillance and testing before their true burden can be judged.
- Support decisions should not rest on reported figures alone.

**Q4. What additional socioeconomic information would be needed to assess vulnerability?**

The dataset contains only cases, deaths and population. It cannot measure vulnerability on its own. Useful additions would include:

- **Economic capacity:** GDP per capita, poverty rate, informal employment share
- **Health system capacity:** hospital and ICU beds per 1,000 people, health workforce density, public health expenditure per capita
- **Testing and surveillance:** tests per 1,000 people, testing positivity rate, reporting completeness
- **Demographic vulnerability:** share aged 65 and over, prevalence of non-communicable diseases, child population share (relevant to UNICEF)
- **Living conditions:** population density, urbanisation, household crowding, access to water and sanitation
- **Social protection:** health insurance and social protection coverage
- **Development context:** Human Development Index (HDI) or equivalent composite indicators
- **Disaggregated data:** cases and deaths by age, sex and region, where available

## Business Recommendations

### Stakeholder priorities

These priorities come from each agency's strategic framework for 2022–2025. Confirm the current wording against the source documents before publishing.

| Stakeholder | Stated priorities (strategic framework, 2022–2025) | How this analysis informs them |
|---|---|---|
| **UNDP** | Six signature solutions: poverty and inequality; governance; resilience (crisis prevention and recovery); environment and climate change; energy; gender equality. Source: [UNDP Strategic Plan 2022–2025](https://strategicplan.undp.org/2022-2025) | Identifies where the reported burden was heaviest and where recovery and resilience support may be needed (Q1, Q3). Its dependence on testing and reporting shows where capacity is weakest (Q4). |
| **UNICEF** | Goal areas: every child survives and thrives; every child learns; every child lives in a safe and clean environment; every child is protected from violence and exploitation; every child has an equitable chance in life. Source: [UNICEF Strategic Plan 2022–2025](https://www.unicef.org/reports/unicef-strategic-plan-2022-2025) | Shows that the dataset has no age-disaggregated figures, so child-specific burden cannot be assessed (Q4). Reported burden across countries highlights where child health and continuity of services may be most at risk (Q1, Q2). |
| **International development agencies** | Commonly: health system strengthening, surveillance and data systems, equity, pandemic preparedness and social protection. [Add the specific agency's current priorities.] | Supports targeting where reporting is weakest and where rates and totals diverge (Q2, Q3). |

Analytical recommendations for these stakeholders follow.

- **Use rates and totals together.** Totals identify where absolute need is largest. Rates identify where burden was heaviest per person.
- **Do not treat reported figures as complete.** Low or zero counts may reflect limited testing. Invest in surveillance and testing where reporting is weakest before drawing conclusions.
- **Read CFR as a reported ratio, not a fixed risk.** Ratios based on small case counts, such as Yemen's, are unstable.
- **Prioritise vulnerability data.** The indicators listed under Q4 are the next step from describing burden to targeting support.
- **Examine high-rate and low-rate countries separately.** They may need different responses.
- **Extend the analysis beyond July 2020** to see whether the patterns persisted.

## Limitations

- **Time coverage:** the data covers 31 December 2019 to 1 July 2020 only. It cannot describe later waves, vaccination or variants.
- **Reporting dependence:** reported cases differ from actual infections because testing capacity and reporting practices varied across countries. Counts are not a complete measure of infection or mortality.
- **Case fatality:** CFR is reported deaths divided by reported cases. It is not the probability of death after infection, and it is unstable when case counts are small.
- **Incomplete final month:** July 2020 includes only 1 July.
- **Data quality:** negative values were kept as corrections without verification. Missing population values were filled with the mean, which may affect per-capita figures for those countries.
- **Vulnerability:** the dataset has no socioeconomic, age or health-system information, so it cannot measure vulnerability directly.
- **Statistical scope:** correlations use raw counts, so large countries carry strong influence. They describe the sample and do not establish cause.
- **Interpretation:** the analysis identifies patterns and associations, not causes. Explanations for specific patterns are hypotheses.

## What I Learned

Data analysis involves more than writing code or producing summaries. It means asking meaningful questions, preparing data, identifying patterns, interpreting findings and recognising the limits of the evidence. The same dataset can tell different stories depending on the question asked and the method used. Presenting totals without population context, or rates without testing context, can each lead to a misleading conclusion.

## Resources

- [WHO COVID-19 data and methodology](https://data.who.int/dashboards/covid19/data): how data are collected and their limitations.
- [Our World in Data COVID-19 dataset documentation](https://github.com/owid/covid-19-data/blob/master/public/data/README.md): the structure of global COVID-19 indicators.
- [WHO COVID-19 surveillance guidance](https://cdn.who.int/media/docs/default-source/2021-dha-docs/who-2019-ncov-surveillanceguidance-2020.8-eng.pdf): the principles of public health surveillance.


## Key Skills Demonstrated

- **Data cleaning:** date parsing, numeric coercion, missing-value handling, standardisation of text fields
- **Aggregation and time series:** group-by totals, cumulative totals, rolling averages, month-on-month change
- **Derived indicators:** per-capita rates, case fatality ratio, population tiers
- **Distributional analysis:** percentiles, concentration of burden, handling of skewed data
- **Exploratory data analysis:** correlation analysis, interpretation of contrasting indicators
- **Data visualisation:** Matplotlib and Seaborn charts for temporal, geographical and distributional patterns
- **Analytical communication:** structuring findings around stakeholder questions, separating description from explanation, and stating limitations

---

**Author:** Tobiloba Aiyelero
**Background:** Political Science, with a growing focus on data science and evidence-based decision-making
**Field of interest:** Data Analytics | Data Science | Evidence-Based Decision Making
**Contact:** www.linkedin.com/in/aiyelero-tobiloba-5a43422a3
