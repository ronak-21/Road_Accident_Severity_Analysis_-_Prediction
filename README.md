# Road_Accident_Severity_Analysis_-_Prediction
Road accident severity analysis and prediction in Bangladesh using Power BI and Orange Data Mining — descriptive dashboards, ML classification, clustering, and association rule mining on 10M GPS traffic records.


## Overview

Road traffic injuries are among the top causes of death and disability in Bangladesh, where vehicle growth has far outpaced infrastructure and enforcement capacity. This project builds a two-part Business Intelligence system on the **BDRoadRisk dataset** — 10 million GPS-tracked traffic events along the Dhaka–Chattogram corridor — to move road safety analysis from reactive to evidence-based and predictive.

The system has two components:

1. **Descriptive analytics** — six interactive Power BI dashboards covering time, speed, driver/vehicle profile, environment, severity, and geography.
2. **Predictive analytics** — four Orange Data Mining workflows: multi-class classification, feature importance ranking, K-Means clustering, and association rule mining.

## Key Findings

- **Night driving is the most dangerous period** — 11,128 accidents occurred at night (more than any other time), with the highest share of Major outcomes (30.4%).
- **Counterintuitive road/weather patterns** — accidents are more frequent on *dry* roads (39.48%) than wet roads (35.35%), and accident frequency peaks at *moderate* visibility (4–6 km) rather than the lowest visibility. This suggests driver overconfidence, not road/weather conditions, is the stronger risk driver.
- **Vehicle type barely affects severity** — motorcycles account for 40.10% of all accidents, but the Major-accident rate across all vehicle types sits in a narrow 25.9%–28.4% band.
- **Strong, safety-conscious classification models** — Random Forest and Logistic Regression both reached AUC > 0.96 and MCC > 0.73. Critically, **neither model ever classified a real Major accident as "No Accident"** — the most important property for a safety decision-support system.
- **Visibility is the single strongest predictor of severity** (Random Forest importance: 0.321), ahead of reaction time (0.174) and speed (0.102).
- **Three driver risk archetypes** emerged from K-Means clustering, with the highest-risk group defined by high speed, slow reaction time, and younger age.
- **Association rule mining** found that seatbelt compliance and good road quality co-occur in 65% of records, and clear weather is strongly linked to dry road surfaces (lift = 1.433).


> **Note:** `traffic_data_predictive_analysis.xlsx` is a large file (~80 MB), tracked via [Git LFS](https://git-lfs.com/). If cloning, make sure Git LFS is installed first: `git lfs install`.

## Dataset

**Source:** [BDRoadRisk: Large-Scale GPS Traffic Accident Severity Dataset](https://doi.org/10.17632/M33BSBSGX2.3) (Dewanjee & Muntaha, 2026), Mendeley Data.

- 10 million GPS-tracked motorway traffic records, Dhaka–Chattogram corridor (22.25°N–23.90°N, 90.33°E–91.90°E)
- 24 variables covering driver behavior, vehicle characteristics, road/environmental conditions, and severity outcome
- Target variable: `accident_severity` — `No_Accident`, `Minor`, `Major`
- **Descriptive subset:** 32,055 confirmed accident records (Minor/Major only), used to study accident patterns in isolation
- **Predictive subset:** stratified 70/30 train-test split on the full dataset, retaining all three severity classes

## Methodology

### Data Preparation
- Removed 4 low-quality/irrelevant variables (`alcohol_influence`, `junction_type`, `road_class`, `month`)
- Assigned `latitude`/`longitude` as geographic metadata; 14 remaining columns used as predictive features
- Stratified 70/30 train-test split preserving class proportions
- Normalized numeric features to [0,1] for K-Means clustering
- Discretized continuous variables into Low/Medium/High bins for Apriori association rule mining

### Data Warehouse
A star schema (`accident_fact` fact table with `dim_driver`, `dim_vehicle`, `dim_road`, `dim_environment` dimension tables) designed to support efficient Power BI cross-filtering and future real-time sensor integration.

### Tools
| Tool | Purpose |
|---|---|
| Microsoft Excel | Data storage, cleaning, and source for Power BI/Orange |
| Microsoft Power BI Desktop | 6 interactive descriptive dashboards |
| Orange Data Mining | Classification (Random Forest, Logistic Regression), feature importance, K-Means clustering, association rule mining (Apriori) |

## Descriptive Dashboards (Power BI)

1. **Executive Overview** — top-line KPIs: 32K accidents, 27.56% Major, avg. speed 118.07 km/h, seatbelt usage 55.53%
2. **Speed and Traffic Analysis** — speed bands, traffic density impact, reaction time distribution
3. **Driver and Vehicle Analysis** — age, experience level, seatbelt usage, vehicle type breakdown
4. **Environment and Road Analysis** — weather, road surface, light condition, visibility patterns
5. **Severity Insights** — high-risk condition combinations (speed zone × lighting × road surface × weather)
6. **Geographic and Hotspot Analysis** — GPS-based accident mapping and hotspot identification

## Predictive Models (Orange Data Mining)

### Classification Performance
| Model | AUC | CA | F1 | MCC |
|---|---|---|---|---|
| Random Forest | 0.961 | 0.937 | 0.933 | 0.730 |
| Logistic Regression | 0.966 | 0.937 | 0.934 | 0.732 |

### Feature Importance (Top 5, Random Forest)
1. Visibility (0.321)
2. Reaction time (0.174)
3. Speed (0.102)
4. Traffic density (0.087)
5. Driver age (0.040)

### Driver Risk Archetypes (K-Means, k=3)
- **High Risk:** high speed, slow reaction time, younger age
- **Moderate Risk:** mid-range on all dimensions
- **Low Risk:** lower speed, fast reaction time, more mature age

### Association Rule Mining
4,955 rules generated (Apriori, min. support 0.05, min. confidence 0.70). Top patterns link seatbelt compliance, road quality, and weather/road-surface co-occurrence.

## Policy Recommendations

1. **Targeted road maintenance and lighting** at GPS-identified high-risk, high-speed, poorly-lit sections
2. **Speed enforcement and graduated licensing** aimed at younger, high-speed drivers
3. **Visibility-triggered variable speed limits** during fog and storm conditions

## Limitations

- Dataset shows statistical characteristics consistent with **synthetic generation** rather than real-world recorded accidents — findings should be read as a demonstration of analytical approach, not validated real-world risk estimates
- Scope limited to **motorway conditions only** (not urban roads, rural highways, or intersections)
- **Partial temporal coverage** — January–June only, missing the monsoon season (July–September)
- **Class imbalance** — Major accidents represent only 4.42% of the full dataset
- Association rules identify co-occurrence patterns among contextual variables, not direct severity causation

## References

- Agrawal, R., Imieliński, T., & Swami, A. (1993). Mining association rules between sets of items in large databases. *ACM SIGMOD*.
- Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5–32.
- Demšar, J., et al. (2013). Orange: Data Mining Toolbox in Python. *JMLR*, 14(71), 2349–2353.
- Dewanjee, S., & Muntaha, S. (2026). BDRoadRisk: Large-Scale GPS Traffic Accident Severity Dataset [Dataset]. Mendeley Data.
- World Health Organization. (2023). *Global Status Report on Road Safety 2023* (1st ed.).

## Authors

- **Ronak Tasnim Kotha** 
- **Tanveer Ahmed** 

Submitted to Rafifa Islam, Lecturer, Department of Business Administration, East Delta University.
